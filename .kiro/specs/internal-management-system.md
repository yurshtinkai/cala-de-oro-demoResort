# Cala de Oro - Internal Management System Specification

**Version:** 1.0  
**Date:** 2024-03-21  
**Status:** In Development

---

## 1. Executive Summary

### 1.1 Project Overview
The **Cala de Oro Internal Management System** is a staff-only web application designed to manage resort operations, monitor bookings, process transactions, generate financial reports, and provide business insights. This system is separate from the customer-facing booking website and serves as the operational backbone for staff members.

### 1.2 System Purpose
- Centralized dashboard for daily operations
- Real-time room status and booking management
- Transaction processing and financial reporting
- Role-based access control for different staff levels
- Business intelligence and analytics for executives

### 1.3 Target Users
- **CEO/Executives:** Strategic oversight, financial insights
- **Administrators:** Full operational control
- **IT Staff:** System maintenance, user management, full system visibility

**Note:** This is a staff-only internal system. Customers/guests use a separate booking website.

---

## 2. System Architecture

### 2.1 Current Architecture
```
┌─────────────────────────────────────────────────────────┐
│                 CUSTOMER BOOKING WEBSITE                 │
│                    (Separate System)                     │
└────────────────────┬────────────────────────────────────┘
                     │
                     ↓
            ┌────────────────────┐
            │  SHARED DATABASE   │ (Future Integration)
            │  - Bookings        │
            │  - Room Status     │
            │  - Guest Data      │
            └────────────────────┘
                     ↑
                     │
┌────────────────────┴────────────────────────────────────┐
│         INTERNAL MANAGEMENT SYSTEM (This Project)        │
├──────────────────────────────────────────────────────────┤
│  Frontend: HTML5, CSS3, JavaScript                       │
│  Storage: localStorage (Demo) / MySQL (Production)       │
│  Authentication: Session-based                           │
│  Architecture: Single-page application                   │
└──────────────────────────────────────────────────────────┘
```

### 2.2 Technology Stack

**Current Implementation:**
- **Frontend:** HTML5, CSS3, Vanilla JavaScript
- **Storage:** localStorage (demo/prototype)
- **Authentication:** sessionStorage
- **Architecture:** Single-page application (SPA)

**Production Recommendation:**
- **Backend:** Node.js + Express / PHP + Laravel
- **Database:** MySQL / PostgreSQL
- **API:** RESTful API with JWT authentication
- **Real-time:** Socket.io / WebSockets
- **Hosting:** Internal network / Cloud with VPN

---

## 3. User Roles & Permissions

### 3.1 Role Hierarchy

```
CEO (Executive)
    ↓
ADMIN (Operations Manager)
    ↓
IT STAFF (Technical Admin)
```

**Total Roles:** 3 (Staff Only)

### 3.2 Role Definitions

#### 3.2.1 CEO Role
**Purpose:** Executive oversight and strategic decision-making

**Permissions:**
- ✅ View executive dashboard with KPIs
- ✅ Access all financial reports (RESTRICTED ACCESS)
- ✅ View revenue analytics and trends
- ✅ Access occupancy reports
- ✅ Export reports (PDF, Excel)
- 👁️ Read-only access to operational data
- ❌ Cannot perform transactions
- ❌ Cannot modify bookings
- ❌ Cannot manage users

**Security:** Only CEO and ADMIN roles can access Reports & Analytics section.

**Default View:** Reports & Analytics

---

#### 3.2.2 ADMIN Role
**Purpose:** Full operational control and staff supervision

**Permissions:**
- ✅ All operational modules
- ✅ Room management (check-in/out, status)
- ✅ Process all transactions
- ✅ View financial reports (RESTRICTED ACCESS)
- ✅ Guest management
- ✅ Booking modifications
- ✅ Inventory management
- ✅ View user list
- ⚠️ Limited user creation (not CEO/ADMIN roles)
- ❌ Cannot access system settings

**Security:** Only CEO and ADMIN roles can access Reports & Analytics section.

**Default View:** Room Status Map

---

#### 3.2.3 IT STAFF Role
**Purpose:** System administration and technical support

