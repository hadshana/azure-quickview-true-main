# Azure QuickView - Demo Instructions

## 60-Second Quick Demo

### 1. Introduction (5 seconds)
"Welcome to Azure QuickView - your comprehensive monitoring dashboard for Azure resources."

### 2. Overview (10 seconds)
"At the top, you can select between different Azure subscriptions. Watch as I switch from Production to Development - all data updates instantly."

**Action**: Switch subscription dropdown

### 3. Summary Tiles (10 seconds)
"These tiles give you instant visibility: Total VMs, Untagged resources, Running VMs, and Stopped VMs. Click any tile to jump to details."

**Action**: Hover over tiles, click one

### 4. Dashboard Filtering (15 seconds)
"The Dashboard Filters dropdown lets you focus on specific areas. Let me select 'Azure Advisor Recommendations' - now only that section shows."

**Action**: 
- Click "Dashboard Filters" dropdown
- Select "Azure Advisor Recommendations"
- Show how only that section appears
- Clear filter to show all

### 5. Key Features (15 seconds)
"Each section is actionable:"
- **Resource Lists**: "See untagged VMs - click 'Portal' to open in Azure, or 'Tag' to remediate"
- **Advisor Recommendations**: "Critical security issues are highlighted - one click to fix"
- **Cost Analysis**: "3-month spending trend with your top 10 cost drivers"

**Action**: Scroll through sections, click a Portal button

### 6. Reporting (5 seconds)
"Need to share? Click 'Send Monthly Summary' to download a PDF or email the report."

**Action**: Click the dropdown, show options

---

## 90-Second Extended Demo

Follow the 60-second script, then add:

### 7. Advanced Features (15 seconds)
- **Patch Management**: "See compliance at 57.1% - non-compliant hosts listed with missing patch counts"
- **Runbook Schedule**: "Monitor your automation - one job failed, I can click to investigate"

**Action**: Scroll to these sections

### 8. Integration & Setup (10 seconds)
"The dashboard uses mock data for demo purposes. Scroll to the bottom to see the integration guide for connecting to real Azure APIs."

**Action**: Scroll to integration section

### 9. Mobile & Responsive (5 seconds)
"Fully responsive - resize your browser to see mobile and tablet views."

**Action**: Resize browser or show mobile view

---

## Key Talking Points

### Benefits
- **1-2 Click Actions**: From problem identification to resolution
- **Multi-Subscription**: Manage multiple Azure environments from one view
- **Cost Visibility**: Identify spending trends and optimization opportunities
- **Security Focus**: Critical recommendations highlighted prominently
- **Automation Monitoring**: Track runbook health and patch compliance

### Technical Highlights
- Built with React, TypeScript, and Tailwind CSS
- Fully responsive (desktop, tablet, mobile)
- Ready for Azure SDK integration
- GitHub authentication built-in
- PDF and email report generation

### Use Cases
1. **Daily Health Check**: Quickly scan for critical issues across subscriptions
2. **Cost Review**: Weekly review of spending trends and top drivers
3. **Security Audit**: Identify unpatched systems and missing configurations
4. **Compliance Reporting**: Generate monthly summaries for stakeholders
5. **Automation Monitoring**: Track scheduled job success/failure rates

---

## Demo Tips

### Before Demo
1. Ensure app is running (`npm run dev`)
2. Have browser at full width (shows best layout)
3. Clear any previous filters
4. Start on Production subscription

### During Demo
1. **Move Smoothly**: Don't rush between sections
2. **Highlight Colors**: Point out severity badges (red = critical)
3. **Show Interactivity**: Hover over charts, click buttons
4. **Emphasize Speed**: "Everything updates in real-time"

### Common Questions & Answers

**Q: Does this work with our existing Azure setup?**
A: Yes! Follow the integration guide in the README to connect your Azure subscriptions via Service Principal or Managed Identity.

**Q: Can we customize the filters and sections?**
A: Absolutely. The component architecture makes it easy to add new sections or modify existing ones.

**Q: What permissions do we need in Azure?**
A: Minimum "Reader" role on the subscriptions you want to monitor. Some remediation actions may require additional permissions.

**Q: Can we schedule automatic reports?**
A: Currently manual, but you could extend this with a backend service to generate and email reports on a schedule.

**Q: How often does the data refresh?**
A: Currently on subscription change or page refresh. You can add auto-refresh functionality as a next step.

---

## Demo Scenarios

### Scenario 1: Security Audit
"Let's say I'm doing a security review. I'll filter to just Azure Advisor Recommendations and Patch Management. Here I see 2 critical security issues and 57% patch compliance. I can click 'Fix' on the encryption recommendation to start remediation."

### Scenario 2: Cost Optimization
"For monthly cost review, I filter to Cost Analysis and Top 10 Cost Drivers. The trend shows we're up 8% this month. Looking at the drivers, vm-db-01 is 35% of our spend and trending up. Let me click Portal to investigate right-sizing options."

### Scenario 3: Resource Cleanup
"To clean up waste, I'll view Untagged VMs and Unattached Resources. Here are 4 untagged VMs - I can apply tags directly. And 5 orphaned resources costing us money - one click to delete."

### Scenario 4: Automation Health Check
"Checking our automation: The Runbook Schedule shows one failed job - Backup-Configuration. I can click Portal to see the error logs and fix it before tonight's run."

---

## Demo Environment Setup

### Optimal Browser Window
- Width: 1920px (full HD monitor)
- Height: 1080px
- Zoom: 100%

### Test Data Highlights
- **Production subscription**: 7 VMs, 4 untagged, 6 recommendations
- **Critical issues**: 1 security (disk encryption), 1 high (OS update)
- **Failed runbook**: Backup-Configuration (easy to spot)
- **Patch compliance**: 57.1% (shows room for improvement)
- **Cost trend**: Up 8% (good for discussion)

---

**Good luck with your demo! This dashboard is designed to impress with its combination of comprehensive data and actionable insights.**
