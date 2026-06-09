# Role Restructure Testing Guide

## 🧪 Test Scenarios

### Test 1: Login Changes

#### Test 1.1: Old Admin Login (Should Fail)
```
Email: admin@caladeoro.com
Password: admin123
Expected: ❌ "Invalid email or password"
Status: [ ] PASS [ ] FAIL
```

#### Test 1.2: New Manager Login (Should Succeed)
```
Email: manager@caladeoro.com
Password: manager123
Expected: ✅ Logged in as MANAGER
Status: [ ] PASS [ ] FAIL
```

#### Test 1.3: IT Staff Login (Unchanged)
```
Email: it@caladeoro.com
Password: it123
Expected: ✅ Logged in as IT
Status: [ ] PASS [ ] FAIL
```

#### Test 1.4: CEO Login (Unchanged)
```
Email: ceo@caladeoro.com
Password: ceo123
Expected: ✅ Logged in as CEO
Status: [ ] PASS [ ] FAIL
```

---

### Test 2: IT Staff Permissions

**Login as IT Staff:** `it@caladeoro.com / it123`

#### Test 2.1: Can Input Transaction
```
Steps:
1. Navigate to "Room Status Map" or any transaction section
2. Add a room booking or entrance fee
3. Check if transaction appears in cart

Expected: ✅ Transaction added successfully
Verify: Transaction shows "By: it@caladeoro.com (IT)"
Status: [ ] PASS [ ] FAIL
```

#### Test 2.2: Cannot See Edit/Delete Buttons
```
Steps:
1. Look at any transaction in the cart
2. Check for Edit and Delete buttons

Expected: ❌ No Edit/Delete buttons visible
Status: [ ] PASS [ ] FAIL
```

#### Test 2.3: Cannot Access Reports
```
Steps:
1. Look at sidebar menu
2. Try to access Reports & Analytics

Expected: ❌ Reports button not visible or access denied
Status: [ ] PASS [ ] FAIL
```

---

### Test 3: Manager Permissions

**Login as Manager:** `manager@caladeoro.com / manager123`

#### Test 3.1: Can Input Transaction
```
Steps:
1. Navigate to any transaction section
2. Add a transaction
3. Verify it appears

Expected: ✅ Transaction added successfully
Verify: Shows "By: manager@caladeoro.com (MANAGER)"
Status: [ ] PASS [ ] FAIL
```

#### Test 3.2: Can See Edit/Delete Buttons
```
Steps:
1. Look at any transaction in cart
2. Check for Edit and Delete buttons

Expected: ✅ Both buttons visible on each transaction
Status: [ ] PASS [ ] FAIL
```

#### Test 3.3: Can Edit Transaction
```
Steps:
1. Find a transaction with amount ₱1,000
2. Click "✏️ Edit" button
3. Change amount to ₱1,500
4. Click OK
5. Check if total updated

Expected: ✅ Transaction amount updated to ₱1,500
Verify: Customer Total increased by ₱500
Status: [ ] PASS [ ] FAIL
```

#### Test 3.4: Edit with Vanessa Split
```
Steps:
1. Create a Duplex room booking (70/30 split)
2. Note the split amounts
3. Edit the transaction
4. Change the amount
5. Verify split ratio maintained

Expected: ✅ Split ratio stays 70/30
Example: ₱3,500 → Vanessa: ₱2,450, Resort: ₱1,050
         ₱4,000 → Vanessa: ₱2,800, Resort: ₱1,200
Status: [ ] PASS [ ] FAIL
```

#### Test 3.5: Can Delete Transaction
```
Steps:
1. Find any transaction
2. Click "🗑️ Delete" button
3. Confirm deletion
4. Check if transaction removed

Expected: ✅ Transaction deleted
Verify: Total decreased by transaction amount
Status: [ ] PASS [ ] FAIL
```

#### Test 3.6: Delete Confirmation Dialog
```
Steps:
1. Click Delete on any transaction
2. Click "Cancel" in confirmation

Expected: ✅ Transaction NOT deleted (stays in list)
Status: [ ] PASS [ ] FAIL
```

#### Test 3.7: Can Access Reports
```
Steps:
1. Click "Reports & Analytics" in sidebar
2. Verify report displays

Expected: ✅ Full access to reports
Status: [ ] PASS [ ] FAIL
```

---

### Test 4: CEO Permissions

**Login as CEO:** `ceo@caladeoro.com / ceo123`

#### Test 4.1: Cannot Input Transactions
```
Steps:
1. Navigate to room sections
2. Check if transaction sections visible

Expected: ❌ No transaction input sections
Verify: Read-only view message appears
Status: [ ] PASS [ ] FAIL
```

#### Test 4.2: Cannot Edit/Delete
```
Steps:
1. Navigate to Reports & Analytics
2. Check transaction log

Expected: ❌ No Edit/Delete buttons anywhere
Status: [ ] PASS [ ] FAIL
```

#### Test 4.3: Can Access Reports
```
Steps:
1. Should auto-open Reports & Analytics
2. Verify CEO dashboard visible

Expected: ✅ Full reports access, read-only
Status: [ ] PASS [ ] FAIL
```

---

### Test 5: Transaction Tracking

**Login as IT Staff, then switch to Manager**

#### Test 5.1: Creator Info Display
```
Steps (as IT Staff):
1. Login as it@caladeoro.com
2. Add entrance fee: Day Tour (x5)
3. Note the transaction

Expected: Shows "By: it@caladeoro.com (IT)"
Status: [ ] PASS [ ] FAIL
```

