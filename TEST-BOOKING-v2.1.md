# Testing Guide - Booking System v2.1

## 🧪 Quick Test Scenarios

### Test 1: View New Room Inventory
**Goal:** Verify all 25 rooms display correctly

**Steps:**
1. Login to system
2. Go to "Room Status Map"
3. Check each category:
   - Duplex: Should see 6 rooms (D-1 to D-6)
   - Row House: Should see 6 rooms (RH-1 to RH-6)
   - AC A-Frame: Should see **7 rooms** (AC-1 to AC-7) ✨ NEW
   - Regular A-Frame: Should see **6 rooms** (Reg-1 to Reg-6) ✨ NEW

**Expected Result:** ✅ Total 25 rooms visible

---

### Test 2: Future Booking (Blue Color)
**Goal:** Test blue color for future bookings

**Steps:**
1. Go to "Room Status Map"
2. Click any available room (e.g., Duplex 1)
3. Fill booking form:
   - Check-in: **3 days from today**
   - Check-out: 5 days from today
   - Guest: "Juan Dela Cruz"
4. Click "Reserve" (don't check-in yet)
5. Observe room color

**Expected Result:**
- ✅ Room background is **BLUE** (#3498db)
- ✅ Shows guest first name "Juan"
- ✅ Shows "3d away" or similar
- ✅ No countdown timer (not checked-in yet)

---

### Test 3: Check-in Due TODAY (Red + Pulse)
**Goal:** Test red pulsing color for today's check-ins

**Steps:**
1. Click another available room
2. Fill booking form:
   - Check-in: **TODAY** (use today's date)
   - Check-out: Tomorrow
   - Guest: "Maria Santos"
3. Click "Reserve"
4. Observe room color

**Expected Result:**
- ✅ Room background is **RED** (#e74c3c)
- ✅ Text shows "CHECK-IN" (pulsing)
- ✅ Second line shows "TODAY!"
- ✅ Animation pulses smoothly
- ✅ In table: Badge says "🔴 DUE TODAY"
- ✅ Button says "Check-In Now" (red button)

---

### Test 4: Occupied Room with Countdown
**Goal:** Test countdown timer for occupied rooms

**Steps:**
1. Click a room that's "DUE TODAY" (from Test 3)
2. Click "Check-In Now" button
3. Fill in guest details
4. Confirm check-in
5. Observe room status

**Expected Result:**
- ✅ Room background stays **RED** (occupied)
- ✅ Shows guest first name
- ✅ Shows countdown timer: "⏱️ Xh Ym"
- ✅ In table: Shows "⏱️ Xh Ym remaining" (green text)
- ✅ Countdown is accurate (calculate manually)

---

### Test 5: Countdown Auto-Update
**Goal:** Verify countdown updates every minute

**Steps:**
1. Note current countdown on an occupied room
2. Wait exactly 60 seconds
3. Observe if countdown changes

**Expected Result:**
- ✅ Countdown decreases by 1 minute
- ✅ Both map and table update
- ✅ No page reload needed
- ✅ No console errors

**How to verify:**
- If countdown was "5h 30m" → should become "5h 29m"
- If countdown was "1h 0m" → should become "59m"

---

### Test 6: Overdue Checkout
**Goal:** Test overdue detection

**Note:** This requires manipulating system time or waiting past 12:00 PM

**Steps:**
1. Check-in a guest with checkout = today
2. Wait until after 12:00 PM (checkout time)
3. Observe countdown display

**Expected Result:**
- ✅ Shows "⏱️ OVERDUE"
- ✅ Text color is RED
- ✅ Staff knows guest should have left

**Simulate Test:**
```javascript
// In browser console:
const room = roomsDB.find(r => r.status === 'occupied');
room.checkOut = '2026-06-09'; // Yesterday
renderRoomMap();
renderRoomsTable();
// Should show OVERDUE
```

---

### Test 7: Color Transition at Midnight
**Goal:** Verify blue → red transition when booking date arrives

**Setup:**
1. Create booking with checkIn = tomorrow
2. Verify it shows BLUE
3. Change system date to tomorrow (or wait overnight)
4. Refresh page

**Expected Result:**
- ✅ Before midnight: BLUE color, "1d away"
- ✅ After midnight: RED color, "CHECK-IN TODAY!" (pulsing)

---

### Test 8: All Room Types
**Goal:** Test with different room categories

**Steps:**
1. Book one room from each category:
   - Duplex 1 → Future booking (blue)
   - Row House 1 → Due today (red pulse)
   - AC A-Frame 1 → Occupied (red + countdown)
   - Regular A-Frame 1 → Available (green)
2. Check room map display
3. Check room table display

**Expected Result:**
- ✅ All 4 status types display correctly
- ✅ Colors are distinct and clear
- ✅ Countdown only on occupied room
- ✅ Pulse only on "due today" room

---

### Test 9: Room Table Detailed View
**Goal:** Verify table shows full information

**Steps:**
1. Go to "Rooms (Tables)"
2. Click "AC AFRAME" category
3. Observe table columns

**Expected Result:**
- ✅ 7 AC A-Frame rooms listed
- ✅ Each row shows: Name, Category, Capacity, Price, Status, Actions
- ✅ Occupied rooms show countdown in status column
- ✅ Due today rooms have pulsing badge
- ✅ Future bookings show "(Xd away)"

---

### Test 10: Legend Accuracy
**Goal:** Verify legend matches actual colors

**Steps:**
1. View "Room Status Map"
2. Compare legend colors with actual room colors
3. Read legend descriptions

**Expected Result:**
- ✅ Green = Available ✅
- ✅ Blue = Booked (Future) ✅
- ✅ Red = Check-in Today / Occupied ✅
- ✅ Yellow = Cleaning ✅
- ✅ Legend mentions countdown timer ✅

---

## 🔍 Edge Cases to Test

### Edge Case 1: Same-Day Booking
```
Check-in: TODAY at 2:00 PM
Check-out: TODAY at 12:00 PM (same day)

Expected: RED color, shows as occupied
```

### Edge Case 2: Multi-Day Stay
```
Check-in: TODAY
Check-out: 5 days from now

Expected: Countdown shows "4d Xh Ym" initially
```

### Edge Case 3: Very Short Stay
```
Check-in: NOW
Check-out: In 2 hours

Expected: Countdown shows "2h 0m" or "1h 59m"
```

### Edge Case 4: All Rooms Occupied
```
Book all 25 rooms as occupied

Expected: 
- All show RED background
- All show different countdowns
- Page remains responsive
- No performance issues
```

---

## 📊 Performance Tests

### Test P1: Load Time
**Steps:**
1. Clear browser cache
2. Load page
3. Measure time to display all 25 rooms

**Expected:** < 1 second

### Test P2: Auto-Refresh Performance
**Steps:**
1. Have 25 rooms with various statuses
2. Monitor CPU usage during auto-refresh
3. Check for memory leaks (use Chrome DevTools)

**Expected:**
- CPU spike < 5%
- Memory stable (no leaks)
- No visual lag

### Test P3: Countdown Accuracy
**Steps:**
1. Set occupied room with checkout tomorrow
2. Note exact countdown time
3. Calculate expected countdown manually
4. Compare

**Expected:** ± 1 minute accuracy (due to 60-second refresh)

---

## ✅ Acceptance Criteria

**All must pass:**
- [ ] 25 rooms display correctly (7 AC + 6 Regular)
- [ ] Future bookings show BLUE
- [ ] Today check-ins show RED + pulse
- [ ] Occupied rooms show RED + countdown
- [ ] Countdown updates every 60 seconds
- [ ] Overdue shows warning
- [ ] No console errors
- [ ] Mobile responsive
- [ ] Legend is accurate
- [ ] Table view matches map view

---

## 🐛 Bug Report Template

**If you find issues:**

```
Bug ID: [Auto-generated]
Test: [Which test scenario]
Browser: [Chrome/Firefox/Safari]
Date: [Today's date]

Steps to Reproduce:
1. 
2. 
3. 

Expected Result:
[What should happen]

Actual Result:
[What actually happened]

Screenshot:
[Attach if possible]

Console Errors:
[Copy any error messages]
```

---

## 🎯 Quick Visual Check

**Room Map should show:**
```
GREEN rooms:  Available
BLUE rooms:   Future bookings
RED rooms:    Today check-ins + Occupied (with timer)
YELLOW rooms: Cleaning

RED rooms should either:
- Pulse (if due today)
- Show countdown (if occupied)
```

**Room Table should show:**
```
For Occupied: ⏱️ countdown + "remaining"
For Due Today: 🔴 badge + "Guest should check-in at 2:00 PM"
For Future: 🔵 badge + "(Xd away)"
```

---

**Test Completed:** ___________
**Tester:** ___________
**Pass/Fail:** ___________
**Notes:** ___________
