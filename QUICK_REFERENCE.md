# Azure QuickView - Quick Reference

## Dashboard Sections

| Section | What It Shows | Key Actions |
|---------|--------------|-------------|
| **Summary Tiles** | VM counts, statuses | Click tile to focus on that resource type |
| **Resource Management** | Untagged VMs, orphaned resources | Tag VMs, Delete unused resources |
| **Advisor Recommendations** | Security, cost, performance tips | Fix critical issues, optimize resources |
| **Runbook Schedule** | Automation job statuses | Investigate failures, view schedules |
| **Patch Management** | Security compliance % | Schedule patches, view non-compliant hosts |
| **Cost Analysis** | 3-month spending trends | Identify cost spikes, optimize top drivers |
| **Top 10 Cost Drivers** | Highest spending resources | Review and right-size expensive resources |

## Filtering

### Available Filters
1. Untagged Virtual Machines
2. Unattached Resources (VM, Disk, NIC, etc.)
3. Azure Advisor Recommendations
4. Patch Management Summary
5. Runbook Schedule Overview
6. Cost Analysis (last 3 months)
7. Top 10 Cost Drivers

### How to Filter
1. Click **"Dashboard Filters"** dropdown in header
2. Check one or more filter options
3. Dashboard updates immediately
4. Uncheck to remove filter
5. Click **"Clear all filters"** to reset

### Filter Rules
- **No filters selected** = All sections visible
- **One or more filters** = Only selected sections show
- Summary tiles hidden when filters active
- Layout stays consistent

## Common Workflows

### 1. Security Audit
```
Filter → Azure Advisor Recommendations
Look for: Critical and High severity badges
Action: Click "Fix" button → Follow remediation steps
```

### 2. Cost Review
```
Filter → Cost Analysis + Top 10 Cost Drivers
Look for: Upward trend arrows, high percentage resources
Action: Click "Portal" → Review and right-size resources
```

### 3. Resource Cleanup
```
Filter → Untagged VMs + Unattached Resources
Look for: Empty tags column, long-unused resources
Action: Click "Tag" or "Delete" → Confirm remediation
```

### 4. Compliance Check
```
Filter → Patch Management Summary
Look for: Compliance % below 90%, critical patches count
Action: Click "Patch" → Schedule maintenance window
```

### 5. Automation Health
```
Filter → Runbook Schedule Overview
Look for: Red "Failed" badges
Action: Click "Portal" → View error logs and fix
```

## Status Indicators

### VM Status
| Badge | Meaning | Color |
|-------|---------|-------|
| Running | VM is active | Green |
| Stopped | VM is stopped but allocated | Gray |
| Deallocated | VM is deallocated (not charged) | Gray |

### Job Status
| Badge | Meaning | Color |
|-------|---------|-------|
| ✓ Completed | Job finished successfully | Green |
| ⚠ Running | Job is currently executing | Blue (animated) |
| ✗ Failed | Job encountered errors | Red |

### Advisor Severity
| Badge | Meaning | Action Priority |
|-------|---------|-----------------|
| Critical | Security risk or major issue | Fix immediately |
| High | Significant impact | Fix within 1 week |
| Medium | Moderate impact | Plan for next sprint |
| Low | Minor optimization | Address when convenient |

### Recommendation Categories
| Category | Icon | Focus Area |
|----------|------|------------|
| Security | 🛡️ | Encryption, updates, access control |
| Cost | 💰 | Right-sizing, unused resources |
| Performance | ⚡ | Throughput, latency, optimization |
| Reliability | ✅ | Backup, redundancy, availability |
| Operational Excellence | 🔧 | Best practices, automation |

## Action Buttons

### Open in Portal
- Opens Azure Portal to the specific resource
- New browser tab
- Requires Azure login

### Remediate Actions
| Button | Section | What It Does |
|--------|---------|--------------|
| **Tag** | Untagged VMs | Apply default tags via runbook |
| **Delete** | Unattached Resources | Remove unused resource |
| **Fix** | Advisor Recommendations | Start remediation workflow |
| **Patch** | Patch Management | Schedule update deployment |

## Report Generation

### Download PDF
```
1. Click "Send Monthly Summary" in header
2. Select "Download PDF"
3. PDF downloads automatically
4. Contains all current dashboard data
```

### Email Report
```
1. Click "Send Monthly Summary" in header
2. Select "Email Report"
3. Outlook opens with pre-populated email
4. Report attached automatically
5. Add recipients and send
```

## Keyboard Shortcuts

| Key | Action |
|-----|--------|
| `Tab` | Navigate between elements |
| `Enter` | Activate focused button |
| `Esc` | Close open dropdown/dialog |
| `Space` | Toggle checkbox/select |

## Tips & Tricks

### 1. Quick Subscription Switch
Use the subscription dropdown frequently - data updates instantly without page reload.

### 2. Focus on Problems
Apply filters to hide "good" sections and focus only on issues needing attention.

### 3. Tile Clicks
Click summary tiles to quickly jump to detailed resource lists.

### 4. Sort Tables
Click column headers in tables to sort by that field (where available).

### 5. Hover for Details
Hover over chart points, badges, and truncated text for more information.

### 6. Mobile View
On mobile, tables become horizontally scrollable - swipe to see all columns.

### 7. Multiple Actions
You can open multiple Portal tabs to compare resources side-by-side.

### 8. Filter Combinations
Combine filters to create custom views (e.g., "Advisor + Patch Management" for security focus).

## Troubleshooting

### Dashboard Not Loading
- Check browser console for errors
- Verify subscription exists in mock data
- Refresh the page

### Data Looks Stale
- Switch subscriptions and back
- Check that mock API is being called
- Look for error toasts

### Buttons Not Working
- Check browser console for JavaScript errors
- Ensure ad blockers aren't interfering
- Try different browser

### Portal Links Don't Open
- Check popup blocker settings
- Ensure "Open in Portal" button was clicked
- Verify Azure Portal URL format

## Performance Tips

### For Large Subscriptions
1. Use filters to show only needed sections
2. Limit date ranges (when feature added)
3. Consider pagination for large lists

### For Slow Connections
1. Data is cached after first load
2. Subsequent navigation is fast
3. Images and icons are optimized

## Best Practices

### Daily Use
1. Check summary tiles for quick health overview
2. Review failed runbooks and address immediately
3. Monitor critical Advisor recommendations

### Weekly Review
1. Review cost trends and top drivers
2. Check patch compliance trends
3. Clean up unattached resources

### Monthly Tasks
1. Generate and distribute summary report
2. Review all Advisor recommendations
3. Tag cleanup and standardization
4. Cost optimization review

## Getting Help

### Resources
- **README.md** - Setup and overview
- **DEMO_SCRIPT.md** - Presentation guide
- **INTEGRATION_GUIDE.md** - Azure SDK integration
- **CHANGELOG.md** - Version history

### Contact
- Check GitHub issues
- Review documentation
- Contact development team

---

**Print this page for quick desk reference!**
