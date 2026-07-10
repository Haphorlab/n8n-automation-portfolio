# Automated Invoice Management System

End-to-end invoice automation for freelancers and service providers. Automatically generates, sends, and tracks invoices with overdue payment alerts.
## 🎯 Overview

This workflow eliminates manual invoice creation and follow-ups by automatically generating professional PDF invoices, emailing them to clients, and alerting you when payments are overdue.

**Perfect For:**
- Freelancers & consultants
- Small agencies
- Service providers
- Contract workers
- Anyone who sends invoices regularly

## ✨ Features

### **Automated Invoice Generation**
- Calculates amounts for hourly or fixed-price projects
- Generates unique invoice numbers (`INV-2026-123456`)
- Creates professional PDF invoices
- Automatically emails clients
- Tracks invoice status in Airtable

### **Smart Payment Tracking**
- Daily checks for overdue invoices
- Telegram alerts for late payments
- Auto-updates project status
- Maintains payment history

### **Flexible Billing**
- Supports hourly billing with rate calculation
- Fixed-price project support
- Configurable payment terms (Net 15/30/45)
- Custom rates per client

## 🏗️ Architecture

### **Flow 1: Invoice Generation**
```
Project marked "Ready" in Airtable
    ↓
Fetch client details (email, rate, payment terms)
    ↓
Calculate invoice amount (hourly × rate OR fixed price)
    ↓
Generate unique invoice number
    ↓
Save to Invoices table
    ↓
Create PDF invoice (professional template)
    ↓
Email PDF to client
    ↓
Mark project as "Invoiced"
```

### **Flow 2: Overdue Alerts**
```
Daily at 9 AM
    ↓
Search for invoices where: Status = "Sent" AND Due Date < Today
    ↓
Get client details
    ↓
Send Telegram alert with invoice details
```

## 🛠️ Tech Stack

- **n8n** - Workflow automation
- **Airtable** - Database for clients, projects, invoices
- **Gmail** - Email delivery
- **Telegram** - Overdue payment alerts
- **PDF Generation** - Professional invoice templates

## 📋 Prerequisites

- n8n instance (Cloud or self-hosted)
- Airtable account
- Gmail account
- Telegram bot (for alerts)

## 🗄️ Database Structure

### **Airtable Base: 3 Tables**

#### **1. Clients Table**
```
- Client name (text)
- Email (email)
- Company (text)
- Address (text)
- Default rate (number, $/hour)
- Payment terms (select: Net 15, Net 30, Net 45)
```

#### **2. Projects Table**
```
- Project name (text)
- Client (link to Clients)
- Project type (select: Hourly, Fixed Price)
- Hours worked (number, if hourly)
- Rate (number, if hourly)
- Fixed amount (number, if fixed)
- Description (long text)
- Status (select: In Progress, Ready, Invoiced)
```

#### **3. Invoices Table**
```
- Invoice Number (text, unique)
- Client (link to Clients)
- Project (link to Projects)
- Amount (currency)
- Issue Date (date)
- Due date (date)
- Status (select: Sent, Paid, Overdue)
- PDF link (attachment, optional)
- Sent date (date)
```

## 🚀 Setup Instructions

### 1. Create Airtable Base

1. Create a new Airtable base
2. Set up the 3 tables above
3. Add sample data for testing
4. Copy your Base ID (from URL)

### 2. Set Up Telegram Bot (Optional)

1. Message [@BotFather](https://t.me/botfather) on Telegram
2. Send `/newbot` and follow instructions
3. Copy the API token
4. Message your new bot and get your Chat ID

### 3. Import Workflow

1. Download `workflow.json`
2. In n8n: Workflows → Import from File
3. Upload the JSON

### 4. Configure Credentials

Set up these credentials in n8n:

- **Airtable API Token**
- **Gmail OAuth**
- **Telegram Bot API** (if using alerts)

### 5. Replace Placeholders

Update these in the workflow:

- `YOUR_AIRTABLE_BASE_ID` → Your Airtable base ID
- `YOUR_PROJECTS_TABLE_ID` → Projects table ID
- `YOUR_CLIENTS_TABLE_ID` → Clients table ID  
- `YOUR_INVOICES_TABLE_ID` → Invoices table ID
- `YOUR_TELEGRAM_CHAT_ID` → Your Telegram chat ID
- `CREDENTIAL_ID` → Your n8n credential references

### 6. Customize Invoice Template

Edit the "Generate invoice" node to:
- Add your company logo
- Update company details
- Customize styling
- Add payment instructions

### 7. Test & Activate

1. Create a test project in Airtable
2. Mark it as "Ready"
3. Verify invoice generation
4. Check email delivery
5. Set workflow to ACTIVE

## 💼 Usage

### **To Generate an Invoice:**

1. Complete work on a project
2. In Airtable, update project status to "Ready"
3. Workflow automatically:
   - Generates invoice
   - Creates PDF
   - Emails client
   - Updates status to "Invoiced"

### **Invoice Calculation:**

**Hourly Projects:**
```
Amount = Hours Worked × Hourly Rate
```

**Fixed Price Projects:**
```
Amount = Fixed Amount
```

### **Payment Terms:**

- Net 15: Due in 15 days
- Net 30: Due in 30 days
- Net 45: Due in 45 days

## 📧 Email Template

```
Subject: Invoice [INV-2026-123456] from Your Company

Hi [Client Name],

Thank you for your business! Please find attached 
invoice [INV-2026-123456] for [Project Name].

Amount Due: $[Amount]
Due Date: [Due Date]

Please let me know if you have any questions.

Best regards
```

## 📊 Invoice Template

The generated PDF includes:

- Your company details
- Client information
- Invoice number & dates
- Project description
- Amount breakdown
- Payment terms
- Professional styling

## 🔔 Overdue Alerts

Every day at 9 AM, you receive Telegram alerts for overdue invoices:

```
🚨 OVERDUE INVOICE ALERT

Invoice: INV-2026-123456
Client: Acme Corp
Amount: $2,500
Due Date: 2026-01-15

Email: client@acmecorp.com

⚡ Action needed: Follow up with client!
```

## 🔧 Customization

### **Change Alert Time:**
Edit "Daily 9am check" node → Change hour

### **Add More Fields:**
Update "calculate invoice" code node to include additional data

### **Custom Invoice Design:**
Edit HTML in "Generate invoice" node

### **Multiple Email Templates:**
Add conditional logic based on client/project type

### **Payment Reminders:**
Add additional scheduled triggers for 7-day/3-day warnings

## 📈 Benefits

- **Time Saved:** ~30 minutes per invoice
- **Error Reduction:** No manual calculations
- **Faster Payments:** Immediate invoice delivery
- **Better Tracking:** Centralized invoice database
- **Reduced Late Payments:** Automatic reminders

## 🤝 Contributing

Feedback and suggestions welcome!

## 📄 License

MIT License - free to use and modify

## 🙋 Questions?

Built by **Aliyah Williams** - Automation Specialist

- GitHub: [@Haphorlab](https://github.com/Haphorlab)
- Portfolio: [n8n Automation Projects](https://github.com/Haphorlab/n8n-automation-portfolio)

## 🔮 Future Enhancements

- [ ] Stripe/PayPal payment integration
- [ ] Recurring invoices for retainers
- [ ] Multi-currency support
- [ ] Invoice templates library
- [ ] Expense tracking
- [ ] Profit/loss reporting
- [ ] QuickBooks/Xero integration

---

**Note:** Requires Airtable base setup and proper credentials. See setup instructions above.
