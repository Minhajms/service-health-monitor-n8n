# 🚀 Service Health Monitor with n8n

A robust, automated service health monitoring system built with n8n that provides real-time Slack notifications for critical service availability. Perfect for DevOps teams who need continuous monitoring without complex infrastructure.

![n8n Workflow](https://img.shields.io/badge/n8n-Workflow-orange)
![Slack Integration](https://img.shields.io/badge/Slack-Integration-green)
![Monitoring](https://img.shields.io/badge/Monitoring-Real--time-blue)
![No Code](https://img.shields.io/badge/No--Code-Friendly-purple)

## 📊 **Features**

- ✅ **Automated Health Monitoring** - Checks 3 critical services every 5 minutes
- ✅ **Real-time Slack Notifications** - Instant alerts for service status changes
- ✅ **Multi-Service Support** - Stock Check, OMS Posting, and OMS Pulling services
- ✅ **Smart Routing** - Different message types for healthy vs unhealthy services
- ✅ **Zero Infrastructure** - No servers or databases required
- ✅ **Beginner Friendly** - Visual workflow editor, no coding required
- ✅ **Production Ready** - Robust error handling and retry logic

## 🏗️ **Architecture Overview**

```
┌─────────────┐    ┌─────────────┐    ┌─────────────┐
│   Stock     │    │    OMS      │    │    OMS      │
│   Service   │    │  Posting    │    │   Pulling   │
│   Check     │    │   Check     │    │    Check    │
└─────────────┘    └─────────────┘    └─────────────┘
       │                   │                   │
       └───────────────────┼───────────────────┘
                           │
                ┌─────────────┐
                │   Process   │
                │   Health    │
                │   Results   │
                └─────────────┘
                           │
                ┌─────────────┐
                │ Has Errors? │
                │    (IF)     │
                └─────────────┘
                    │       │
          ┌─────────┘       └─────────┐
          │                           │
   ┌─────────────┐           ┌─────────────┐
   │ Send Slack  │           │ Service OK  │
   │   Alert     │           │ (Success)   │
   │ (Error)     │           │             │
   └─────────────┘           └─────────────┘
```

## 🛠️ **Prerequisites**

- **n8n Instance** (Self-hosted or Cloud)
- **Slack Workspace** with admin permissions
- **Slack Bot Token** for sending messages
- **Channel ID** where notifications will be sent

## 📥 **Installation**

### Step 1: Clone Repository

```bash
git clone https://github.com/yourusername/service-health-monitor.git
cd service-health-monitor
```

### Step 2: Set Up Slack Bot

1. **Create Slack App**
   - Go to [Slack API Apps](https://api.slack.com/apps)
   - Click "Create New App" → "From scratch"
   - Name: "Health Monitor Bot"
   - Select your workspace

2. **Configure Bot Permissions**
   - Go to "OAuth & Permissions"
   - Add Bot Token Scopes:
     - `chat:write` - Send messages
     - `channels:read` - Read channel info
     - `groups:read` - Read private channels

3. **Install Bot to Workspace**
   - Click "Install to Workspace"
   - Copy the "Bot User OAuth Token" (starts with `xoxb-`)

4. **Get Channel ID**
   - Right-click your Slack channel
   - Select "View channel details"
   - Copy the Channel ID (starts with `C`)

### Step 3: Import n8n Workflow

1. **Open n8n**
2. **Import Workflow**
   - Click "Import from File"
   - Select `health-monitor-workflow.json`
3. **Configure Credentials**
   - Add Slack credential with your bot token
   - Update channel ID in both Slack nodes

## ⚙️ **Configuration**

### Service URLs (Modify as needed)

The workflow monitors these endpoints by default:

```javascript
// Current test endpoints
Stock Check: https://httpbin.org/get
OMS Posting: https://jsonplaceholder.typicode.com/posts  
OMS Pulling: https://httpbin.org/delay/2
```

### Update for Your Services

1. **Open each HTTP Request node**
2. **Replace URLs** with your actual service endpoints:

```javascript
// Example production endpoints
Stock Check: https://your-api.com/health/stock
OMS Posting: https://your-api.com/health/oms-posting
OMS Pulling: https://your-api.com/health/oms-pulling
```

### Customize Check Frequency

Default: Every 5 minutes

1. **Click "Every 5 Minutes" node**
2. **Modify interval** as needed (1min, 10min, hourly, etc.)

## 🚀 **Usage**

### Manual Execution

1. **Open n8n workflow**
2. **Click "Execute workflow"**
3. **Check Slack channel** for notifications

### Automated Monitoring

1. **Activate the workflow** (toggle switch in n8n)
2. **Monitoring runs automatically** every 5 minutes
3. **Receive notifications** in Slack

## 📱 **Sample Notifications**

### ✅ Healthy Service
```
✅ Service Healthy ✅

⏰ 2025-07-07T13:32:50.194Z
🌐 Stock Check  
📊 Stock Check is working perfectly

🎉 System operational!
```

### 🚨 Service Alert
```
🚨 SERVICE HEALTH ALERT 🚨

⏰ 2025-07-07T13:32:50.194Z
🌐 OMS Posting
📊 Service down - Status 500

🔧 Please investigate immediately!
```

## 🔧 **Troubleshooting**

### Common Issues

1. **No Slack Messages**
   - ✅ Check bot is added to channel (`/invite @HealthMonitorBot`)
   - ✅ Verify bot token is correct
   - ✅ Ensure channel ID is accurate

2. **All Services Show as Alerts**
   - ✅ Check IF node condition: `{{ $json.hasErrors }}` equals `true`
   - ✅ Verify arrow routing: FALSE → Success, TRUE → Alert

3. **"Unknown Service" Names**
   - ✅ Update service detection logic in JavaScript
   - ✅ Check URL patterns match your endpoints

### Debug Mode

Enable console logging in the "Process Single Result" node:

```javascript
console.log('Processing URL:', url);
console.log('Identified service:', serviceName);
console.log('Has errors:', hasErrors);
```

## 🎯 **Customization Examples**

### Add Email Notifications

1. **Add Email node** after IF condition
2. **Configure SMTP** credentials
3. **Set recipient** for critical alerts

### Monitor Response Times

Update JavaScript to check response times:

```javascript
const responseTime = Date.now() - startTime;
const isHealthy = statusCode >= 200 && statusCode < 300 && responseTime < 5000;
```

### Add More Services

1. **Duplicate HTTP Request node**
2. **Update service URL**
3. **Add to service detection logic**

## 📈 **Future Enhancements**

- [ ] **Dashboard Integration** - Grafana/DataDog visualizations
- [ ] **Historical Logging** - Store health check results
- [ ] **SLA Monitoring** - Track uptime percentages
- [ ] **Escalation Rules** - Multi-level alerting
- [ ] **Mobile App Notifications** - Push notifications
- [ ] **Webhook Integration** - PagerDuty, OpsGenie
- [ ] **AI-Powered Insights** - Predictive failure detection

## 🤝 **Contributing**

1. **Fork the repository**
2. **Create feature branch** (`git checkout -b feature/amazing-feature`)
3. **Commit changes** (`git commit -m 'Add amazing feature'`)
4. **Push to branch** (`git push origin feature/amazing-feature`)
5. **Open Pull Request**

## 📝 **License**

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🙏 **Acknowledgments**

- **n8n Community** - For the amazing workflow automation platform
- **Slack API** - For robust notification capabilities
- **httpbin.org** - For reliable testing endpoints
- **JSONPlaceholder** - For mock API services

## 📧 **Contact**

**Minhaj S** - Junior DevOps Engineer | MLOps Enthusiast | Cloud Automation Specialist

- LinkedIn: [Your LinkedIn Profile]
- Email: [your.email@domain.com]
- Portfolio: [Your Portfolio Website]

---

⭐ **If this project helped you, please give it a star!** ⭐

## 🔖 **Tags**

`#DevOps` `#Monitoring` `#Automation` `#n8n` `#Slack` `#NoCode` `#SRE` `#HealthCheck` `#AlertSystem` `#CloudOps`