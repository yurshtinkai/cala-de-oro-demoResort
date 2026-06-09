# Cala de Oro - New Role Workflow Guide

## 🎯 Quick Reference

| Role | Login | Can Input | Can Edit | Can Delete | Can View Reports |
|------|-------|-----------|----------|------------|------------------|
| **IT Staff** | it@caladeoro.com | ✅ Yes | ❌ No | ❌ No | ❌ No |
| **Manager** | manager@caladeoro.com | ✅ Yes | ✅ Yes | ✅ Yes | ✅ Yes |
| **CEO** | ceo@caladeoro.com | ❌ No | ❌ No | ❌ No | ✅ Yes (Read-only) |

---

## 📋 IT Staff Workflow

### What You Can Do:
1. **Process Customer Transactions**
   - Check-in guests to rooms
   - Record entrance fees (day/night tour)
   - Process corkage fees
   - Record tent rentals
   - Record resort outlet sales
   - Add custom items

2. **View Information**
   - Room status map
   - Available rooms
   - Occupied rooms
   - Room details

### What Happens When You Input:
- Your email and role are automatically tracked
- Records are immediately visible to Manager
- You can see your own submissions
- You cannot edit or delete after submission

### Example Transaction Display:
```
🛏️ Duplex 1 (Juan Dela Cruz)
Paid: ₱3,500
Resort: ₱3,500
By: it@caladeoro.com (IT)
```

### Restrictions:
- ❌ No access to Reports & Analytics
- ❌ Cannot edit transactions after submission
- ❌ Cannot delete any records
- ❌ Cannot access financial summaries

---

## 👔 Manager Workflow

### What You Can Do:
1. **All IT Staff Capabilities**
   - Input transactions
   - Process bookings
   - Manage room status

2. **Review & Edit Records**
   - View all transactions (from any staff)
   - Edit transaction amounts
   - Update booking details
   - Correct errors

3. **Delete Records**
   - Remove duplicate entries
   - Delete incorrect transactions
   - Clean up test data

4. **View Financial Reports**
   - Access Reports & Analytics
   - View revenue splits
   - See CEO dashboard data
   - Export reports

### How to Edit a Transaction:
1. Find the transaction in the list
2. Click the "✏️ Edit" button
3. Enter new amount
4. System automatically recalculates totals

### How to Delete a Transaction:
1. Find the transaction in the list
2. Click the "🗑️ Delete" button
3. Confirm deletion
4. Transaction removed, totals updated

### Example Transaction Display (Manager View):
```
🛏️ Duplex 1 (Juan Dela Cruz)
Paid: ₱3,500
Resort: ₱3,500
By: it@caladeoro.com (IT)

[✏️ Edit]  [🗑️ Delete]
```

### Best Practices:
- ✅ Review transactions at end of each shift
- ✅ Verify amounts match receipts
- ✅ Correct errors immediately
- ✅ Delete only confirmed duplicates
- ✅ Keep audit trail in mind

---

## 👨‍💼 CEO Workflow

### What You Can Do:
1. **View Executive Dashboard**
   - Monthly/yearly revenue
   - Occupancy rates
   - Revenue trends
   - Top performing rooms

2. **Access Financial Reports**
   - Transaction logs
   - Revenue split reports (CALA vs Vanessa)
   - Customer totals
   - Export/print reports

3. **Strategic Analysis**
   - Review business metrics
   - Analyze trends
   - Make informed decisions

### What You Cannot Do:
- ❌ Input transactions
- ❌ Edit any records
- ❌ Delete any records
- ❌ Modify room bookings
- ❌ Process payments

### Access Level:
**Read-Only** - Pure analytics and reporting

---

## 🔄 Complete Workflow Example

### Scenario: Guest Check-in and Payment

**Step 1: IT Staff Inputs** (8:00 AM)
```
Action: IT Staff checks in guest to Duplex 1
Input: 
  - Guest Name: Maria Santos
  - Check-in: June 9, 2026
  - Check-out: June 11, 2026
  - Room Rate: ₱3,500/night = ₱7,000
  - Extra pax: 2 (₱500 each) = ₱1,000
  - Total: ₱8,000

Result: Transaction created
  - By: it@caladeoro.com (IT)
  - Time: 08:00 AM
  - Status: Submitted
```

**Step 2: Manager Reviews** (10:00 AM)
```
Action: Manager notices rate should be ₱7,500 (not ₱8,000)
Process:
  1. Click "✏️ Edit" on transaction
  2. Change ₱8,000 to ₱7,500
  3. Click OK
  
Result: Transaction updated
  - Amount: ₱7,500
  - All totals recalculated automatically
  - Original creator still tracked
```

**Step 3: CEO Reviews** (6:00 PM)
```
Action: CEO views daily reports
Access:
  - Reports & Analytics section
  - Sees all transactions
  - Views revenue: ₱7,500 recorded
  - Reviews occupancy rates
  - Cannot modify anything
```

---

## ⚠️ Important Notes

### For IT Staff:
- Double-check amounts before submitting
- Ask Manager if unsure about rates
- Report system issues immediately
- Remember: You cannot undo your entries

### For Manager:
- Review transactions regularly
- Edit carefully - changes affect reports
- Confirm deletions to avoid data loss
- Monitor IT Staff accuracy

### For CEO:
- All data is final after Manager review
- Reports reflect real-time data
- Export reports for external analysis
- Trust the Manager's data verification

---

## 🔐 Security & Audit

### Every Transaction Tracks:
- Creator email
- Creator role
- Timestamp
- Transaction details

### Permissions Are Enforced At:
- Login screen
- Section access
- Button visibility
- Action execution

### If You See "Access Denied":
- You're trying to access restricted feature
- Check your role permissions
- Contact Manager or CEO if needed

---

## 📞 Support

**Technical Issues:** Contact IT Staff  
**Process Questions:** Contact Manager  
**Business Decisions:** Contact CEO

**System Access:** http://localhost/index.html

---

**Last Updated:** June 9, 2026  
**Version:** 2.0 (Role Restructure)
