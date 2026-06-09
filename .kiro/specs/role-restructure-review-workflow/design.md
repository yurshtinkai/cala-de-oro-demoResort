# Design Document: Role Restructure and Review Workflow

## 1. Overview

This document provides the technical design for restructuring the Cala de Oro Internal Management System's role hierarchy, renaming ADMIN to MANAGER, implementing data entry workflows for IT Staff, and establishing supervisory review capabilities for Managers.

### 1.1 Design Goals

- Seamless migration of ADMIN role to MANAGER
- Enforce data entry workflow with submission tracking
- Implement comprehensive audit trail for accountability
- Maintain system performance with new permission checks
- Preserve backward compatibility during transition

### 1.2 Design Constraints

- localStorage-based storage (demo environment)
- sessionStorage authentication
- Single-page application architecture
- No page reloads for role permission updates
- Must work with existing transaction, booking, and room modules

---

## 2. High-Level Architecture

### 2.1 Updated Role Hierarchy

```
┌─────────────────────────────────────────────────────────┐
│                    CEO (Executive)                       │
│                                                          │
│  Permissions:                                            │
│  • View financial reports (read-only)                    │
│  • View analytics dashboard                              │
│  • View audit trails                                     │
│  • NO data entry or modifications                        │
└─────────────────────────────────────────────────────────┘
                           ↑
                           │ Reports To
                           │
┌─────────────────────────────────────────────────────────┐
│              MANAGER (formerly ADMIN)                    │
│                                                          │
│  Permissions:                                            │
│  • View all IT Staff submissions                         │
│  • Edit any operational record                           │
│  • Delete any operational record                         │
│  • View financial reports                                │
│  • Manage user accounts                                  │
│  • Review audit trails                                   │
│  • Full operational control                              │
└─────────────────────────────────────────────────────────┘
                           ↑
                           │ Submits To
                           │
┌─────────────────────────────────────────────────────────┐
│                  IT STAFF (Data Entry)                   │
│                                                          │
│  Permissions:                                            │
│  • Create transactions                                   │
│  • Create bookings                                       │
│  • Check-in / Check-out guests                           │
│  • Create sales records                                  │
│  • View own submission history (read-only)               │
│  • NO edit/delete after submission                       │
│  • NO reports access                                     │
│  • NO user management                                    │
└─────────────────────────────────────────────────────────┘
```

### 2.2 System Component Architecture

```
┌─────────────────────────────────────────────────────────┐
│                    UI Layer                              │
│  ┌─────────────┐  ┌──────────────┐  ┌────────────────┐ │
│  │ IT Staff    │  │ Manager      │  │ CEO            │ │
│  │ Dashboard   │  │ Dashboard    │  │ Dashboard      │ │
│  └─────────────┘  └──────────────┘  └────────────────┘ │
└───────────────────────┬─────────────────────────────────┘
                        ↓
┌─────────────────────────────────────────────────────────┐
│              Permission Control Layer                    │
│  ┌──────────────────┐  ┌──────────────────┐             │
│  │ Role Checker     │  │ Permission       │             │
│  │ Service          │  │ Validator        │             │
│  └──────────────────┘  └──────────────────┘             │
└───────────────────────┬─────────────────────────────────┘
                        ↓
┌─────────────────────────────────────────────────────────┐
│                Business Logic Layer                      │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐  │
│  │ Transaction  │  │ Booking      │  │ Room         │  │
│  │ Manager      │  │ Manager      │  │ Manager      │  │
│  └──────────────┘  └──────────────┘  └──────────────┘  │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐  │
│  │ Audit Trail  │  │ Notification │  │ User         │  │
│  │ Logger       │  │ Service      │  │ Manager      │  │
│  └──────────────┘  └──────────────┘  └──────────────┘  │
└───────────────────────┬─────────────────────────────────┘
                        ↓
┌─────────────────────────────────────────────────────────┐
│                  Data Access Layer                       │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐  │
│  │ Data Entry   │  │ Audit Trail  │  │ User         │  │
│  │ Repository   │  │ Repository   │  │ Repository   │  │
│  └──────────────┘  └──────────────┘  └──────────────┘  │
└───────────────────────┬─────────────────────────────────┘
                        ↓
┌─────────────────────────────────────────────────────────┐
│                  localStorage                            │
│  • transactions    • auditTrail    • users               │
│  • bookings        • notifications • sessions            │
└─────────────────────────────────────────────────────────┘
```