**Permissions:**
- ✅ Full user management (CRUD operations)
- ✅ System settings and configuration
- ✅ Database backups
- ✅ Audit logs and security
- ✅ All operational modules (for troubleshooting)
- ✅ Integration monitoring
- ✅ Technical reports
- ✅ Create users with any role
- ❌ **Cannot access Financial Reports & Analytics** (restricted to CEO and ADMIN only)

**Security:** IT Staff does NOT have access to the CEO Executive Dashboard or Financial Reports for security and separation of duties. Only CEO and ADMIN roles can view financial data.

**Default View:** User Management Dashboard

**Note:** IT Staff maintains full operational and system administration capabilities but financial reporting access is restricted to maintain proper separation of duties and data security.

---

---

## 4. Core Modules

### 4.1 Authentication Module ✅ (Completed)

**Features:**
- Secure login with email/password
- Password visibility toggle
- Forgot password functionality
- Session management
- Auto-logout on inactivity
- Role-based redirection

**Credentials (Demo - Staff Only):**
- CEO: ceo@caladeoro.com / ceo123
- ADMIN: admin@caladeoro.com / admin123
- IT: it@caladeoro.com / it123

---

### 4.2 Dashboard Module (Role-Based)

#### 4.2.1 CEO Dashboard
**Components:**
- Revenue summary cards (monthly, yearly)
- Occupancy rate indicator
- Revenue trend chart (last 12 months)
- Occupancy trend chart
- Top performing rooms
- Revenue split breakdown (CALA vs Vanessa)
- Quick actions (export reports, print)

#### 4.2.2 ADMIN Dashboard
**Components:**
- Today's statistics cards
  - Check-ins today
  - Check-outs today
  - Available rooms
  - Rooms in cleaning
- Upcoming bookings list
- Recent transactions
- Pending tasks
- Staff on duty
- Quick actions (new booking, check-in)

#### 4.2.3 IT Dashboard
**Components:**
- System health indicators
- Active users count
- Database status
- Recent user activity
- System alerts/warnings
- Integration status
- Backup status
- Quick actions (user management, settings)

---

### 4.3 Room Management Module ✅ (Partially Completed)

**Current Features:**
- Room status map (visual overview)
- Room categories view
- Room table with details
- Check-in / Check-out
- Room status updates (Available, Occupied, Booked, Cleaning)
- Guest information
- Excess pax calculation

**Enhancements Needed:**
- Booking history per room
- Maintenance scheduling
- Housekeeping assignments
- Room notes/issues
- Rate modifications
- Seasonal pricing

---

### 4.4 Booking Management Module 🔨 (To Build)

**Features Required:**
- View all bookings (table view)
- Filter by:
  - Date range
  - Status (Confirmed, Pending, Cancelled)
  - Source (Online, Walk-in, Phone)
  - Room type
- Booking details modal
- Modify booking
- Cancel booking
- Add notes
- Guest contact information
- Payment status
- Booking timeline

**Data Structure:**
```javascript
{
  bookingId: "BK-2024-0001",
  guestName: "Juan Dela Cruz",
  email: "juan@email.com",
  phone: "+63 912 345 6789",
  checkIn: "2024-03-25",
  checkOut: "2024-03-27",
  room: "Duplex 1",
  guests: 4,
  status: "Confirmed",
  source: "Online",
  totalAmount: 6000,
  paid: 3000,
  balance: 3000,
  notes: "",
  createdAt: "2024-03-21 10:30 AM",
  createdBy: "user@caladeoro.com"
}
```

---

### 4.5 Transaction Module ✅ (Partially Completed)

**Current Features:**
- Room rentals
- Entrance fees (day/night tour)
- Corkage fees
- Tent rentals & equipment
- Resort outlets sales
- Custom items
- Transaction log
- Revenue split calculation (CALA vs Vanessa)

**Enhancements Needed:**
- Payment methods (Cash, Card, GCash, Bank Transfer)
- Receipt printing
- Refund processing
- Transaction editing (admin only)
- Payment history per guest
- Discount/promo codes

---

### 4.6 Financial Reporting Module ✅ (Partially Completed)

**Current Features:**
- Live transaction log
- Revenue split report (CALA vs Vanessa)
- Customer total
- Printable reports

