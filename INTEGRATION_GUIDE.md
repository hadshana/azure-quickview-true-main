# Azure Integration Guide

This guide explains how to replace the mock data with real Azure API calls.

## Prerequisites

1. **Azure Subscription**: Active Azure subscription with resources
2. **Azure CLI**: Installed and authenticated (`az login`)
3. **Permissions**: At minimum, Reader role on subscriptions you want to monitor
4. **Node.js**: Version 18 or higher

## Step-by-Step Integration

### Step 1: Install Azure SDK Packages

```bash
npm install @azure/identity \
  @azure/arm-subscriptions \
  @azure/arm-compute \
  @azure/arm-resources \
  @azure/arm-advisor \
  @azure/arm-automation \
  @azure/arm-costmanagement
```

### Step 2: Set Up Authentication

#### Option A: Service Principal (Recommended for Applications)

1. **Create Service Principal**:
```bash
az ad sp create-for-rbac \
  --name "azure-quickview-sp" \
  --role "Reader" \
  --scopes /subscriptions/{YOUR_SUBSCRIPTION_ID}
```

2. **Save the output**:
```json
{
  "appId": "00000000-0000-0000-0000-000000000000",
  "displayName": "azure-quickview-sp",
  "password": "your-secret-password",
  "tenant": "00000000-0000-0000-0000-000000000000"
}
```

3. **Create `.env.local`**:
```env
VITE_AZURE_TENANT_ID=your-tenant-id
VITE_AZURE_CLIENT_ID=your-client-id
VITE_AZURE_CLIENT_SECRET=your-client-secret
```

#### Option B: Managed Identity (Azure-Hosted Apps)

If running on Azure (VM, App Service, Container Instance):

```bash
# Assign Reader role to the managed identity
az role assignment create \
  --assignee {managed-identity-object-id} \
  --role "Reader" \
  --scope /subscriptions/{subscription-id}
```

No environment variables needed - credentials are automatically available.

#### Option C: Azure CLI (Local Development)

For local development, just run `az login`:

```bash
az login
```

The SDK will use your CLI credentials automatically.

### Step 3: Create Azure API Service

Create `src/lib/azureApi.ts`:

```typescript
import { DefaultAzureCredential, ClientSecretCredential } from '@azure/identity'
import { SubscriptionClient } from '@azure/arm-subscriptions'
import { ComputeManagementClient } from '@azure/arm-compute'
import { ResourceManagementClient } from '@azure/arm-resources'
import { AdvisorManagementClient } from '@azure/arm-advisor'
import type { AzureSubscription, DashboardData, VirtualMachine } from './types'

// Choose credential based on environment
const getCredential = () => {
  if (import.meta.env.VITE_AZURE_CLIENT_ID) {
    // Use Service Principal
    return new ClientSecretCredential(
      import.meta.env.VITE_AZURE_TENANT_ID,
      import.meta.env.VITE_AZURE_CLIENT_ID,
      import.meta.env.VITE_AZURE_CLIENT_SECRET
    )
  }
  // Use default (Managed Identity or Azure CLI)
  return new DefaultAzureCredential()
}

const credential = getCredential()

export const azureApi = {
  getSubscriptions: async (): Promise<AzureSubscription[]> => {
    const client = new SubscriptionClient(credential)
    const subscriptions: AzureSubscription[] = []
    
    for await (const sub of client.subscriptions.list()) {
      subscriptions.push({
        id: sub.subscriptionId!,
        name: sub.displayName!,
        displayName: sub.displayName!,
      })
    }
    
    return subscriptions
  },

  getDashboardData: async (subscriptionId: string): Promise<DashboardData> => {
    const computeClient = new ComputeManagementClient(credential, subscriptionId)
    const resourceClient = new ResourceManagementClient(credential, subscriptionId)
    const advisorClient = new AdvisorManagementClient(credential, subscriptionId)

    // Fetch VMs
    const virtualMachines: VirtualMachine[] = []
    for await (const vm of computeClient.virtualMachines.listAll()) {
      const instanceView = await computeClient.virtualMachines.instanceView(
        vm.id!.split('/')[4], // resource group name
        vm.name!
      )
      
      virtualMachines.push({
        id: vm.id!,
        name: vm.name!,
        resourceGroup: vm.id!.split('/')[4],
        status: instanceView.statuses?.find(s => s.code?.startsWith('PowerState/'))
          ?.displayStatus === 'VM running' ? 'Running' : 'Stopped',
        tags: vm.tags || {},
        lastModified: vm.timeCreated?.toISOString() || '',
        location: vm.location!,
      })
    }

    // Fetch unattached resources
    const unattachedResources = []
    for await (const disk of computeClient.disks.list()) {
      if (!disk.managedBy) {
        unattachedResources.push({
          id: disk.id!,
          name: disk.name!,
          type: 'Disk' as const,
          resourceGroup: disk.id!.split('/')[4],
          lastModified: disk.timeCreated?.toISOString() || '',
          size: `${disk.diskSizeGB} GB`,
          location: disk.location!,
        })
      }
    }

    // Fetch Advisor recommendations
    const advisorRecommendations = []
    for await (const rec of advisorClient.recommendations.list()) {
      advisorRecommendations.push({
        id: rec.id!,
        vmName: rec.impactedValue || 'Unknown',
        category: rec.category as any,
        severity: rec.impact as any,
        title: rec.shortDescription?.problem || '',
        description: rec.shortDescription?.solution || '',
        impactedResource: rec.impactedValue || '',
        resourceGroup: rec.id!.split('/')[4],
      })
    }

    // Calculate summary
    const untaggedVMs = virtualMachines.filter(vm => Object.keys(vm.tags).length === 0)
    const runningVMs = virtualMachines.filter(vm => vm.status === 'Running')
    const stoppedVMs = virtualMachines.filter(vm => vm.status !== 'Running')

    // TODO: Implement other data fetching (runbooks, patch compliance, cost analysis)
    // These require additional Azure services (Automation, Update Management, Cost Management)

    return {
      subscriptionId,
      summary: {
        totalVMs: virtualMachines.length,
        untaggedVMs: untaggedVMs.length,
        runningVMs: runningVMs.length,
        stoppedVMs: stoppedVMs.length,
      },
      virtualMachines,
      unattachedResources,
      advisorRecommendations,
      runbookJobs: [], // TODO: Implement with @azure/arm-automation
      patchCompliance: {
        totalHosts: 0,
        compliantHosts: 0,
        compliancePercentage: 0,
        nonCompliantHosts: [],
      }, // TODO: Implement with Update Management API
      costAnalysis: {
        history: [],
        topDrivers: [],
        totalCost: 0,
        previousMonthCost: 0,
        trend: 'stable',
      }, // TODO: Implement with @azure/arm-costmanagement
    }
  },
}
```

