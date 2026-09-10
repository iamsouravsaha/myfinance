# 💰 My Finance — Personal Finance Management System

A lightweight, professional personal finance management system built around **Google Sheets + Google Apps Script + HTML/CSS/JavaScript**.

The system is designed to make daily transaction entry easy from mobile while providing a separate MIS dashboard for financial analysis.

---

## 📌 1. Project Overview

**My Finance** is a personal financial control system where:

- Google Sheets works as the central database.
- Google Apps Script works as the backend/API.
- A separate HTML page is used for transaction entry.
- A separate MIS dashboard is used for reporting and analysis.
- A professional Home Page provides navigation between the MIS and Data Entry pages.

### Main Flow

```text
                    ┌─────────────────────┐
                    │   MY FINANCE HOME   │
                    │     Home Page       │
                    └──────────┬──────────┘
                               │
                  ┌────────────┴────────────┐
                  │                         │
                  ▼                         ▼
        ┌──────────────────┐      ┌──────────────────┐
        │    FINANCE MIS   │      │  ADD TRANSACTION │
        │    Dashboard     │      │    Entry Page    │
        └────────┬─────────┘      └────────┬─────────┘
                 │                         │
                 │                         ▼
                 │               ┌──────────────────┐
                 │               │ Google Apps      │
                 │               │ Script API       │
                 │               └────────┬─────────┘
                 │                        │
                 └────────────────────────┤
                                          ▼
                              ┌──────────────────────┐
                              │     Google Sheets    │
                              │     RAW_DATA         │
                              └──────────────────────┘
```

---

# 📁 2. Project Files

The recommended project structure is:

```text
My Finance/
│
├── FINANCE_HOME.html
├── MIS.html
├── DATA_ENTRY.html
│
├── Code.gs
│
└── README.md
```

### File Purpose

| File | Purpose |
|---|---|
| `FINANCE_HOME.html` | Main landing/home page |
| `MIS.html` | Finance MIS dashboard |
| `DATA_ENTRY.html` | Transaction entry page |
| `Code.gs` | Google Apps Script backend/API |
| `README.md` | Project documentation |

---

# 🏠 3. Home Page

The Home Page provides two primary options:

### 📊 Finance MIS

Opens the MIS dashboard where financial performance can be reviewed.

### ➕ Add Transaction

Opens the transaction entry page for recording a new transaction.

The current Home Page uses:

```javascript
const MIS_URL = "MIS.html";
const DATA_ENTRY_URL = "DATA_ENTRY.html";
```

If the pages are hosted separately, replace these values with the actual URLs.

Example:

```javascript
const MIS_URL = "https://your-mis-url.com";
const DATA_ENTRY_URL = "https://your-entry-url.com";
```

---

# 📊 4. Finance MIS

The MIS dashboard is designed to provide a complete financial overview.

### Main Areas

- Current financial position
- Bank balance
- Cash balance
- Savings
- Investments
- Income
- Expenses
- Net cash flow
- Monthly analysis
- Yearly analysis
- Category analysis
- Transaction history
- Search/filter
- Edit transaction
- Delete transaction
- Charts and visual analytics
- Financial insights

---

# 💳 5. Transaction Structure

Every transaction contains the following fields:

| Field | Description |
|---|---|
| Date | Transaction date |
| Source Type | Income / Expense / Investment / Savings |
| Sub Type | Category under the selected Source Type |
| Transaction Mode | Bank / Cash |
| Amount | Transaction amount |
| Notes / Description | Optional transaction details |

---

# 🗂️ 6. Source Types & Sub Types

## Income

```text
Salary
Professional Fee
Interest
Others
```

## Expense

```text
Electricity
Maa
Baba
Moni
Others
```

## Investment

```text
SIP
PF
```

## Savings

```text
BOB
KBL
```

The Sub Type dropdown should depend on the selected Source Type.

Example:

```text
Source Type = Expense

Sub Type =
    Electricity
    Maa
    Baba
    Moni
    Others
```

---

# 🧾 7. Google Sheet Database Structure

The Google Spreadsheet should contain the following tabs:

```text
RAW_DATA
MASTER_DATA
ACCOUNT_MASTER
CONFIG
```

---

## 7.1 RAW_DATA

`RAW_DATA` is the primary transaction database.

Recommended columns:

| Column | Field |
|---|---|
| A | Transaction ID |
| B | Date |
| C | Source Type |
| D | Sub Type |
| E | Transaction Mode |
| F | Amount |
| G | Notes |
| H | Created Timestamp |
| I | Month |
| J | Year |
| K | Month-Year |

### Important

The user should normally **not manually enter transactions in RAW_DATA**.

Transactions should be submitted from the Data Entry webpage and saved automatically through Apps Script.

---

# 🗃️ 8. MASTER_DATA

`MASTER_DATA` stores the Source Type and Sub Type mapping.

Recommended structure:

| Source Type | Sub Type |
|---|---|
| Income | Salary |
| Income | Professional Fee |
| Income | Interest |
| Income | Others |
| Expense | Electricity |
| Expense | Maa |
| Expense | Baba |
| Expense | Moni |
| Expense | Others |
| Investment | SIP |
| Investment | PF |
| Savings | BOB |
| Savings | KBL |

