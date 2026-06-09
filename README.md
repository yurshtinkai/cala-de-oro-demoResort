# 🏖️ Cala de Oro Resort - Internal Management System

**Version:** 1.3.1  
**Last Updated:** March 21, 2024  
**Status:** In Active Development

---

## 📋 Project Overview

The **Cala de Oro Internal Management System** is a comprehensive web-based application designed for staff-only access to manage resort operations, process transactions, generate reports, and provide business insights.

This system is **separate from the customer-facing booking website** and serves as the operational backbone for:
- 👔 CEO/Executives (strategic oversight)
- 👨‍💼 Administrators (full operations)
- 💻 IT Staff (system management & technical administration)

**Note:** This is a **staff-only internal system**. Customers/guests use a separate booking website.

---

## 🚀 Quick Start

### **Access the System:**

1. **Main Dashboard:**
   - Open `index.html` in your browser
   - Login with demo credentials (see below)

2. **Admin Dashboard (User Management):**
   - Open `admin-dashboard.html` in your browser
   - Access restricted to ADMIN and IT roles only

### **Demo Credentials:**

| Role | Email | Password | Access Level |
|------|-------|----------|--------------|
| **CEO** | ceo@caladeoro.com | ceo123 | Executive Dashboard, Reports (Read-only) |
| **ADMIN** | admin@caladeoro.com | admin123 | Full Operations, User Management |
| **IT STAFF** | it@caladeoro.com | it123 | Full System Access, User Management, Executive Dashboard |

**Note:** Only 3 staff roles. No customer/guest access to this system.

---

## 📁 Project Structure

```
cala-de-oro-demoResort/
├── index.html                    # Main Dashboard (Operations)
├── admin-dashboard.html          # User Management Dashboard
├── README.md                     # This file
├── .kiro/
│   └── specs/
│       ├── internal-management-system.md  # Full Specification
│       └── tasks.md              # Implementation Tasks
└── .git/                         # Git repository (if initialized)
```

---

## ✅ Completed Features

### 1. **Authentication System**
- Secure login with email/password
- Password visibility toggle
- Forgot password functionality
- Session management
- Role-based authentication

### 2. **Role-Based Access Control**
- 4 distinct roles (CEO, ADMIN, IT, USER)
- Dynamic menu visibility
- Permission-based features
- Read-only mode for CEO

### 3. **User Management**
- CRUD operations for users
- Role assignment
- Status management (Active/Inactive)
- Search and filter
- Statistics dashboard
- Access: ADMIN and IT only

### 4. **Room Management**
- Visual room status map
- Room categories view
- Check-in/check-out
- Room status updates
- Guest information
- Excess pax calculation

### 5. **Transaction Processing**
- Room rentals
- Entrance fees (day/night tours)
- Corkage fees
- Tent rentals & equipment
- Resort outlet sales
- Custom items
- Live transaction log

### 6. **Basic Financial Reporting**
- Transaction log
- Revenue split (CALA vs Vanessa)
- Customer total
- Printable reports
- Real-time calculations

---

## 🔨 In Development (Phase 2)

### High Priority:
1. **CEO Executive Dashboard**
   - KPI cards with trends
   - Revenue charts (Chart.js)
   - Occupancy graphs
   - Export functionality

2. **Enhanced Booking Management**
   - Booking list with filters
   - Booking details modal
   - Modify/cancel bookings
   - Payment tracking

3. **Advanced Financial Reporting**
   - Date range selector
   - Category filters
   - Export to Excel/PDF
   - Multiple report types

### Medium Priority:
4. **Guest Management (CRM)**
   - Guest database
   - Booking history
   - Guest preferences
   - Communication logs

5. **Enhanced Room Features**
   - Maintenance scheduling
   - Housekeeping assignments
   - Room history
   - Rate modifications

---

## 📊 System Architecture

### Current Stack:
- **Frontend:** HTML5, CSS3, Vanilla JavaScript
- **Storage:** localStorage (demo/prototype)
- **Authentication:** sessionStorage
- **Architecture:** Single-page application (SPA)

### Production Stack (Future):
- **Backend:** Node.js + Express or PHP + Laravel
- **Database:** MySQL or PostgreSQL
- **API:** RESTful with JWT
- **Real-time:** WebSockets / Socket.io

---

## 👥 User Roles & Permissions

### 🎩 CEO (Executive)
**Focus:** Strategic oversight, business insights

**Can:**
- ✅ View executive dashboard
- ✅ Access all financial reports
- ✅ View analytics and trends
- ✅ Export reports

**Cannot:**
- ❌ Perform transactions
- ❌ Modify bookings
- ❌ Manage users

**Default View:** Reports & Analytics

---

### 👨‍💼 ADMIN (Operations Manager)
**Focus:** Full operational control

**Can:**
- ✅ All operational modules
- ✅ Room management
- ✅ Process transactions
- ✅ View reports
- ✅ Guest management
- ✅ View users

**Cannot:**
- ❌ Create CEO/ADMIN users
- ❌ Access system settings

**Default View:** Room Status Map

---

### 💻 IT STAFF (Technical Admin)
**Focus:** System administration and full system visibility

**Can:**
- ✅ Full user management (CRUD)
- ✅ System settings
- ✅ All operational modules
- ✅ Audit logs
- ✅ Create any user role
- ✅ **CEO Executive Dashboard** (full system insights for troubleshooting)