### 2.3 Data Flow: IT Staff Creates Record

```
IT Staff UI → Permission Check (IT Staff = CREATE) → Business Logic
     ↓
Create Record with Metadata (createdBy, createdAt)
     ↓
Save to localStorage → Log to Audit Trail → Notify Manager
     ↓
Record Status: "Submitted" (Read-only for IT Staff)
```

### 2.4 Data Flow: Manager Edits Record

```
Manager UI → Permission Check (Manager = EDIT) → Load Record
     ↓
Display Editable Form with Original Creator Info
     ↓
Manager Makes Changes → Save Record
     ↓
Update Record (editedBy, editedAt, editCount++) → Log to Audit Trail
     ↓
Record remains "Submitted" with Edit History
```

---

## 3. Database Schema Design

### 3.1 Enhanced Transaction Model

```javascript
{
  transactionId: String,        // Unique ID (existing)
  timestamp: DateTime,          // Transaction time (existing)
  itemName: String,             // Item description (existing)
  category: String,             // Transaction category (existing)
  quantity: Number,             // Quantity (existing)
  unitPrice: Number,            // Unit price (existing)
  totalAmount: Number,          // Total (existing)
  resortShare: Number,          // Revenue split (existing)
  vanessaShare: Number,         // Revenue split (existing)
  paymentMethod: String,        // Payment method (existing)
  
  // NEW METADATA FIELDS
  createdBy: String,            // User email who created
  createdByName: String,        // User full name
  createdAt: DateTime,          // Creation timestamp
  editedBy: String,             // User email who last edited (null if never edited)
  editedByName: String,         // User full name who edited
  editedAt: DateTime,           // Last edit timestamp (null if never edited)
  editCount: Number,            // Number of times edited (default: 0)
  status: String,               // "submitted", "edited", "deleted"
  notes: String                 // Optional notes (existing)
}
```

### 3.2 Audit Trail Model

```javascript
{
  auditId: String,              // Unique audit entry ID
  recordType: String,           // "transaction", "booking", "room", etc.
  recordId: String,             // ID of the affected record
  action: String,               // "CREATE", "UPDATE", "DELETE"
  performedBy: String,          // User email who performed action
  performedByName: String,      // User full name
  performedAt: DateTime,        // When action occurred
  changes: Array<{              // Array of field changes
    field: String,              // Field name that changed
    oldValue: Any,              // Previous value
    newValue: Any               // New value
  }>,
  snapshot: Object,             // Full record snapshot (for DELETE)
  ipAddress: String,            // User IP (optional, for security)
  userAgent: String             // Browser info (optional)
}
```

### 3.3 User Model Updates

```javascript
{
  id: Number,                   // User ID (existing)
  fullName: String,             // Full name (existing)
  email: String,                // Email (existing)
  password: String,             // Hashed password (existing)
  role: String,                 // "CEO", "MANAGER", "IT" (ADMIN → MANAGER)
  status: String,               // "active", "inactive" (existing)
  dateCreated: Date,            // Account creation (existing)
  lastLogin: Date,              // Last login time (existing)
  createdBy: String             // Who created account (existing)
}
```

### 3.4 Notification Model

```javascript
{
  notificationId: String,       // Unique notification ID
  recipientRole: String,        // "MANAGER" (who should see it)
  recipientEmail: String,       // Specific user email (optional)
  type: String,                 // "NEW_SUBMISSION", "PENDING_REVIEW"
  recordType: String,           // "transaction", "booking", etc.
  recordId: String,             // ID of related record
  createdBy: String,            // Who triggered notification
  createdByName: String,        // Creator's full name
  message: String,              // Notification message
  isRead: Boolean,              // Read status (default: false)
  createdAt: DateTime,          // When notification created
  readAt: DateTime              // When marked as read (null if unread)
}
```

---

## 4. Permission Matrix

### 4.1 Complete Permission Table