### Step 4: Update App.tsx

Replace the import in `src/App.tsx`:

```typescript
// Change this:
import { mockApi } from '@/lib/mockData'

// To this:
import { azureApi as api } from '@/lib/azureApi'
```

Then update the API calls:

```typescript
// Change:
const subs = await mockApi.getSubscriptions()

// To:
const subs = await api.getSubscriptions()

// And:
const data = await mockApi.getDashboardData(selectedSubscription)

// To:
const data = await api.getDashboardData(selectedSubscription)
```

### Step 5: Additional Azure Services

#### Runbook Jobs (Azure Automation)

```typescript
import { AutomationClient } from '@azure/arm-automation'

const automationClient = new AutomationClient(credential, subscriptionId)

// List automation accounts
for await (const account of automationClient.automationAccount.list()) {
  // List runbooks in each account
  const runbooks = automationClient.runbook.listByAutomationAccount(
    account.resourceGroup,
    account.name
  )
  
  // Get job schedules
  const schedules = automationClient.jobSchedule.listByAutomationAccount(
    account.resourceGroup,
    account.name
  )
}
```

#### Patch Compliance (Update Management)

Update Management uses Azure Automation and Log Analytics. Query the Log Analytics workspace:

```typescript
import { LogsQueryClient } from '@azure/monitor-query'

const logsClient = new LogsQueryClient(credential)

const query = `
  Update
  | where TimeGenerated > ago(7d)
  | summarize 
      TotalUpdates = count(),
      MissingUpdates = countif(UpdateState == "Needed")
    by Computer
`

const result = await logsClient.queryWorkspace(
  workspaceId,
  query,
  { duration: 'P7D' }
)
```

#### Cost Management

```typescript
import { CostManagementClient } from '@azure/arm-costmanagement'

const costClient = new CostManagementClient(credential)

const scope = `/subscriptions/${subscriptionId}`
const query = {
  type: 'ActualCost',
  timeframe: 'MonthToDate',
  dataset: {
    granularity: 'Daily',
    aggregation: {
      totalCost: { name: 'PreTaxCost', function: 'Sum' }
    }
  }
}

const result = await costClient.query.usage(scope, query)
```

## Security Best Practices

### 1. Never Commit Secrets

Add to `.gitignore`:
```
.env.local
.env.production.local
*.key
*.pem
```

### 2. Use Azure Key Vault (Production)

```typescript
import { SecretClient } from '@azure/keyvault-secrets'

const vaultUrl = `https://${vaultName}.vault.azure.net`
const secretClient = new SecretClient(vaultUrl, credential)