**Enhancements Needed:**
- Date range selection
- Report filters (by category, by staff)
- Charts and graphs
  - Revenue trend (line chart)
  - Revenue by category (pie chart)
  - Occupancy rate (area chart)
  - Monthly comparison (bar chart)
- Export to Excel/CSV
- Scheduled reports
- Email reports
- Tax calculations
- Summary reports:
  - Daily sales report
  - Monthly revenue report
  - Occupancy report
  - Performance report

**Report Types:**
1. Daily Sales Summary
2. Revenue by Category
3. Occupancy Report
4. Room Performance
5. Staff Performance
6. Owner Revenue Split
7. Tax Report
8. Custom Date Range Report

---

### 4.7 Guest Management (CRM) Module 🔨 (To Build)

**Features Required:**
- Guest database
- Guest profile:
  - Personal information
  - Contact details
  - Booking history
  - Total spent
  - Preferences
  - Special requests
  - Notes
- Search and filter
- Guest communication log
- Loyalty points (future)
- Birthday notifications
- Repeat guest identification

**Data Structure:**
```javascript
{
  guestId: "G-0001",
  firstName: "Juan",
  lastName: "Dela Cruz",
  email: "juan@email.com",
  phone: "+63 912 345 6789",
  address: "Manila, Philippines",
  dateOfBirth: "1990-05-15",
  bookings: [
    {bookingId: "BK-2024-0001", date: "2024-03-25"},
    {bookingId: "BK-2023-0250", date: "2023-12-20"}
  ],
  totalBookings: 2,
  totalSpent: 15000,
  preferences: "Quiet room, ground floor",
  notes: "VIP guest, prefers early check-in",
  createdAt: "2023-12-20",
  lastVisit: "2024-03-25"
}
```

---

### 4.8 User Management Module ✅ (Completed)

**Current Features:**
- User listing (table view)
- Add new user
- Edit user details
- Delete user
- Role assignment
- Status management (Active/Inactive)
- Search and filter
- Statistics dashboard

**File:** `admin-dashboard.html`

---

### 4.9 System Settings Module 🔨 (To Build)

**Features Required:**

#### 4.9.1 Company Settings
- Resort name
- Address
- Contact information
- Logo upload
- Tax ID
- Business permits

#### 4.9.2 Room Configuration
- Room types management
- Pricing management
- Seasonal rates
- Capacity limits
- Amenities list

#### 4.9.3 Payment Settings
- Accepted payment methods
- Tax rates (VAT, Local Tax)
- Service charges
- Revenue split percentages
- Currency settings

#### 4.9.4 Email Templates
- Booking confirmation
- Payment receipt
- Cancellation notice
- Reminder emails

#### 4.9.5 System Configuration
- Session timeout
- Auto-logout duration
- Backup schedule
- Date/time format
- Language settings

---

### 4.10 Inventory Management Module 🔨 (To Build)

**Features Required:**
- Equipment tracking
  - Tents
  - Tables
  - Chairs
  - Water sports equipment
- Stock management
  - Linens
  - Toiletries
  - Cleaning supplies
- Low stock alerts
- Purchase orders
- Supplier management
- Stock movement log

---

### 4.11 Audit & Logs Module 🔨 (To Build)

**Features Required:**
- User activity logs
- Login/logout history
- Transaction logs
- System changes
- Security events
- Data exports
- Filter and search
- Export logs

---

## 5. UI/UX Design Guidelines

### 5.1 Design Principles
- **Consistency:** Uniform design across all modules
- **Clarity:** Clear labels, intuitive navigation
- **Efficiency:** Minimal clicks to complete tasks
- **Responsiveness:** Works on desktop, tablet, mobile
- **Accessibility:** WCAG 2.1 AA compliance

### 5.2 Color Scheme
```css
--primary-bg: #f4f7f6 (Light gray background)
--sidebar-bg: #2c3e50 (Dark blue-gray)
--accent-color: #3498db (Blue)
--success-color: #27ae60 (Green)
--warning-color: #f39c12 (Orange)
--danger-color: #e74c3c (Red)
--text-color: #333 (Dark gray)
```