| Operation                    | IT STAFF | MANAGER | CEO |
|------------------------------|----------|---------|-----|
| **Transactions**             |          |         |     |
| Create Transaction           | ✅       | ✅      | ❌  |
| View Own Transactions        | ✅       | ✅      | ✅  |
| View All Transactions        | ❌       | ✅      | ✅  |
| Edit Transaction             | ❌       | ✅      | ❌  |
| Delete Transaction           | ❌       | ✅      | ❌  |
| **Bookings**                 |          |         |     |
| Create Booking               | ✅       | ✅      | ❌  |
| View Own Bookings            | ✅       | ✅      | ✅  |
| View All Bookings            | ❌       | ✅      | ✅  |
| Edit Booking                 | ❌       | ✅      | ❌  |
| Cancel/Delete Booking        | ❌       | ✅      | ❌  |
| **Room Management**          |          |         |     |
| View Room Status             | ✅       | ✅      | ✅  |
| Check-in Guest               | ✅       | ✅      | ❌  |
| Check-out Guest              | ✅       | ✅      | ❌  |
| Edit Room Details            | ❌       | ✅      | ❌  |
| Change Room Status           | ✅       | ✅      | ❌  |
| **Reports & Analytics**      |          |         |     |
| View Financial Reports       | ❌       | ✅      | ✅  |
| View Analytics Dashboard     | ❌       | ✅      | ✅  |
| Export Reports               | ❌       | ✅      | ✅  |
| **Audit & History**          |          |         |     |
| View Own Submission History  | ✅       | ✅      | ❌  |
| View All Audit Trails        | ❌       | ✅      | ✅  |
| View Record Edit History     | ❌       | ✅      | ✅  |
| **User Management**          |          |         |     |
| View User List               | ❌       | ✅      | ❌  |
| Create IT Staff Account      | ❌       | ✅      | ❌  |
| Create Manager Account       | ❌       | ✅      | ❌  |
| Create CEO Account           | ❌       | ❌      | ❌  |
| Edit User Details            | ❌       | ✅      | ❌  |
| Deactivate User              | ❌       | ✅      | ❌  |
| **Notifications**            |          |         |     |
| Receive Submission Alerts    | ❌       | ✅      | ❌  |
| View Notification Count      | ❌       | ✅      | ❌  |
| Mark as Reviewed             | ❌       | ✅      | ❌  |

---

## 5. UI/UX Design

### 5.1 IT Staff Dashboard Layout

```
┌─────────────────────────────────────────────────────────┐
│  CALA DE ORO - IT Staff Dashboard    Logged in: IT Name │
├─────────────────────────────────────────────────────────┤
│  🗺️ Room Map   🛏️ Rooms   🎟️ Entrance   🍾 Corkage    │
│  ⛺ Tents   🏪 Outlets   🛒 Other   📋 My Submissions    │
├─────────────────────────────────────────────────────────┤
│                                                          │
│  Quick Stats (Today):                                    │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐     │
│  │ Transactions│  │ Check-ins   │  │ Total Sales │     │
│  │     12      │  │      3      │  │  ₱ 8,500    │     │
│  └─────────────┘  └─────────────┘  └─────────────┘     │
│                                                          │
│  Recent Submissions:                                     │
│  ┌───────────────────────────────────────────────┐      │
│  │ Time     │ Type        │ Amount   │ Status    │      │
│  ├───────────────────────────────────────────────┤      │
│  │ 10:30 AM │ Transaction │ ₱ 1,200  │ ✅ Submitted│     │
│  │ 10:15 AM │ Check-in    │ -        │ ✅ Submitted│     │
│  │ 09:45 AM │ Transaction │ ₱ 3,500  │ ✏️ Edited   │     │
│  └───────────────────────────────────────────────┘      │
│                                                          │
│  ℹ️ Note: Records marked ✅ are submitted and read-only │
│          Records marked ✏️ were modified by Manager     │
└─────────────────────────────────────────────────────────┘
```

### 5.2 Manager Dashboard Layout

