# Quick Reference Card - Booking System v2.1

## 🎨 Color Guide (Print & Post Near Computer)

```
┌─────────────────────────────────────────────┐
│         ROOM STATUS COLORS                  │
├─────────────────────────────────────────────┤
│                                             │
│  🟢 GREEN    = Available (ready to book)   │
│                                             │
│  🔵 BLUE     = Future booking (not today)  │
│              → No urgency, plan ahead      │
│                                             │
│  🔴 RED      = URGENT ATTENTION NEEDED!    │
│   (Pulsing)  → Guest checking in TODAY     │
│   (Solid)    → Currently occupied          │
│                                             │
│  🟡 YELLOW   = Cleaning in progress        │
│                                             │
└─────────────────────────────────────────────┘
```

---

## ⏱️ Countdown Timer Meaning

```
⏱️ 2d 8h 30m   = 2 days, 8 hours, 30 minutes until checkout
⏱️ 5h 15m      = 5 hours, 15 minutes until checkout
⏱️ 45m         = 45 minutes until checkout
⚠️ OVERDUE     = Guest should have checked out!
```

**Updates:** Every 60 seconds automatically

---

## 🏠 Room Inventory (Total: 25)

```
Duplex:           6 units (D-1 to D-6)
Row House:        6 units (RH-1 to RH-6)
AC A-Frame:       7 units (AC-1 to AC-7)    ← UPDATED
Regular A-Frame:  6 units (Reg-1 to Reg-6)  ← UPDATED
```

---

## 🕐 Check-in/Check-out Times

```
Check-in:  2:00 PM (14:00)
Check-out: 12:00 PM (12:00)
```

---

## 📋 Daily Workflow

### Morning (8:00 AM):
1. ✅ Check for RED PULSING rooms → Guests arriving TODAY
2. ✅ Verify rooms are clean and ready
3. ✅ Check countdown timers on occupied rooms
4. ✅ Follow up on any OVERDUE checkouts

### Afternoon (2:00 PM):
5. ✅ Check-in guests from RED PULSING rooms
6. ✅ Countdown timer starts automatically

### Before Noon (11:30 AM):
7. ✅ Review rooms with countdown < 1 hour
8. ✅ Prepare for checkouts

---

## 🚀 Quick Actions

### Book Future Guest:
```
1. Click GREEN room
2. Enter check-in date (future date)
3. Enter guest name
4. Save → Room turns BLUE
```

### Guest Arriving Today:
```
1. Click GREEN room  
2. Enter check-in date = TODAY
3. Enter guest name
4. Save → Room turns RED + PULSES
5. When guest arrives: Click "Check-In Now"
6. → Countdown starts
```

### Check Out Guest:
```
1. Click RED room with countdown
2. Click "Check Out"
3. → Room turns GREEN
```

---

## ⚠️ Attention Flags

```
🔴 + PULSING    = Check-in due NOW!
🔴 + COUNTDOWN  = Guest will leave soon
⚠️ OVERDUE      = Contact guest immediately
🔵 + "3d away"  = Guest arrives in 3 days
```

---

## 🐛 Troubleshooting

**Problem:** Color not changing  
**Fix:** Refresh page (F5)

**Problem:** Countdown not updating  
**Fix:** Wait 60 seconds (auto-updates)

**Problem:** New rooms not showing  
**Fix:** Hard refresh (Ctrl + F5)

---

## 📞 Quick Contacts

**IT Support:** it@caladeoro.com  
**Manager:** manager@caladeoro.com  
**System Issues:** Report immediately

---

**Version:** 2.1.0  
**Updated:** June 10, 2026  
**Print Date:** _________