### Why MASTER_DATA?

It makes the transaction categories manageable without changing the main application logic.

If a new category is required, add another row to `MASTER_DATA`.

Example:

```text
Expense | Internet
```

---

# 🏦 9. ACCOUNT_MASTER

`ACCOUNT_MASTER` stores the financial accounts/assets used by the system.

Recommended structure:

| Account | Type | Opening Balance |
|---|---|---:|
| Main Bank | Bank | 0 |
| Cash | Cash | 0 |
| BOB | Savings | 0 |
| KBL | Savings | 0 |
| SIP | Investment | 0 |
| PF | Investment | 0 |

The opening balances should be replaced with the actual starting balances when the system is first implemented.

---

# ⚙️ 10. CONFIG

`CONFIG` stores application-level settings.

Recommended structure:

| Setting | Value |
|---|---|
| App Name | My Finance |
| Currency | INR |
| Version | 1.0 |

This sheet can later be expanded for:

- App title
- Currency
- Fiscal year
- Default settings
- Version control
- Other configuration parameters

---

# 🧠 11. Financial Logic

The system separates **spendable money** from **savings and investments**.

### Spendable Money

```text
Spendable Money = Bank + Cash
```

### Savings

```text
Savings = BOB + KBL
```

### Investments

```text
Investments = SIP + PF
```

### Total Financial Assets

```text
Total Financial Assets
= Bank + Cash + Savings + Investments
```

---

# 📈 12. Monthly Net Cash Flow

The monthly cash flow calculation is:

```text
Net Cash Flow
= Income
- Expense
- Savings
- Investment
```

Example:

```text
Income       = ₹50,000
Expense      = ₹25,000
Savings      = ₹10,000
Investment   = ₹5,000

Net Cash Flow
= 50,000 - 25,000 - 10,000 - 5,000
= ₹10,000
```

---

# 💹 13. Savings & Investment Rate

### Savings Rate

```text
Savings Rate
= Savings / Income × 100
```

### Investment Rate

```text
Investment Rate
= Investment / Income × 100
```

These metrics help understand how much of the income is being allocated toward savings and investments.

---

# 🔄 14. Transaction Accounting Logic

### Income

Income increases the selected Bank/Cash balance.

```text
Income → Bank/Cash ↑
```

### Expense

Expense decreases the selected Bank/Cash balance.

```text
Expense → Bank/Cash ↓
```

### Savings

Savings moves money from spendable money into savings.

```text
Bank/Cash ↓
Savings ↑
```

### Investment

Investment moves money from spendable money into investments.

```text
Bank/Cash ↓
Investment ↑
```

This prevents savings and investments from being treated as money that simply disappeared.

---

# 🔌 15. Google Apps Script Backend

Google Apps Script acts as the backend/API between the webpages and Google Sheets.

The main API actions are:

```text
?action=master
?action=save
?action=mis
?action=delete
?action=edit
```

### Master Data

```text
?action=master
```

Returns Source Type and Sub Type information.

### Save Transaction

```text
?action=save
```

Saves a new transaction into `RAW_DATA`.

### MIS Data

```text
?action=mis
```

Returns calculated MIS data.

### Delete

```text
?action=delete
```

Deletes a transaction.

### Edit

```text
?action=edit
```

Updates an existing transaction.

---

# 🌐 16. Apps Script Web App

The Google Apps Script project should be deployed as a **Web App**.

Recommended deployment settings:

```text
Execute as:
Me

Who has access:
Anyone with the link
```

The resulting URL will look similar to:

```text
https://script.google.com/macros/s/XXXXXXXXXXXX/exec
```

Keep this URL available because the HTML pages use it to communicate with the backend.

---

# 🏗️ 17. Recommended Architecture

```text
                   USER
                    │
                    ▼
          ┌───────────────────┐
          │ Finance Home Page │
          └─────────┬─────────┘
                    │
             ┌──────┴──────┐
             │             │
             ▼             ▼
         MIS.html     DATA_ENTRY.html
             │             │
             │             ▼
             │       Apps Script API
             │             │
             └──────┬──────┘
                    ▼
             Google Sheets
                    │
          ┌─────────┼─────────┐
          ▼         ▼         ▼
       RAW_DATA  MASTER_DATA  ACCOUNT_MASTER
```

---

# 📱 18. Mobile UI/UX

The application is designed with mobile usage in mind.

Important UI principles:

- Large touch-friendly buttons
- Simple navigation
- Clear typography
- Responsive cards
- Easy transaction entry
- Minimal scrolling where possible
- Mobile-friendly dashboard
- Desktop compatibility
- Professional finance-oriented color scheme

The Home Page uses a mobile-first layout with two primary action cards.

---

# 🎨 19. Design System

The Home Page uses a professional finance-oriented palette.

Primary visual direction:

```text
Navy
Royal Blue
Emerald Green
White
Light Grey
```

### Design Philosophy

```text
Professional
Clean
Minimal
Modern
Trustworthy
Mobile First
```

The system intentionally avoids excessive colors so that financial information remains easy to read.

---