**Default View:** User Management Dashboard

**Note:** IT Staff have complete system visibility including executive dashboards for technical analysis, performance monitoring, and troubleshooting. Unlike CEO, IT maintains full operational capabilities.

---
---

## 🔐 Security Features

- Password hashing (production)
- Session management
- Auto-logout after inactivity
- Role-based access control (RBAC)
- 3 staff roles only (CEO, ADMIN, IT)
- Audit logging (planned)
- Input validation
- HTTPS/SSL (production)

---

## 📖 Documentation

### For Users:
- **Specification:** `.kiro/specs/internal-management-system.md`
  - Complete system documentation
  - Features, modules, data models
  - User guides per role

- **Tasks:** `.kiro/specs/tasks.md`
  - Implementation roadmap
  - Phase-by-phase tasks
  - Priority matrix

### For Developers:
- Code comments in HTML files
- Data structure definitions
- API endpoint specifications (future)

---

## 🎯 Development Roadmap

### ✅ Phase 1: Foundation (COMPLETED)
- Authentication system
- Role-based access control
- User management
- Basic room management
- Transaction processing
- Basic reporting

### 🔨 Phase 2: Core Features (IN PROGRESS)
- CEO executive dashboard
- Enhanced booking management
- Guest/CRM management
- Advanced financial reporting
- Export functionality

### 🔮 Phase 3: Advanced Features (PLANNED)
- System settings module
- Inventory management
- Audit logs
- Email notifications
- Backup/restore

### 🚀 Phase 4: Production (FUTURE)
- Backend API development
- Database migration
- Integration with customer website
- Performance optimization
- Production deployment

---

## 🧪 Testing

### Manual Testing:
1. **Test all staff roles (3 roles)**
   - Login with each role (CEO, ADMIN, IT)
   - Verify menu visibility
   - Test permissions

2. **Test workflows**
   - Room booking flow
   - Check-in/check-out flow
   - Transaction flow

3. **Test data persistence**
   - Refresh browser
   - Clear localStorage
   - Test data recovery

### Browser Compatibility:
- ✅ Chrome (recommended)
- ✅ Firefox
- ✅ Edge
- ⚠️ Safari (may have issues with localStorage)

---

## 🔧 Installation & Setup

### Development:
```bash
# 1. Clone or download the repository
# 2. Navigate to the project folder
cd cala-de-oro-demoResort

# 3. Open in browser
# - Windows: double-click index.html
# - Mac/Linux: open index.html

# Or use a local server:
# Python:
python -m http.server 8000

# Node.js:
npx http-server

# Then visit: http://localhost:8000
```

### Production (Future):
```bash
# 1. Set up backend server
# 2. Configure database
# 3. Update API endpoints
# 4. Deploy to hosting
# 5. Configure SSL certificate
# 6. Set up monitoring
```

---

## 📝 Data Storage

### Current (Demo):
- **localStorage** - Browser-based storage
- **sessionStorage** - User session
- Data persists across page refreshes
- Cleared when browser cache is cleared

### Production:
- **MySQL/PostgreSQL** - Relational database
- **Redis** - Session storage
- **Cloud Storage** - File uploads
- **Backups** - Automated daily backups

---

## 🐛 Known Issues

### Current Issues:
1. **localStorage Limitation**
   - Limited to ~5-10MB
   - Not suitable for production
   - No multi-user sync

2. **No Real-time Sync**
   - Changes not reflected across tabs
   - Requires page refresh

3. **No Backend Validation**
   - All validation is client-side
   - Can be bypassed

### Planned Fixes:
- Migrate to backend database
- Implement WebSocket for real-time updates
- Add server-side validation

---

## 🤝 Contributing

### How to Contribute:
1. Review the specification document
2. Check the tasks list
3. Pick a task to work on
4. Test thoroughly
5. Document your changes

### Coding Standards:
- Consistent indentation (2 or 4 spaces)
- Meaningful variable names
- Comment complex logic
- Follow existing code style

---

## 📞 Support

### For Issues:
- Check the specification document
- Review the tasks list
- Test in different browsers
- Clear browser cache and retry

### Contact:
- **Project Lead:** Aleson C. Irag
- **Organization:** Cala de Oro Resort

---

## 📄 License

This is a proprietary internal management system for Cala de Oro Resort.
All rights reserved.

---

## 🎉 Acknowledgments

- Developed with Kiro AI Assistant
- Built for Cala de Oro Resort
- Designed for efficient resort operations

---

## 📈 Version History

### Version 1.0 (Current)
- ✅ Authentication system
- ✅ Role-based access
- ✅ User management
- ✅ Room operations
- ✅ Transaction processing
- ✅ Basic reporting

### Version 1.1 (Planned)
- 🔨 CEO dashboard
- 🔨 Enhanced booking
- 🔨 CRM system
- 🔨 Advanced reports

### Version 2.0 (Future)
- 🔮 Backend API
- 🔮 Database integration
- 🔮 Customer website sync
- 🔮 Mobile app

---

**Happy Managing! 🏖️**

---

**Last Updated:** March 21, 2024  
**Next Milestone:** Complete Phase 2 Core Features  
**Target Date:** April 2024