```
┌─────────────────────────────────────────────────────────┐
│  CALA DE ORO - Manager Dashboard  Logged in: Mgr Name  │
├─────────────────────────────────────────────────────────┤
│  🗺️ Room Map   📊 Reports   👥 Users   🔔 Pending (5)  │
├─────────────────────────────────────────────────────────┤
│                                                          │
│  Pending Review:  [Filter ▼] [Date Range]  [Search 🔍]  │
│  ┌───────────────────────────────────────────────────┐  │
│  │ Time  │ Staff │ Type    │ Amount │ Actions        │  │
│  ├───────────────────────────────────────────────────┤  │
│  │ 10:30 │ Juan  │ Trans   │ ₱1,200 │ [View] [Edit] [Del]│  │
│  │ 10:15 │ Maria │ CheckIn │ -      │ [View] [Edit] [Del]│  │
│  │ 09:45 │ Juan  │ Trans   │ ₱3,500 │ [View] [Edit] [Del]│  │
│  │ 09:20 │ Pedro │ Booking │ ₱6,000 │ [View] [Edit] [Del]│  │
│  │ 09:00 │ Maria │ Trans   │ ₱800   │ [View] [Edit] [Del]│  │
│  └───────────────────────────────────────────────────┘  │
│                                                          │
│  Today's Summary:                                        │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐     │
│  │ Submissions │  │ Edited      │  │ Total       │     │
│  │     18      │  │      3      │  │  ₱ 45,200   │     │
│  └─────────────┘  └─────────────┘  └─────────────┘     │
└─────────────────────────────────────────────────────────┘
```

### 5.3 Record Edit Modal (Manager View)

```
┌─────────────────────────────────────────┐
│  Edit Transaction                    [X] │
├─────────────────────────────────────────┤
│                                          │
│  Original Entry Information:             │
│  Created by: Juan Dela Cruz (IT Staff)   │
│  Created at: June 9, 2026 10:30 AM      │
│  Edit count: 0 edits                     │
│                                          │
│  ─────────────────────────────────────   │
│                                          │
│  Item Name: *                            │
│  [Day Tour - Adult____________]          │
│                                          │
│  Quantity: *                             │
│  [5_____]                                │
│                                          │
│  Unit Price: *                           │
│  [₱ 100.00______]                        │
│                                          │
│  Total Amount: ₱ 500.00 (calculated)     │
│                                          │
│  Payment Method: *                       │
│  [Cash ▼]                                │
│                                          │
│  Notes: (optional)                       │
│  [Corrected quantity from 4 to 5____]    │
│                                          │
│  ─────────────────────────────────────   │
│                                          │
│  [View Audit History]                    │
│                                          │
│  [ Cancel ]         [ Save Changes ]     │
│                                          │
└─────────────────────────────────────────┘
```

### 5.4 Audit Trail Viewer

```
┌──────────────────────────────────────────────┐
│  Audit History: Transaction #TXN-2026-0142  │
├──────────────────────────────────────────────┤
│                                              │
│  ✅ CREATED                                  │
│  By: Juan Dela Cruz (it@caladeoro.com)      │
│  Date: June 9, 2026 10:30:15 AM             │
│  ─────────────────────────────────────────  │
│  Item Name: Day Tour - Adult                │
│  Quantity: 4                                │
│  Unit Price: ₱ 100.00                       │
│  Total: ₱ 400.00                            │
│  Payment: Cash                              │
│                                              │
│  ✏️ EDITED                                   │
│  By: Maria Santos (manager@caladeoro.com)   │
│  Date: June 9, 2026 11:15:42 AM             │
│  ─────────────────────────────────────────  │
│  Changes:                                   │
│    • Quantity: 4 → 5                        │
│    • Total: ₱ 400.00 → ₱ 500.00             │
│  Notes: "Corrected quantity from 4 to 5"    │
│                                              │
│                                              │
│                           [ Close ]          │
└──────────────────────────────────────────────┘
```

### 5.5 My Submissions View (IT Staff)

