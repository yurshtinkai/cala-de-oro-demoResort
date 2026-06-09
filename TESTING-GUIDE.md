# 🧪 Testing Guide - CEO Executive Dashboard

## Quick Test Checklist

### ✅ Before Testing
- [ ] Clear browser cache
- [ ] Open `index.html` in browser
- [ ] Ensure JavaScript is enabled

---

## Test Scenario 1: CEO Login & Dashboard

### Steps:
1. Open `index.html`
2. Login with CEO credentials:
   - **Email:** `ceo@caladeoro.com`
   - **Password:** `ceo123`
3. Wait for dashboard to load

### Expected Results:
- ✅ Login screen disappears
- ✅ Redirects to "Reports & Analytics" section automatically
- ✅ Yellow banner shows "👁️ CEO View - Read Only Mode"
- ✅ **NEW:** CEO Executive Dashboard appears at top
- ✅ 4 KPI cards visible with data
- ✅ 4 charts rendered correctly
- ✅ Top performing rooms table shows data
- ✅ Sidebar shows "Logged in as: CEO"

---

## Test Scenario 2: KPI Cards Validation

### Check Each KPI Card:

#### 1. Revenue This Month
- [ ] Shows ₱ amount
- [ ] Shows percentage change (green = positive, red = negative)
- [ ] Value makes sense based on transactions

#### 2. Occupancy Rate
- [ ] Shows percentage (0-100%)
- [ ] Shows trend indicator
- [ ] Matches actual room occupancy

#### 3. Total Bookings
- [ ] Shows number
- [ ] Shows change from previous
- [ ] Reasonable number

#### 4. Average Daily Rate
- [ ] Shows ₱ amount
- [ ] Shows percentage change
- [ ] Makes sense (revenue / bookings)

---

## Test Scenario 3: Charts Validation

### 1. Revenue Trend Chart
- [ ] Line chart displays
- [ ] 12 months of data shown
- [ ] X-axis shows month names
- [ ] Y-axis shows ₱ amounts
- [ ] Current month shows actual data
- [ ] Hover shows tooltip with values
- [ ] Chart is responsive

### 2. Occupancy Trend Chart
- [ ] Line chart with area fill
- [ ] Shows occupancy percentages
- [ ] Current month matches actual occupancy
- [ ] Hover works correctly
- [ ] Y-axis goes from 0-100%

### 3. Revenue by Category (Doughnut)
- [ ] Pie chart displays
- [ ] Shows categories: Rooms, Entrance, Corkage, Tents, Outlets, Other
- [ ] Colors are distinct
- [ ] Legend shows on right
- [ ] Hover shows amount and percentage
- [ ] Categories reflect actual transactions

### 4. Revenue Split (Pie Chart)
- [ ] Shows CALA vs Vanessa split
- [ ] Blue for CALA, Orange for Vanessa
- [ ] Legend shows at bottom
- [ ] Values match the summary cards
- [ ] Percentages add up to 100%

---

## Test Scenario 4: Top Performing Rooms

### Validation:
- [ ] Table displays 5 rooms
- [ ] Columns: Rank, Room, Bookings, Revenue, Occupancy, Avg Rate
- [ ] Data is sorted by rank
- [ ] Revenue shows in green
- [ ] Numbers are formatted correctly (with commas)

---

## Test Scenario 5: Original Reports Still Work

### Scroll Down and Check:
- [ ] Summary cards still visible (CALA, Vanessa, Customer)
- [ ] Live transaction log works
- [ ] "Refresh Report Data" button works
- [ ] "Print PDF" button works
- [ ] Report table generates correctly
- [ ] All existing functionality intact

---

## Test Scenario 6: Other Roles Don't See CEO Dashboard

### Test with ADMIN:
1. Logout (CEO)
2. Login as: `admin@caladeoro.com` / `admin123`
3. Navigate to Reports & Analytics

**Expected:**
- ❌ CEO Executive Dashboard **NOT visible**
- ✅ Original summary cards visible
- ✅ Transaction log and reports work normally

### Test with USER:
1. Logout
2. Login as: `user@caladeoro.com` / `user123`

**Expected:**
- ❌ Reports & Analytics menu item **NOT visible**
- ✅ Can only access operational sections

### Test with IT:
1. Logout
2. Login as: `it@caladeoro.com` / `it123`
3. Navigate to Reports & Analytics

**Expected:**
- ❌ CEO Executive Dashboard **NOT visible**
- ✅ Full reports access
- ✅ Can access User Management

---

## Test Scenario 7: Responsive Design

### Desktop (1920x1080):
- [ ] All charts fit properly
- [ ] KPI cards in 4 columns
- [ ] Charts in 2 columns
- [ ] No horizontal scroll

### Tablet (768px):
- [ ] KPI cards in 2 columns
- [ ] Charts stack vertically
- [ ] All content readable

### Mobile (375px):
- [ ] KPI cards in 1 column
- [ ] Charts full width
- [ ] Touch-friendly
- [ ] Sidebar collapsible (if implemented)

