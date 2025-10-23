# Azure QuickView - Monitoring Dashboard

A comprehensive, responsive monitoring dashboard for Azure resources that provides quick insights into subscription health, cost analysis, and actionable remediation workflows.

![Azure QuickView Dashboard](https://img.shields.io/badge/Azure-QuickView-0078D4?style=for-the-badge&logo=microsoft-azure)

## 🎯 Features

### Core Functionality
- **Multi-Subscription Support**: Switch between Azure subscriptions with a single click
- **Advanced Filtering**: Multi-select dashboard filters to focus on specific problem areas
- **Summary Metrics**: At-a-glance tiles showing Total VMs, Untagged VMs, Running VMs, and Stopped VMs
- **Resource Management**: 
  - Identify and remediate untagged virtual machines
  - Track unattached resources (disks, NICs, public IPs, NSGs)
- **Azure Advisor Integration**: View and prioritize recommendations by severity (Critical, High, Medium, Low)
- **Runbook Automation**: Monitor scheduled runbooks and their execution status
- **Patch Compliance**: Track security posture with compliance percentages and non-compliant host details
- **Cost Analytics**: 
  - 3-month cost trend visualization
  - Top 10 cost drivers with percentage breakdown
- **Quick Actions**: One-click access to Azure Portal and remediation runbooks
- **Report Generation**: Download PDF summaries or email reports directly from the dashboard

### Dashboard Filters
Select one or more filters to show only relevant sections:
- Untagged Virtual Machines
- Unattached Resources (VM, Disk, NIC, etc.)
- Azure Advisor Recommendations
- Patch Management Summary
- Runbook Schedule Overview
- Cost Analysis (last 3 months)
- Top 10 Cost Drivers

**Filter Behavior**: 
- No filters selected = All sections visible
- One or more filters selected = Only matching sections shown
- Dashboard layout remains consistent regardless of filtering

## 🚀 Quick Start

### Prerequisites
- Node.js 18+ and npm
- Modern web browser
- (Optional) Azure subscription for connecting real data

### Installation

```bash
# Clone the repository
git clone <repository-url>
cd spark-template

# Install dependencies
npm install

# Start the development server
npm run dev
```

The dashboard will be available at `http://localhost:5173`

## 📊 60-90s Demo Script

### Introduction (10s)
"Azure QuickView is a comprehensive monitoring dashboard that gives you instant visibility into your Azure resources, costs, and recommendations."

### Core Features (30s)
1. **Subscription Selection**: "Switch between subscriptions using the dropdown. Watch as all data updates in real-time."
2. **Summary Tiles**: "Get instant metrics on your VMs - total count, untagged resources, running vs stopped states."
3. **Filtering**: "Use the Dashboard Filters to focus on specific areas. Select 'Azure Advisor Recommendations' to see only critical security and optimization suggestions."

### Key Workflows (30s)
4. **Resource Management**: "The Resource Management section shows untagged VMs and unattached resources. Click 'Portal' to open in Azure or 'Tag'/'Delete' to remediate directly."
5. **Cost Analysis**: "The cost chart shows spending trends over 3 months. Below, see your top 10 cost drivers with trend indicators."
6. **Patch Compliance**: "Monitor security posture with the Patch Management section showing compliance percentage and hosts needing updates."

### Reporting (10s)
7. **Monthly Summary**: "Click 'Send Monthly Summary' to download a PDF report or open an email with the report pre-attached."

### Wrap-up (10s)
"All actions are 1-2 clicks from tile to resolution. The dashboard uses mock data now but can be connected to real Azure APIs following the setup guide at the bottom."

## 🏗️ Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                         Frontend (React)                     │
│  ┌──────────────────────────────────────────────────────┐  │
│  │  Dashboard Header (Subscriptions, Filters, User)     │  │
│  ├──────────────────────────────────────────────────────┤  │
│  │  Summary Tiles (Metrics)                             │  │
│  ├──────────────────────────────────────────────────────┤  │
│  │  Resource Lists (VMs, Unattached Resources)          │  │
│  ├──────────────────────────────────────────────────────┤  │
│  │  Advisor Recommendations                             │  │
│  ├──────────────────────────────────────────────────────┤  │
│  │  Runbook Schedule & Patch Management                 │  │
│  ├──────────────────────────────────────────────────────┤  │
│  │  Cost Analysis (Chart + Top Drivers)                 │  │
│  └──────────────────────────────────────────────────────┘  │
└────────────────────┬────────────────────────────────────────┘
                     │
                     │ API Calls
                     ▼
┌─────────────────────────────────────────────────────────────┐
│               Mock API Layer (mockData.ts)                  │
│  ┌──────────────────────────────────────────────────────┐  │
│  │  - getSubscriptions()                                 │  │
│  │  - getDashboardData(subscriptionId)                   │  │
│  └──────────────────────────────────────────────────────┘  │
└────────────────────┬────────────────────────────────────────┘
                     │
                     │ (Replace with real Azure SDK)
                     ▼
┌─────────────────────────────────────────────────────────────┐
│                      Azure APIs                              │
│  ┌──────────────────────────────────────────────────────┐  │
│  │  - Azure Resource Manager (Subscriptions, VMs)       │  │
│  │  - Azure Advisor API (Recommendations)               │  │
│  │  - Azure Automation (Runbooks)                       │  │
│  │  - Azure Update Management (Patch Compliance)        │  │
│  │  - Azure Cost Management API (Cost Data)             │  │
│  └──────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────┘
```

## 🔌 Connecting to Real Azure APIs

### Step 1: Authentication Setup

**Option A: Service Principal (Recommended for Apps)**
```bash
# Create a service principal
az ad sp create-for-rbac --name "azure-quickview-sp" \
  --role "Reader" \
  --scopes /subscriptions/{subscription-id}

# Save the output (appId, password, tenant)
```

**Option B: Managed Identity (Recommended for Azure-hosted)**
```bash
# Assign Reader role to your app's managed identity
az role assignment create \
  --assignee {managed-identity-principal-id} \
  --role "Reader" \
  --scope /subscriptions/{subscription-id}
```

### Step 2: Install Azure SDKs

```bash
npm install @azure/identity \
  @azure/arm-subscriptions \
  @azure/arm-compute \
  @azure/arm-advisor \
  @azure/arm-automation \
  @azure/arm-costmanagement \
  @azure/arm-resources
```

### Step 3: Replace Mock API

Update `src/lib/mockData.ts` with real Azure SDK calls:

```typescript
import { DefaultAzureCredential } from '@azure/identity'
import { ComputeManagementClient } from '@azure/arm-compute'
import { AdvisorManagementClient } from '@azure/arm-advisor'
import { CostManagementClient } from '@azure/arm-costmanagement'

const credential = new DefaultAzureCredential()

export const azureApi = {
  getSubscriptions: async () => {
    const client = new SubscriptionClient(credential)
    const subscriptions = []
    for await (const sub of client.subscriptions.list()) {
      subscriptions.push(sub)
    }
    return subscriptions
  },

  getDashboardData: async (subscriptionId: string) => {
    const computeClient = new ComputeManagementClient(credential, subscriptionId)
    const advisorClient = new AdvisorManagementClient(credential, subscriptionId)
    // ... implement real API calls
  }
}
```

### Step 4: Environment Variables

Create `.env.local`:

```env
VITE_AZURE_TENANT_ID=your-tenant-id
VITE_AZURE_CLIENT_ID=your-client-id
VITE_AZURE_CLIENT_SECRET=your-client-secret
```

**Important**: Never commit secrets to version control. Use Azure Key Vault for production.

## 📁 Project Structure

```
src/
├── components/
│   ├── DashboardHeader.tsx      # Header with subscriptions, filters, user
│   ├── SummaryTiles.tsx         # Metric tiles (Total VMs, etc.)
│   ├── ResourcesList.tsx        # Untagged VMs and unattached resources
│   ├── AdvisorRecommendations.tsx # Azure Advisor recommendations
│   ├── RunbookSchedule.tsx      # Runbook jobs and schedules
│   ├── PatchManagement.tsx      # Patch compliance summary
│   ├── CostAnalysis.tsx         # Cost chart and top drivers
│   └── ui/                      # shadcn components (pre-installed)
├── lib/
│   ├── types.ts                 # TypeScript interfaces
│   ├── mockData.ts              # Mock API and sample data
│   ├── helpers.ts               # Utility functions
│   └── utils.ts                 # shadcn utilities
├── App.tsx                      # Main application component
└── index.css                    # Theme and global styles
```

## 🎨 Design System

### Colors
- **Primary**: Azure Blue `oklch(0.45 0.15 250)` - Main brand and actions
- **Secondary**: Sky Blue `oklch(0.75 0.10 240)` - Supporting elements
- **Accent**: Cyan `oklch(0.70 0.15 210)` - CTAs and highlights
- **Success**: Green for completed states and positive trends
- **Warning**: Orange/Yellow for warnings and critical counts
- **Destructive**: Red for errors and critical severity

### Typography
- **Font Family**: Inter (Google Fonts)
- **Hierarchy**:
  - H1: 32px SemiBold (Dashboard title)
  - H2: 24px SemiBold (Section headers)
  - Body: 14px Regular (Tables, lists)
  - Metrics: 36px Bold (Tile numbers)

### Components
Built with **shadcn/ui v4** and **Tailwind CSS**:
- Cards for section containers
- Tables for resource lists
- Badges for status indicators
- Buttons for actions (Portal, Remediate)
- Recharts for cost visualization

## 🔒 Security Considerations

1. **Credential Storage**: Use Azure Key Vault or environment variables, never hardcode
2. **RBAC Permissions**: Grant minimum required permissions (Reader role for most operations)
3. **API Rate Limiting**: Implement caching and throttling for Azure API calls
4. **Data Sanitization**: Validate and sanitize all data before rendering
5. **HTTPS Only**: Ensure production deployment uses HTTPS
6. **Authentication**: Integrate with Azure AD for production use

## 📦 Mock Data Structure

The dashboard includes comprehensive mock data in `src/lib/mockData.ts`:

### Subscriptions
- 3 sample subscriptions (Production, Development, Staging)

### Virtual Machines
- 7 VMs across different resource groups
- Mix of Running, Stopped, and Deallocated states
- Some with tags, some without (for untagged VM testing)

### Advisor Recommendations
- 6 recommendations with varying severity levels
- Categories: Security, Cost, Performance, Reliability, Operational Excellence

### Runbook Jobs
- 5 scheduled runbooks with different frequencies
- Status: Running, Completed, Failed

### Patch Compliance
- 57.1% compliance rate
- 3 non-compliant hosts with patch details

### Cost Data
- 90 days of cost history (3 months)
- 10 top cost drivers with trends

## 🧪 Testing

```bash
# Run type checking
npm run type-check

# Build for production
npm run build

# Preview production build
npm run preview
```

## 📱 Mobile Responsiveness

- **Desktop (>1280px)**: 4-column tile grid, full tables
- **Tablet (768-1279px)**: 2-column tile grid, scrollable tables
- **Mobile (<768px)**: 1-column layout, card-based resource lists, drawer for filters

## 🛠️ Technologies Used

- **React 19** - UI framework
- **TypeScript** - Type safety
- **Vite** - Build tool and dev server
- **Tailwind CSS v4** - Styling
- **shadcn/ui v4** - Component library
- **Recharts** - Data visualization
- **Phosphor Icons** - Icon library
- **Sonner** - Toast notifications

## 📄 License

This project is licensed under the MIT License.

## 🤝 Contributing

Contributions are welcome! Please follow these steps:
1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## 📧 Support

For issues or questions:
- Create an issue in the repository
- Contact the development team
- Review the Azure SDK documentation

---

**Built with ❤️ for Azure administrators and DevOps teams**