#### Test 5.2: Manager Can Edit IT Staff Records
```
Steps (as Manager):
1. Logout IT Staff
2. Login as manager@caladeoro.com
3. Find the Day Tour transaction from IT Staff
4. Click Edit button
5. Change quantity or amount

Expected: ✅ Manager can edit IT Staff's transaction
Verify: Creator still shows "it@caladeoro.com (IT)"
Status: [ ] PASS [ ] FAIL
```

#### Test 5.3: Manager Can Delete IT Staff Records
```
Steps (as Manager):
1. Find any transaction created by IT Staff
2. Click Delete button
3. Confirm deletion

Expected: ✅ Transaction deleted successfully
Status: [ ] PASS [ ] FAIL
```

---

### Test 6: Total Recalculation

#### Test 6.1: Edit Updates Totals
```
Steps:
1. Note current Customer Total
2. Edit a ₱1,000 transaction to ₱1,200
3. Check new Customer Total

Expected: ✅ Total increased by ₱200
Status: [ ] PASS [ ] FAIL
```

#### Test 6.2: Delete Updates Totals
```
Steps:
1. Note current totals:
   - Customer Total
   - Resort Total
   - Vanessa Total
2. Delete a transaction worth ₱1,500
3. Verify all totals decreased by ₱1,500

Expected: ✅ All totals updated correctly
Status: [ ] PASS [ ] FAIL
```

#### Test 6.3: Multiple Edits
```
Steps:
1. Edit transaction A: ₱500 → ₱700
2. Edit transaction B: ₱1,000 → ₱800
3. Verify net change: +₱200 - ₱200 = ₱0

Expected: ✅ Totals reflect cumulative changes
Status: [ ] PASS [ ] FAIL
```

---

### Test 7: Edge Cases

#### Test 7.1: Edit to Zero
```
Steps:
1. Try to edit transaction to ₱0
2. Enter "0" in edit prompt

Expected: ⚠️ Should accept or show validation
Status: [ ] PASS [ ] FAIL
```

#### Test 7.2: Edit to Negative
```
Steps:
1. Try to edit transaction to -₱100
2. Enter "-100" in edit prompt

Expected: ❌ Should show error or prevent
Status: [ ] PASS [ ] FAIL
```

#### Test 7.3: Edit Cancel
```
Steps:
1. Click Edit on transaction
2. Click Cancel without changing

Expected: ✅ No changes made
Status: [ ] PASS [ ] FAIL
```

#### Test 7.4: Delete All Transactions
```
Steps:
1. Delete all transactions one by one
2. Check if empty message appears

Expected: ✅ Empty cart message shows
Verify: All totals = ₱0
Status: [ ] PASS [ ] FAIL
```

---

### Test 8: Cross-Role Scenarios

#### Test 8.1: IT Staff Cannot Bypass Permissions
```
Steps (as IT Staff):
1. Open browser console
2. Try: editTransaction('TXN-123456789')
3. Check response

Expected: ❌ "Access Denied" alert
Status: [ ] PASS [ ] FAIL
```

#### Test 8.2: Role Display
```
Steps:
1. Login as each role
2. Check role indicator in sidebar

Expected:
- IT: "Logged in as: IT"
- MANAGER: "Logged in as: MANAGER"
- CEO: "Logged in as: CEO"
Status: [ ] PASS [ ] FAIL
```

---

### Test 9: Reports Integration

#### Test 9.1: Edited Transactions in Reports
```
Steps (as Manager):
1. Add transaction: ₱1,000
2. Edit to: ₱1,500
3. Navigate to Reports & Analytics
4. Check if report shows ₱1,500

Expected: ✅ Report shows edited amount
Status: [ ] PASS [ ] FAIL
```

#### Test 9.2: Deleted Transactions Not in Reports
```
Steps:
1. Note transaction count in report
2. Delete a transaction
3. Refresh report
4. Verify count decreased

Expected: ✅ Deleted transaction not in report
Status: [ ] PASS [ ] FAIL
```

---

## 📊 Test Results Summary

**Total Tests:** 29  
**Passed:** [ ]  
**Failed:** [ ]  
**Success Rate:** [ ]%

### Critical Failures:
- [ ] None
- [ ] List any critical issues here

### Minor Issues:
- [ ] None
- [ ] List any minor issues here

### Recommendations:
- [ ] Production ready
- [ ] Needs fixes
- [ ] Requires retesting

---

## 🔍 Manual Inspection Checklist

### UI Elements:
- [ ] Edit button appears for Manager
- [ ] Delete button appears for Manager
- [ ] No edit/delete for IT Staff
- [ ] Creator info displays correctly
- [ ] Totals update in real-time
- [ ] Confirmation dialogs appear

### Functionality:
- [ ] Edit updates amounts correctly
- [ ] Delete removes transactions
- [ ] Totals recalculate accurately
- [ ] Revenue splits maintained
- [ ] Role permissions enforced
- [ ] Session validation works

### User Experience:
- [ ] Buttons are intuitive
- [ ] Confirmation messages clear
- [ ] Error messages helpful
- [ ] Response times acceptable
- [ ] No console errors
- [ ] Mobile responsive (if applicable)

---

## 🐛 Bug Report Template

**Bug ID:** #  
**Severity:** Critical / Major / Minor  
**Found By:**  
**Date:**  

**Steps to Reproduce:**
1. 
2. 
3. 

**Expected Result:**


**Actual Result:**


**Screenshots/Logs:**


**Browser/Environment:**


---

**Test Date:** ___________  
**Tester Name:** ___________  
**Environment:** Production / Staging / Local  
**Browser:** Chrome / Firefox / Safari / Edge  

**Sign-off:** ___________