```
┌─────────────────────────────────────────────────────────┐
│  My Submissions History                                  │
├─────────────────────────────────────────────────────────┤
│  Filter: [All Types ▼]  Date: [Last 7 Days ▼] [Search🔍]│
├─────────────────────────────────────────────────────────┤
│  Date/Time     │ Type        │ Details      │ Status    │
├─────────────────────────────────────────────────────────┤
│  Jun 9, 10:30  │ Transaction │ ₱ 1,200      │ 📋 Submit │
│  Jun 9, 10:15  │ Check-in    │ Duplex 1     │ 📋 Submit │
│  Jun 9, 09:45  │ Transaction │ ₱ 3,500      │ ✏️ Edited │
│                │             │              │ (By Mgr)  │
│  Jun 8, 16:20  │ Booking     │ BK-2026-0145 │ 📋 Submit │
│  Jun 8, 14:10  │ Transaction │ ₱ 800        │ 📋 Submit │
│  Jun 8, 11:05  │ Check-out   │ Row House 2  │ 📋 Submit │
└─────────────────────────────────────────────────────────┘

Legend:
📋 Submitted - Record saved, read-only
✏️ Edited - Manager modified this record
```

---

## 6. Low-Level Design

### 6.1 Permission Checking Algorithm

```javascript
/**
 * Permission Checker Service
 * Validates if user has permission to perform action
 */
function checkPermission(userId, action, resourceType, resourceId = null) {
    // Step 1: Get current user from session
    const currentUser = JSON.parse(sessionStorage.getItem('currentUser'));
    
    // Step 2: Validate session
    if (!currentUser || currentUser.id !== userId) {
        throw new Error('Invalid session. Please login again.');
    }
    
    // Step 3: Define permission matrix
    const permissions = {
        'CEO': {
            'transaction': ['VIEW_ALL', 'VIEW_AUDIT'],
            'booking': ['VIEW_ALL', 'VIEW_AUDIT'],
            'room': ['VIEW'],
            'report': ['VIEW', 'EXPORT'],
            'user': []
        },
        'MANAGER': {
            'transaction': ['CREATE', 'VIEW_ALL', 'EDIT', 'DELETE', 'VIEW_AUDIT'],
            'booking': ['CREATE', 'VIEW_ALL', 'EDIT', 'DELETE', 'VIEW_AUDIT'],
            'room': ['VIEW', 'EDIT', 'CHECKIN', 'CHECKOUT'],
            'report': ['VIEW', 'EXPORT'],
            'user': ['CREATE', 'VIEW', 'EDIT', 'DEACTIVATE'],
            'notification': ['VIEW', 'MARK_READ']
        },
        'IT': {
            'transaction': ['CREATE', 'VIEW_OWN'],
            'booking': ['CREATE', 'VIEW_OWN'],
            'room': ['VIEW', 'CHECKIN', 'CHECKOUT', 'UPDATE_STATUS'],
            'report': [],
            'user': [],
            'submission': ['VIEW_OWN_HISTORY']
        }
    };
    
    // Step 4: Get user's allowed actions for resource
    const userRole = currentUser.role;
    const allowedActions = permissions[userRole]?.[resourceType] || [];
    
    // Step 5: Check if action is allowed
    if (!allowedActions.includes(action)) {
        return {
            allowed: false,
            reason: `${userRole} role does not have permission to ${action} ${resourceType}`
        };
    }
    
    // Step 6: Additional checks for ownership-based permissions
    if (action === 'VIEW_OWN' && resourceId) {
        const record = getRecordById(resourceType, resourceId);
        if (record.createdBy !== currentUser.email) {
            return {
                allowed: false,
                reason: 'You can only view your own records'
            };
        }
    }
    
    // Step 7: Permission granted
    return {
        allowed: true,
        reason: null
    };
}

/**
 * Usage Examples:
 */
// IT Staff tries to create transaction
checkPermission(userId, 'CREATE', 'transaction');
// Result: { allowed: true, reason: null }

// IT Staff tries to edit transaction
checkPermission(userId, 'EDIT', 'transaction', 'TXN-123');
// Result: { allowed: false, reason: "IT role does not have permission..." }

// Manager tries to edit transaction
checkPermission(managerId, 'EDIT', 'transaction', 'TXN-123');
// Result: { allowed: true, reason: null }

// CEO tries to view report
checkPermission(ceoId, 'VIEW', 'report');
// Result: { allowed: true, reason: null }
```

### 6.2 Audit Trail Logger