---

## Test Scenario 8: Data Accuracy

### Add Some Transactions:
1. Login as USER or ADMIN
2. Go to "Entrance Fees"
3. Add: 5x Day Tour Adult (₱100 each = ₱500)
4. Add: 3x Night Tour Adult (₱250 each = ₱750)
5. Go to "Rooms"
6. Book a room (e.g., Duplex 1 for ₱3000)
7. **Total Expected:** ₱4,250

### Now Login as CEO:
- [ ] KPI "Revenue This Month" shows ₱4,250
- [ ] Revenue Trend chart updates (current month)
- [ ] Revenue by Category shows correct breakdown
- [ ] Revenue Split shows correct distribution

---

## Test Scenario 9: Browser Compatibility

### Test in Multiple Browsers:

#### Chrome:
- [ ] Dashboard loads correctly
- [ ] Charts render properly
- [ ] No console errors

#### Firefox:
- [ ] All features work
- [ ] Charts display correctly
- [ ] Animations smooth

#### Edge:
- [ ] Full functionality
- [ ] No rendering issues

#### Safari (if available):
- [ ] Charts load
- [ ] Colors correct
- [ ] localStorage works

---

## Test Scenario 10: Performance

### Load Time:
- [ ] Dashboard loads in < 2 seconds
- [ ] Charts render smoothly
- [ ] No lag when scrolling
- [ ] Animations are smooth

### Memory Usage:
- [ ] Check browser DevTools > Memory
- [ ] Should be < 50MB
- [ ] No memory leaks after multiple logins

---

## Test Scenario 11: Error Handling

### Test Edge Cases:

#### No Transaction Data:
1. Clear localStorage
2. Login as CEO
3. Check dashboard

**Expected:**
- [ ] KPIs show ₱0 or 0%
- [ ] Charts show with default data
- [ ] No JavaScript errors
- [ ] "No data" message in top rooms table

#### After Logout & Re-login:
- [ ] Dashboard reinitializes
- [ ] Charts redraw correctly
- [ ] Data persists from localStorage

---

## Test Scenario 12: Print Functionality

### Print Preview (CEO):
1. Login as CEO
2. Go to Reports & Analytics
3. Click "Print PDF" button

**Expected:**
- [ ] Print dialog opens
- [ ] CEO dashboard hidden in print (if .print-hide class applied)
- [ ] Regular reports show in print
- [ ] Layout optimized for print

---

## Common Issues & Solutions

### Issue: Charts Not Showing
**Solution:**
- Check browser console for errors
- Verify Chart.js loaded (check Network tab)
- Clear browser cache
- Try different browser

### Issue: KPIs Show Wrong Data
**Solution:**
- Add some transactions first
- Refresh the page
- Check localStorage has data

### Issue: CEO Dashboard Visible to Other Roles
**Solution:**
- Check `applyRoleBasedAccess()` function
- Verify role detection
- Clear session and re-login

### Issue: Charts Look Distorted
**Solution:**
- Check screen resolution
- Resize browser window
- Check CSS for chart-wrapper height

---

## Automated Test Checklist

### Quick 5-Minute Test:
1. ✅ Login as CEO
2. ✅ Dashboard appears
3. ✅ All 4 KPIs show data
4. ✅ All 4 charts render
5. ✅ Top rooms table displays
6. ✅ Original reports work
7. ✅ Logout and login as ADMIN
8. ✅ CEO dashboard NOT visible
9. ✅ No console errors

---

## Bug Report Template

If you find a bug, report it with:

```
**Bug Title:** [Short description]

**Steps to Reproduce:**
1. 
2. 
3. 

**Expected Behavior:**


**Actual Behavior:**


**Browser:** [Chrome/Firefox/Edge/Safari]
**Version:** [Browser version]
**Role:** [CEO/ADMIN/IT/USER]

**Screenshots:** [If applicable]

**Console Errors:** [Copy from browser console]
```

---

## Success Criteria

### ✅ Test Passes If:
- CEO login shows executive dashboard
- All KPIs display correctly
- All 4 charts render without errors
- Data reflects actual transactions
- Other roles don't see CEO dashboard
- No JavaScript console errors
- Existing features still work
- Performance is acceptable (< 2s load)

### ❌ Test Fails If:
- Charts don't render
- KPIs show NaN or undefined
- CEO dashboard visible to non-CEO users
- Console has JavaScript errors
- Original reports broken
- Performance severely degraded
- Data doesn't update

---

## Next Steps After Testing

### If All Tests Pass:
1. ✅ Mark Phase 2 Task 2.1 as COMPLETED
2. ✅ Update README.md
3. ✅ Move to next priority feature
4. ✅ Consider user training

### If Tests Fail:
1. Document issues found
2. Prioritize by severity
3. Fix critical bugs first
4. Retest after fixes

---

**Testing Completed By:** _______________  
**Date:** _______________  
**Result:** [ ] PASS [ ] FAIL  
**Notes:**