### 5.3 Typography
- **Primary Font:** 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif
- **Headings:** Bold, larger size
- **Body Text:** 1rem, line-height 1.6
- **Small Text:** 0.85-0.9rem

### 5.4 Components

#### Buttons
- Primary: Blue background, white text
- Success: Green background, white text
- Danger: Red background, white text
- Secondary: Gray background, white text

#### Cards
- White background
- Subtle shadow
- Rounded corners (8px)
- 4px colored top border

#### Tables
- Striped rows (optional)
- Hover effect
- Sortable headers
- Fixed header on scroll

#### Modals
- Centered overlay
- Dark background (50% opacity)
- White content box
- Close button (X)
- Smooth animation

---

## 6. Data Models

### 6.1 User Model
```javascript
{
  id: Number,
  fullName: String,
  email: String (unique),
  password: String (hashed),
  role: Enum ['CEO', 'ADMIN', 'IT', 'USER'],
  status: Enum ['active', 'inactive'],
  dateCreated: Date,
  lastLogin: Date,
  createdBy: String (email)
}
```

### 6.2 Room Model
```javascript
{
  id: String,
  name: String,
  category: Enum ['Duplex', 'Row House', 'AC A-Frame', 'Regular A-Frame'],
  baseCapacity: Number,
  maxCapacity: Number,
  basePrice: Number,
  excessRate: Number,
  isVanessa: Boolean, // Revenue split 70/30
  status: Enum ['available', 'occupied', 'booked', 'cleaning'],
  checkIn: Date,
  checkOut: Date,
  guestDisplay: String,
  amenities: Array,
  notes: String
}
```

### 6.3 Booking Model
```javascript
{
  bookingId: String (unique),
  guestId: String (foreign key),
  roomId: String (foreign key),
  checkIn: Date,
  checkOut: Date,
  guests: Number,
  status: Enum ['Pending', 'Confirmed', 'Cancelled', 'Completed'],
  source: Enum ['Online', 'Walk-in', 'Phone', 'Email'],
  totalAmount: Number,
  paidAmount: Number,
  balance: Number,
  paymentMethod: Enum ['Cash', 'Card', 'GCash', 'Bank Transfer'],
  notes: String,
  specialRequests: String,
  createdAt: DateTime,
  createdBy: String (user email),
  modifiedAt: DateTime,
  modifiedBy: String
}
```

### 6.4 Transaction Model
```javascript
{
  transactionId: String (unique),
  timestamp: DateTime,
  itemName: String,
  category: Enum ['Room', 'Entrance', 'Corkage', 'Tent', 'Outlet', 'Other'],
  quantity: Number,
  unitPrice: Number,
  totalAmount: Number,
  resortShare: Number,
  vanessaShare: Number,
  paymentMethod: Enum ['Cash', 'Card', 'GCash', 'Bank Transfer'],
  processedBy: String (user email),
  bookingId: String (optional),
  notes: String
}
```

### 6.5 Guest Model
```javascript
{
  guestId: String (unique),
  firstName: String,
  lastName: String,
  email: String,
  phone: String,
  address: String,
  dateOfBirth: Date,
  totalBookings: Number,
  totalSpent: Number,
  lastVisit: Date,
  preferences: String,
  notes: String,
  createdAt: DateTime
}
```

---

## 7. API Endpoints (Future Backend)

### 7.1 Authentication
- `POST /api/auth/login` - User login
- `POST /api/auth/logout` - User logout
- `POST /api/auth/forgot-password` - Request password reset
- `POST /api/auth/reset-password` - Reset password

### 7.2 Users
- `GET /api/users` - Get all users (IT/ADMIN)
- `GET /api/users/:id` - Get user by ID
- `POST /api/users` - Create new user
- `PUT /api/users/:id` - Update user
- `DELETE /api/users/:id` - Delete user

### 7.3 Rooms
- `GET /api/rooms` - Get all rooms
- `GET /api/rooms/:id` - Get room by ID
- `PUT /api/rooms/:id/status` - Update room status
- `GET /api/rooms/available` - Get available rooms