```javascript
/**
 * Audit Trail Service
 * Logs all CRUD operations for accountability
 */
class AuditTrailService {
    
    constructor() {
        this.storageKey = 'auditTrail';
    }
    
    /**
     * Log a CREATE action
     */
    logCreate(recordType, recordId, recordData, userId) {
        const user = this._getUserInfo(userId);
        const auditEntry = {
            auditId: this._generateId(),
            recordType: recordType,
            recordId: recordId,
            action: 'CREATE',
            performedBy: user.email,
            performedByName: user.fullName,
            performedAt: new Date().toISOString(),
            changes: this._buildCreateChanges(recordData),
            snapshot: JSON.parse(JSON.stringify(recordData)),
            ipAddress: this._getClientIP(),
            userAgent: navigator.userAgent
        };
        
        this._saveAuditEntry(auditEntry);
        return auditEntry.auditId;
    }
    
    /**
     * Log an UPDATE action
     */
    logUpdate(recordType, recordId, oldData, newData, userId) {
        const user = this._getUserInfo(userId);
        const changes = this._buildUpdateChanges(oldData, newData);
        
        const auditEntry = {
            auditId: this._generateId(),
            recordType: recordType,
            recordId: recordId,
            action: 'UPDATE',
            performedBy: user.email,
            performedByName: user.fullName,
            performedAt: new Date().toISOString(),
            changes: changes,
            snapshot: null, // Not needed for updates
            ipAddress: this._getClientIP(),
            userAgent: navigator.userAgent
        };
        
        this._saveAuditEntry(auditEntry);
        return auditEntry.auditId;
    }
    
    /**
     * Log a DELETE action
     */
    logDelete(recordType, recordId, recordData, userId) {
        const user = this._getUserInfo(userId);
        
        const auditEntry = {
            auditId: this._generateId(),
            recordType: recordType,
            recordId: recordId,
            action: 'DELETE',
            performedBy: user.email,
            performedByName: user.fullName,
            performedAt: new Date().toISOString(),
            changes: [],
            snapshot: JSON.parse(JSON.stringify(recordData)), // Save full record
            ipAddress: this._getClientIP(),
            userAgent: navigator.userAgent
        };
        
        this._saveAuditEntry(auditEntry);
        return auditEntry.auditId;
    }
    
    /**
     * Get audit history for a specific record
     */
    getAuditHistory(recordType, recordId) {
        const allAudits = this._getAllAudits();
        return allAudits
            .filter(audit => audit.recordType === recordType && audit.recordId === recordId)
            .sort((a, b) => new Date(a.performedAt) - new Date(b.performedAt));
    }
    
    /**
     * Helper: Build changes array for CREATE
     */
    _buildCreateChanges(recordData) {
        const changes = [];
        for (const [field, value] of Object.entries(recordData)) {
            if (field !== 'createdBy' && field !== 'createdAt') {
                changes.push({
                    field: field,
                    oldValue: null,
                    newValue: value
                });
            }
        }
        return changes;
    }
    
    /**
     * Helper: Build changes array for UPDATE
     */
    _buildUpdateChanges(oldData, newData) {
        const changes = [];
        for (const [field, newValue] of Object.entries(newData)) {
            const oldValue = oldData[field];
            if (JSON.stringify(oldValue) !== JSON.stringify(newValue)) {
                // Exclude metadata fields from change tracking
                if (!['editedBy', 'editedAt', 'editCount', 'editedByName'].includes(field)) {
                    changes.push({
                        field: field,
                        oldValue: oldValue,
                        newValue: newValue
                    });
                }
            }
        }
        return changes;
    }
    
    /**
     * Helper: Get user info from session
     */
    _getUserInfo(userId) {
        const currentUser = JSON.parse(sessionStorage.getItem('currentUser'));
        if (!currentUser || currentUser.id !== userId) {
            throw new Error('Invalid user session');
        }
        return currentUser;
    }
    
    /**
     * Helper: Save audit entry to localStorage
     */
    _saveAuditEntry(auditEntry) {
        const audits = this._getAllAudits();
        audits.push(auditEntry);
        localStorage.setItem(this.storageKey, JSON.stringify(audits));
    }
    
    /**
     * Helper: Get all audit entries
     */
    _getAllAudits() {
        const data = localStorage.getItem(this.storageKey);
        return data ? JSON.parse(data) : [];
    }
    
    /**
     * Helper: Generate unique ID
     */
    _generateId() {
        return 'AUD-' + new Date().getFullYear() + '-' + 
               String(Date.now()).slice(-6) + '-' + 
               Math.random().toString(36).substr(2, 4).toUpperCase();
    }
    
    /**
     * Helper: Get client IP (placeholder for demo)
     */
    _getClientIP() {
        return '127.0.0.1'; // In production, get from server
    }
}
```

