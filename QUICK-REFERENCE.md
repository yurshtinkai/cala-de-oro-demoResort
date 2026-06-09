# Quick Reference - Cala de Oro Internal Management System

**Version:** 1.3.1  
**Date:** March 21, 2024

---

## 🔑 Login Credentials (Staff Only)

| Role | Email | Password |
|------|-------|----------|
| **CEO** | ceo@caladeoro.com | ceo123 |
| **ADMIN** | admin@caladeoro.com | admin123 |
| **IT STAFF** | it@caladeoro.com | it123 |

**Total:** 3 Staff Roles Only

---

## 🎯 Role Capabilities

### 🎩 CEO (Executive)
✅ Executive Dashboard (KPIs, Charts)  
✅ Financial Reports  
✅ View Room Status  
✅ Export Reports  
❌ **Read-Only** (Cannot process transactions)

**Default View:** Reports & Analytics

---

### 👨‍💼 ADMIN (Operations Manager)
✅ All Operational Modules  
✅ Room Management (Check-in/out)  
✅ Process Transactions  
✅ Booking Management  
✅ Guest Management  
✅ Financial Reports  
✅ View Users (limited creation)  
❌ Cannot create CEO/ADMIN users  
❌ No system settings access

**Default View:** Room Status Map

---

### 💻 IT STAFF (Technical Admin)
✅ **Everything ADMIN Can Do**  
✅ **PLUS Executive Dashboard**  
✅ **Reports & Analytics** (Fixed in v1.3.1)  
✅ Full User Management (create any role)  
✅ System Settings  
✅ Audit Logs  
✅ Database Management  
✅ Technical Reports

**Default View:** User Management Dashboard

---

## 📱 Quick Actions by Role

### CEO Can:
- View KPIs and trends
- Generate reports
- Export data (PDF, Excel)
- Monitor business performance

### ADMIN Can:
- Check-in/out guests
- Process all transactions
- Manage room status
- Handle bookings
- Manage guests

### IT Can:
- All ADMIN functions
- All CEO insights
- Manage users
- Configure system
- Troubleshoot issues
- Access all data

---

## 🏢 System Sections

### Available to All Staff:
- Room Status Map
- Room Categories
- Entrance Fees
- Corkage
- Tents & Equipment
- Resort Outlets
- Other Sales

### CEO & IT Only:
- Executive Dashboard
- Advanced Analytics

### ADMIN, IT Only:
- Full Transaction Processing
- Booking Modifications

### IT Only:
- User Management
- System Settings
- Audit Logs

---

## 🚫 What This System is NOT

❌ **Not for customers/guests** - They use separate booking website  
❌ **Not for front desk staff** - Operations handled by ADMIN  
❌ **Not public-facing** - Internal staff use only  
❌ **Not a booking engine** - Management and reporting tool

---

## ✅ What This System IS

✅ **Staff-only internal tool**  
✅ **Operations management**  
✅ **Financial reporting**  
✅ **Business intelligence**  
✅ **User administration**  
✅ **System monitoring**

---

## 🔄 Typical Workflows

### Guest Check-in (ADMIN):
1. Login as ADMIN
2. Go to Room Status Map
3. Click available room
4. Enter guest details
5. Confirm check-in

### Generate Report (CEO):
1. Login as CEO
2. Automatically in Reports section
3. View executive dashboard
4. Export if needed

### Create User (IT):
1. Login as IT
2. Go to User Management
3. Click "Add New User"
4. Fill details and assign role
5. Save

### Process Transaction (ADMIN/IT):
1. Navigate to transaction section
2. Select category (entrance, corkage, etc.)
3. Enter details and quantity
4. Add to cart
5. Submit transaction

---

## 📊 Key Metrics (CEO Dashboard)

### KPI Cards:
- Revenue This Month (with % change)
- Occupancy Rate (with trend)
- Total Bookings (with comparison)
- Average Daily Rate (with % change)

### Charts:
- Revenue Trend (12 months)
- Occupancy Trend
- Revenue by Category
- Revenue Split (CALA vs Vanessa)

### Tables:
- Top Performing Rooms
- Transaction Log
- Revenue Breakdown

---

## 🛠️ Troubleshooting

### Can't Login?
- Check email format (must be exact)
- Check password (case-sensitive)
- Clear browser cache
- Try different browser

### Missing Features?
- **CEO:** Should see executive dashboard in Reports
- **ADMIN:** Should see all operation sections
- **IT:** Should see everything + user management

### Role Confusion?
- **CEO** = Read-only executive view
- **ADMIN** = Full operations
- **IT** = Full system access + tech admin

---

## 📁 File Locations

### Main Files:
- `index.html` - Main Dashboard
- `admin-dashboard.html` - User Management

### Documentation:
- `README.md` - Project overview
- `CHANGELOG.md` - Version history
- `ROLE-SIMPLIFICATION-SUMMARY.md` - This change details
- `.kiro/specs/internal-management-system.md` - Full spec
- `.kiro/specs/tasks.md` - Implementation tasks

---

## 🆘 Quick Help

### Forgot Password?
- Click "Forgot Password?" on login
- (Currently demo only - shows message)

### Need Different Access?
- Contact IT Staff
- They can modify user roles

### System Not Working?
- Clear browser cache: Ctrl+Shift+Delete
- Clear localStorage: `localStorage.clear()`
- Hard refresh: Ctrl+Shift+R
- Check browser console: F12

---

## 📞 Support

### For Technical Issues:
- Contact: IT Staff
- Check: Browser console (F12)
- Review: Documentation files

### For Access Issues:
- Contact: IT Staff (can manage users)
- Verify: Using correct credentials

### For Feature Requests:
- Contact: Project Lead
- Review: Tasks.md for roadmap

---

## 🎓 Training Resources

### New CEO Users:
- Focus on Reports & Analytics section
- Learn to interpret KPIs
- Practice exporting reports

### New ADMIN Users:
- Practice check-in/out workflow
- Learn transaction processing
- Understand booking management

### New IT Users:
- Review all system sections
- Practice user management
- Understand role permissions

---

## 🔐 Security Reminders

✅ **Do:**
- Keep credentials private
- Logout when done
- Report suspicious activity
- Use strong passwords (production)

❌ **Don't:**
- Share login credentials
- Leave system unattended
- Access from public computers
- Give credentials to customers

---

## 📈 Current Version Features

### v1.3.1 (Current):
✅ 3 staff roles (CEO, ADMIN, IT)  
✅ CEO Executive Dashboard  
✅ IT gets executive dashboard  
✅ **IT Reports access fixed** 🐛  
✅ Role-based access control  
✅ User management  
✅ Room management  
✅ Transaction processing  
✅ Basic reporting  

### Coming Soon (Phase 2):
🔨 Enhanced booking management  
🔨 Guest/CRM system  
🔨 Advanced reporting with filters  
🔨 Export to Excel/PDF  

---

**Print this page for quick reference!**

---

**Last Updated:** March 21, 2024  
**Version:** 1.3.1  
**Status:** Active - Production Ready