### 7.4 Bookings
- `GET /api/bookings` - Get all bookings
- `GET /api/bookings/:id` - Get booking by ID
- `POST /api/bookings` - Create new booking
- `PUT /api/bookings/:id` - Update booking
- `DELETE /api/bookings/:id` - Cancel booking

### 7.5 Transactions
- `GET /api/transactions` - Get all transactions
- `GET /api/transactions/:id` - Get transaction by ID
- `POST /api/transactions` - Create transaction
- `GET /api/transactions/report` - Generate report

### 7.6 Guests
- `GET /api/guests` - Get all guests
- `GET /api/guests/:id` - Get guest by ID
- `POST /api/guests` - Create guest
- `PUT /api/guests/:id` - Update guest

---

## 8. Security Requirements

### 8.1 Authentication & Authorization
- Password hashing (bcrypt)
- Session management
- JWT tokens (production)
- Role-based access control
- Auto-logout after 30 minutes inactivity

### 8.2 Data Protection
- HTTPS/SSL required
- Input validation and sanitization
- SQL injection prevention
- XSS protection
- CSRF tokens

### 8.3 Audit Trail
- Log all user actions
- Track data modifications
- Security event logging
- Regular audit reviews

---

## 9. Testing Strategy

### 9.1 Unit Testing
- Test individual functions
- Data validation
- Calculation accuracy

### 9.2 Integration Testing
- Module interactions
- API endpoints
- Database operations

### 9.3 User Acceptance Testing
- Role-based workflows
- Real-world scenarios
- Performance testing

---

## 10. Deployment Plan

### 10.1 Development Phase (Current)
- localhost testing
- localStorage for data
- Demo credentials

### 10.2 Staging Phase
- Internal network deployment
- Test database
- User training

### 10.3 Production Phase
- Production server setup
- Database migration
- Real user accounts
- Backup systems
- Monitoring setup

---

## 11. Future Enhancements

### 11.1 Phase 2
- Mobile app (iOS/Android)
- Two-factor authentication
- Advanced analytics with ML
- Customer feedback system
- Integration with accounting software

### 11.2 Phase 3
- Multi-property support
- API for third-party integrations
- Automated marketing campaigns
- Loyalty program
- Channel manager integration

---

## 12. Development Roadmap

### ✅ Phase 1 - Foundation (Completed)
- [x] Authentication system
- [x] Role-based access control
- [x] User management dashboard
- [x] Basic room management
- [x] Transaction processing
- [x] Basic reporting

### 🔨 Phase 2 - Core Features (Next)
- [ ] Enhanced CEO dashboard with charts
- [ ] Complete booking management module
- [ ] Guest/CRM management
- [ ] Advanced financial reporting
- [ ] Export functionality (PDF, Excel)

### 🔨 Phase 3 - Advanced Features
- [ ] System settings module
- [ ] Inventory management
- [ ] Audit logs
- [ ] Email notifications
- [ ] Backup/restore functionality

### 🔨 Phase 4 - Integration & Optimization
- [ ] Backend API development
- [ ] Database migration
- [ ] Integration with customer website
- [ ] Performance optimization
- [ ] Mobile responsiveness

---

## 13. Support & Maintenance

### 13.1 Documentation
- User manual for each role
- Video tutorials
- FAQ section
- Troubleshooting guide

### 13.2 Training
- Staff training sessions
- Role-specific tutorials
- Hands-on practice
- Ongoing support

### 13.3 Maintenance
- Regular backups
- Security updates
- Bug fixes
- Feature updates

---

## 14. Success Metrics

### 14.1 KPIs
- User adoption rate
- Time to complete tasks
- Data accuracy
- System uptime
- User satisfaction

### 14.2 Goals
- 100% staff adoption within 1 month
- 50% reduction in manual paperwork
- Real-time data accuracy
- 99.9% system uptime

---

## 15. Contact & Support

**Project Team:**
- Development: Kiro AI Assistant
- Stakeholder: Aleson C. Irag
- Organization: Cala de Oro Resort

**System Access:**
- Internal Management: http://localhost/index.html
- Admin Dashboard: http://localhost/admin-dashboard.html

---

**Document Status:** Living Document - Updated as features are developed
**Last Updated:** March 21, 2024
**Next Review:** After Phase 2 completion