### 6.3 Record Metadata Manager

```javascript
/**
 * Add metadata to records when created or edited
 */
class RecordMetadataManager {
    
    /**
     * Prepare metadata for new record creation
     */
    static prepareCreateMetadata(userId) {
        const user = JSON.parse(sessionStorage.getItem('currentUser'));
        if (!user || user.id !== userId) {
            throw new Error('Invalid user session');
        }
        
        return {
            createdBy: user.email,
            createdByName: user.fullName,
            createdAt: new Date().toISOString(),
            editedBy: null,
            editedByName: null,
            editedAt: null,
            editCount: 0,
            status: 'submitted'
        };
    }
    
    /**
     * Prepare metadata for record update
     */
    static prepareUpdateMetadata(existingRecord, userId) {
        const user = JSON.parse(sessionStorage.getItem('currentUser'));
        if (!user || user.id !== userId) {
            throw new Error('Invalid user session');
        }
        
        return {
            ...existingRecord,
            editedBy: user.email,
            editedByName: user.fullName,
            editedAt: new Date().toISOString(),
            editCount: (existingRecord.editCount || 0) + 1,
            status: 'edited'
        };
    }
    
    /**
     * Check if user can edit record
     */
    static canEdit(record, userId) {
        const user = JSON.parse(sessionStorage.getItem('currentUser'));
        if (!user || user.id !== userId) {
            return false;
        }
        
        // Manager can edit any record
        if (user.role === 'MANAGER') {
            return true;
        }
        
        // IT Staff cannot edit submitted records
        if (user.role === 'IT' && record.status === 'submitted') {
            return false;
        }
        
        return false;
    }
    
    /**
     * Format metadata for display
     */
    static formatMetadataDisplay(record) {
        const created = new Date(record.createdAt).toLocaleString('en-US', {
            month: 'short',
            day: 'numeric',
            year: 'numeric',
            hour: 'numeric',
            minute: '2-digit',
            hour12: true
        });
        
        let display = `Created by ${record.createdByName} on ${created}`;
        
        if (record.editCount > 0 && record.editedBy) {
            const edited = new Date(record.editedAt).toLocaleString('en-US', {
                month: 'short',
                day: 'numeric',
                year: 'numeric',
                hour: 'numeric',
                minute: '2-digit',
                hour12: true
            });
            display += ` | Last edited by ${record.editedByName} on ${edited} (${record.editCount} edit${record.editCount > 1 ? 's' : ''})`;
        }
        
        return display;
    }
}
```

### 6.4 Notification Service