# 🔐 20. Data & Security Notes

The Google Sheet is the main data store, so access permissions should be carefully controlled.

Recommended practices:

- Do not publicly share the Google Sheet itself.
- Keep the Apps Script deployment permissions intentional.
- Avoid exposing unnecessary sheet data through API responses.
- Validate transaction inputs before saving.
- Validate amount values.
- Validate Source Type and Sub Type.
- Keep backup copies of the spreadsheet.
- Do not store passwords or sensitive authentication information inside the sheet.

---

# 🧪 21. Testing Checklist

Before using the system regularly, test:

### Home Page

- [ ] Home page loads correctly
- [ ] MIS button works
- [ ] Add Transaction button works
- [ ] Mobile layout works
- [ ] Desktop layout works

### Data Entry

- [ ] Date selection works
- [ ] Source Type works
- [ ] Sub Type changes according to Source Type
- [ ] Bank/Cash selection works
- [ ] Amount validation works
- [ ] Notes work
- [ ] Submit works
- [ ] Success message appears
- [ ] Data reaches RAW_DATA

### MIS

- [ ] Dashboard loads
- [ ] Current month works
- [ ] All time works
- [ ] Year filter works
- [ ] Search works
- [ ] Charts load
- [ ] Bank balance is correct
- [ ] Cash balance is correct
- [ ] Savings is correct
- [ ] Investment is correct
- [ ] Net cash flow is correct
- [ ] Transaction history loads
- [ ] Edit works
- [ ] Delete works

---

# 🚀 22. Deployment Options

The HTML pages can be hosted separately from Google Apps Script.

Possible hosting options include:

```text
GitHub Pages
Netlify
Vercel
Traditional Web Hosting
Local Server
```

Google Apps Script remains the backend/API.

### Example

```text
Frontend
│
├── Finance Home
├── MIS
└── Data Entry

          ↓

Google Apps Script

          ↓

Google Sheets
```

This architecture allows the UI to be redesigned or hosted independently without changing the underlying database.

---

# 🔧 23. Future Enhancements

The system can later be expanded with:

### Dashboard

- Advanced monthly comparison
- Year-over-year comparison
- Expense heatmap
- Budget vs actual
- Savings goals
- Investment performance
- Financial health score

### Transactions

- Advanced search
- Date range filter
- Category filter
- Bank/Cash filter
- Bulk edit
- Transaction export
- CSV export

### Automation

- Monthly financial report
- Weekly summary
- Email report
- Google Sheets automated backup
- Monthly closing
- Reminder system

### Security

- User login
- PIN/Password protection
- Google authentication
- Role-based access

### Mobile

- Installable PWA
- App icon
- Offline entry
- Mobile notifications

---

# 📝 24. Maintenance Guide

If a new transaction category is required:

1. Open `MASTER_DATA`.
2. Add a new row.
3. Enter the Source Type.
4. Enter the Sub Type.
5. Save the sheet.
6. Refresh the Data Entry page.

Example:

```text
Expense | Internet
Expense | Mobile
Income  | Bonus
Investment | Stocks
```

The application can then use the updated master data.

---

# ⚠️ 25. Important Configuration Points

Always verify these values in the Apps Script:

```javascript
const RAW_SHEET = 'RAW_DATA';
const MASTER_SHEET = 'MASTER_DATA';
const ACCOUNT_SHEET = 'ACCOUNT_MASTER';
const CONFIG_SHEET = 'CONFIG';
```

The Google Sheet tab names must exactly match these names.

For example:

```text
RAW_DATA        ✅
Raw Data        ❌
RAW DATA        ❌
```

---

# 🔁 26. Basic Operating Process

Everyday usage should be:

```text
Open My Finance
      ↓
Add Transaction
      ↓
Select Date
      ↓
Select Source Type
      ↓
Select Sub Type
      ↓
Select Bank/Cash
      ↓
Enter Amount
      ↓
Add Notes
      ↓
Submit
      ↓
Data saved to Google Sheets
      ↓
Open MIS
      ↓
Review financial position
```

---

# 📌 27. Golden Rules

### Rule 1
**RAW_DATA is the transaction database.**

### Rule 2
**MASTER_DATA controls categories.**

### Rule 3
**ACCOUNT_MASTER controls account/opening balance information.**

### Rule 4
**CONFIG controls application settings.**

### Rule 5
**Do not manually modify calculated MIS values.**

### Rule 6
**Enter transactions through the Data Entry page whenever possible.**

### Rule 7
**Keep regular backups of the Google Sheet.**

---

# 🎯 28. Project Goal

The ultimate goal of **My Finance** is to create a simple but professional personal financial management system that answers four important questions:

```text
💰 How much money do I have?

📉 Where is my money being spent?

💎 How much am I saving?

📈 How much am I investing?
```

The MIS should make these answers available quickly from both **mobile and desktop**.

---

## Version

**My Finance v1.0**

**Technology Stack**

```text
Frontend:
HTML
CSS
JavaScript

Backend:
Google Apps Script

Database:
Google Sheets

Charts:
Chart.js

Hosting:
External HTML Hosting / Google Apps Script
```

---

**End of Documentation**