const secret = await secretClient.getSecret('azure-quickview-client-secret')
const clientSecret = secret.value
```

### 3. Rotate Credentials Regularly

```bash
# Create new secret for service principal
az ad sp credential reset --id {app-id}
```

### 4. Use Managed Identity When Possible

No secrets to manage - Azure handles authentication automatically.

### 5. Implement Rate Limiting

```typescript
// Add retry logic with exponential backoff
import { delay } from '@azure/core-util'

async function retryableRequest<T>(fn: () => Promise<T>, maxRetries = 3): Promise<T> {
  for (let i = 0; i < maxRetries; i++) {
    try {
      return await fn()
    } catch (error: any) {
      if (error.statusCode === 429 && i < maxRetries - 1) {
        await delay(Math.pow(2, i) * 1000)
        continue
      }
      throw error
    }
  }
  throw new Error('Max retries exceeded')
}
```

## Caching Strategy

To avoid excessive API calls:

```typescript
import { useKV } from '@github/spark/hooks'

// Cache data for 5 minutes
const CACHE_TTL = 5 * 60 * 1000

export function useCachedDashboardData(subscriptionId: string) {
  const [cached, setCached] = useKV(`dashboard-${subscriptionId}`, null)
  const [loading, setLoading] = useState(false)

  useEffect(() => {
    const fetchData = async () => {
      if (cached && Date.now() - cached.timestamp < CACHE_TTL) {
        return // Use cached data
      }

      setLoading(true)
      const data = await azureApi.getDashboardData(subscriptionId)
      setCached({ data, timestamp: Date.now() })
      setLoading(false)
    }

    fetchData()
  }, [subscriptionId])

  return { data: cached?.data, loading }
}
```

## Testing Azure Integration

### 1. Test Authentication

```typescript
const testAuth = async () => {
  try {
    const subs = await azureApi.getSubscriptions()
    console.log(`✓ Successfully authenticated. Found ${subs.length} subscriptions.`)
  } catch (error) {
    console.error('✗ Authentication failed:', error)
  }
}
```

### 2. Test API Permissions

```bash
# Verify Reader role
az role assignment list --assignee {service-principal-id} --scope /subscriptions/{subscription-id}
```

### 3. Monitor API Usage

Check Azure portal for API request metrics and throttling events.

## Troubleshooting

### Error: "No subscriptions found"

**Cause**: Service Principal doesn't have access to any subscriptions

**Fix**:
```bash
az role assignment create \
  --assignee {service-principal-id} \
  --role "Reader" \
  --scope /subscriptions/{subscription-id}
```

### Error: "Authentication failed"

**Cause**: Invalid credentials or expired secret

**Fix**:
1. Verify environment variables are set correctly
2. Regenerate service principal secret:
```bash
az ad sp credential reset --id {app-id}
```

### Error: "Rate limit exceeded"

**Cause**: Too many API requests

**Fix**: Implement caching and request throttling (see Caching Strategy section)

### Error: "Insufficient permissions"

**Cause**: Service Principal needs additional roles for certain operations

**Fix**:
```bash
# For cost data, add Cost Management Reader
az role assignment create \
  --assignee {service-principal-id} \
  --role "Cost Management Reader" \
  --scope /subscriptions/{subscription-id}

# For advisor recommendations
az role assignment create \
  --assignee {service-principal-id} \
  --role "Advisor Recommendations Contributor" \
  --scope /subscriptions/{subscription-id}
```

## Performance Optimization

1. **Parallel Requests**: Fetch independent data in parallel
```typescript
const [vms, disks, recommendations] = await Promise.all([
  computeClient.virtualMachines.listAll(),
  computeClient.disks.list(),
  advisorClient.recommendations.list(),
])
```

2. **Pagination**: Handle large result sets properly
3. **Selective Fetching**: Only fetch data for visible sections
4. **Background Refresh**: Update cache in background

## Next Steps

1. ✅ Set up authentication
2. ✅ Replace mock subscription list
3. ✅ Replace VM and resource data
4. ✅ Implement Advisor recommendations
5. ⬜ Add Automation/Runbook integration
6. ⬜ Add Update Management integration
7. ⬜ Add Cost Management integration
8. ⬜ Implement caching strategy
9. ⬜ Add error handling and retry logic
10. ⬜ Deploy to Azure (App Service / Static Web App)

## Support

- **Azure SDK Documentation**: https://docs.microsoft.com/azure/developer/javascript/
- **Service Principal Setup**: https://docs.microsoft.com/cli/azure/create-an-azure-service-principal-azure-cli
- **Managed Identity**: https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/

---

**Remember**: Always follow the principle of least privilege - grant only the minimum permissions needed for the dashboard to function.