```javascript
/**
 * Notification Service for Manager alerts
 */
class NotificationService {
    
    constructor() {
        this.storageKey = 'notifications';
    }
    
    /**
     * Create notification when IT Staff submits record
     */
    notifyNewSubmission(recordType, recordId, creatorId) {
        const creator = this._getUserById(creatorId);
        
        const notification = {
            notificationId: this._generateId(),
            recipientRole: 'MANAGER',
            recipientEmail: null, // All managers
            type: 'NEW_SUBMISSION',
            recordType: recordType,
            recordId: recordId,
            createdBy: creator.email,
            createdByName: creator.fullName,
            message: `${creator.fullName} submitted a new ${recordType}`,
            isRead: false,
            createdAt: new Date().toISOString(),
            readAt: null
        };
        
        this._saveNotification(notification);
        this._updateNotificationBadge();
        return notification.notificationId;
    }
    
    /**
     * Get unread count for current user
     */
    getUnreadCount(userId) {
        const user = this._getUserById(userId);
        if (user.role !== 'MANAGER') {
            return 0;
        }
        
        const notifications = this._getAllNotifications();
        return notifications.filter(n => 
            !n.isRead && 
            (n.recipientRole === user.role || n.recipientEmail === user.email)
        ).length;
    }
    
    /**
     * Mark notification as read
     */
    markAsRead(notificationId, userId) {
        const notifications = this._getAllNotifications();
        const notification = notifications.find(n => n.notificationId === notificationId);
        
        if (notification && !notification.isRead) {
            notification.isRead = true;
            notification.readAt = new Date().toISOString();
            localStorage.setItem(this.storageKey, JSON.stringify(notifications));
            this._updateNotificationBadge();
        }
    }
    
    /**
     * Mark multiple notifications as read in batch
     */
    markMultipleAsRead(notificationIds, userId) {
        notificationIds.forEach(id => this.markAsRead(id, userId));
    }
    
    /**
     * Get all notifications for user
     */
    getNotificationsForUser(userId) {
        const user = this._getUserById(userId);
        if (user.role !== 'MANAGER') {
            return [];
        }
        
        const notifications = this._getAllNotifications();
        return notifications
            .filter(n => n.recipientRole === user.role || n.recipientEmail === user.email)
            .sort((a, b) => new Date(b.createdAt) - new Date(a.createdAt));
    }
    
    // Helper methods
    _saveNotification(notification) {
        const notifications = this._getAllNotifications();
        notifications.push(notification);
        localStorage.setItem(this.storageKey, JSON.stringify(notifications));
    }
    
    _getAllNotifications() {
        const data = localStorage.getItem(this.storageKey);
        return data ? JSON.parse(data) : [];
    }
    
    _getUserById(userId) {
        const users = JSON.parse(localStorage.getItem('users') || '[]');
        return users.find(u => u.id === userId);
    }
    
    _generateId() {
        return 'NOTIF-' + Date.now() + '-' + Math.random().toString(36).substr(2, 6).toUpperCase();
    }
    
    _updateNotificationBadge() {
        const currentUser = JSON.parse(sessionStorage.getItem('currentUser'));
        if (currentUser && currentUser.role === 'MANAGER') {
            const count = this.getUnreadCount(currentUser.id);
            const badge = document.getElementById('notification-badge');
            if (badge) {
                badge.textContent = count;
                badge.style.display = count > 0 ? 'inline-block' : 'none';
            }
        }
    }
}
```

---

## 7. Migration Strategy

### 7.1 Role Name Migration Script

```javascript
/**
 * Migrate ADMIN role to MANAGER
 * Run this once during deployment
 */
function migrateAdminToManager() {
    console.log('Starting ADMIN → MANAGER migration...');
    
    // Step 1: Migrate user accounts
    const users = JSON.parse(localStorage.getItem('users') || '[]');
    let adminCount = 0;
    
    users.forEach(user => {
        if (user.role === 'ADMIN') {
            user.role = 'MANAGER';
            adminCount++;
            console.log(`Migrated user: ${user.email} → MANAGER`);
        }
    });
    
    localStorage.setItem('users', JSON.stringify(users));
    console.log(`✓ Migrated ${adminCount} ADMIN accounts to MANAGER`);
    
    // Step 2: Migrate active sessions
    const currentUser = JSON.parse(sessionStorage.getItem('currentUser'));
    if (currentUser && currentUser.role === 'ADMIN') {
        currentUser.role = 'MANAGER';
        sessionStorage.setItem('currentUser', JSON.stringify(currentUser));
        console.log('✓ Migrated active session');
    }
    
    // Step 3: Update demo credentials comment
    console.log('✓ Migration complete!');
    console.log('New credentials:');
    console.log('  CEO: ceo@caladeoro.com / ceo123');
    console.log('  MANAGER: admin@caladeoro.com / admin123');
    console.log('  IT: it@caladeoro.com / it123');
    
    return {
        success: true,
        migratedUsers: adminCount,
        sessionMigrated: currentUser && currentUser.role === 'MANAGER'
    };
}
```

### 7.2 Add Metadata to Existing Records

```javascript
/**
 * Backfill metadata for existing records
 * Assigns historical records to "System" user
 */
function backfillRecordMetadata() {
    console.log('Backfilling metadata for existing records...');
    
    const collections = ['transactions', 'bookings', 'rooms'];
