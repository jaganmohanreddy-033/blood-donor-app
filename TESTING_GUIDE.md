# Jeeva-Bindu Testing Guide

## 🧪 Complete Testing Checklist

This guide will help you test all features of the Jeeva-Bindu app to ensure it meets the success criteria.

## Prerequisites

- App installed on Android device or emulator
- Firebase project configured
- Internet connection active

## Test Scenarios

### 1. User Authentication & Registration

#### Test 1.1: Phone Number Verification
**Steps:**
1. Launch the app
2. Enter a 10-digit phone number (e.g., `9876543210`)
3. Click "Verify Phone Number"

**Expected Result:**
- ✓ Loading indicator appears
- ✓ After 1 second, registration screen appears (for new users)
- ✓ Or home screen appears (for existing users)

**Pass/Fail:** ___

#### Test 1.2: New Donor Registration
**Steps:**
1. Complete phone verification with a new number
2. Fill registration form:
   - Full Name: "Test Donor 1"
   - Blood Group: "O+"
   - Age: "25"
   - Location: "Test Town"
3. Click "Register"

**Expected Result:**
- ✓ Loading indicator appears
- ✓ Data saved to Firestore
- ✓ Navigates to home screen
- ✓ Welcome message shows donor name

**Pass/Fail:** ___

#### Test 1.3: Existing User Login
**Steps:**
1. Logout from app
2. Enter previously registered phone number
3. Click "Verify Phone Number"

**Expected Result:**
- ✓ Directly navigates to home screen
- ✓ No registration screen shown
- ✓ Profile data loaded correctly

**Pass/Fail:** ___

### 2. Emergency Alert System

#### Test 2.1: Post Emergency Alert
**Steps:**
1. From home screen, click red "Post Emergency" button
2. Fill emergency form:
   - Hospital Name: "City Hospital"
   - Blood Group: "O+"
   - Contact Number: "9876543210"
   - Location: "Test Town"
3. Click "Post Emergency Alert"

**Expected Result:**
- ✓ Loading indicator appears
- ✓ Success message "Alert Posted!" appears
- ✓ Returns to home screen
- ✓ Alert visible in Firestore console

**Pass/Fail:** ___

#### Test 2.2: View Emergency Alerts
**Steps:**
1. From home screen, click "Emergency Alerts"
2. Observe the alerts list

**Expected Result:**
- ✓ All active alerts displayed
- ✓ Alerts show: hospital name, blood group, location, contact
- ✓ Alerts sorted by most recent first
- ✓ Red color scheme used for emergency

**Pass/Fail:** ___

#### Test 2.3: Respond to Emergency Alert (Eligible Donor)
**Steps:**
1. Ensure current donor is eligible (90+ days since last donation)
2. View emergency alerts
3. Click "I'm Coming" button on an alert

**Expected Result:**
- ✓ Button changes to "You're Coming!" with green color
- ✓ Button becomes disabled
- ✓ Responding donors count increases by 1
- ✓ Response saved to Firestore

**Pass/Fail:** ___

#### Test 2.4: Respond to Emergency Alert (Ineligible Donor)
**Steps:**
1. Mark a donation to become ineligible
2. View emergency alerts
3. Observe the alert card

**Expected Result:**
- ✓ "I'm Coming" button not shown
- ✓ Message displayed: "You're not eligible to donate yet"
- ✓ Cannot respond to alert

**Pass/Fail:** ___

#### Test 2.5: Emergency Alert Notification Timing ⏱️
**Critical Success Criterion: Alert must appear within 5 seconds**

**Steps:**
1. Have two devices/emulators ready
2. Device A: Register as donor with blood group "O+"
3. Device B: Post emergency alert for "O+" blood
4. Start timer when "Post Alert" is clicked
5. Observe Device A for notification

**Expected Result:**
- ✓ Notification appears on Device A within 5 seconds
- ✓ Notification shows hospital name and blood group
- ✓ Clicking notification opens app to alerts screen

**Actual Time:** ___ seconds

**Pass/Fail:** ___

### 3. Donor Eligibility & Health Tracking

#### Test 3.1: Initial Eligibility Status
**Steps:**
1. Register a new donor
2. View home screen
3. Observe eligibility card

**Expected Result:**
- ✓ Status shows "✓ Ready to Donate" in green
- ✓ No last donation date shown
- ✓ Total donations: 0

**Pass/Fail:** ___

#### Test 3.2: Mark Donation
**Steps:**
1. Go to "My Profile"
2. Click "Mark Donation" button
3. Confirm in dialog

**Expected Result:**
- ✓ Confirmation dialog appears
- ✓ After confirmation, profile updates
- ✓ Last donation date set to today
- ✓ Total donations increases by 1
- ✓ Status changes to "⏳ Not Eligible" in red/orange

**Pass/Fail:** ___

#### Test 3.3: Eligibility Date Calculation ✅
**Critical Success Criterion: Must correctly calculate 90-day eligibility**

**Steps:**
1. Mark a donation
2. Note today's date
3. Check "Eligible on" date in profile

**Expected Result:**
- ✓ Eligible date = Today + 90 days
- ✓ Date displayed in format: "dd MMM yyyy"
- ✓ Message: "You can donate again on: [date]"
- ✓ Subtext: "90-day waiting period for safe donation"

**Today's Date:** ___
**Expected Eligible Date:** ___
**Actual Eligible Date:** ___

**Pass/Fail:** ___

#### Test 3.4: Multiple Donations Tracking
**Steps:**
1. Mark donation (Total: 1)
2. Manually update Firestore to set lastDonationDate to 91 days ago
3. Refresh app
4. Mark another donation (Total: 2)

**Expected Result:**
- ✓ Total donations increments correctly
- ✓ Last donation date updates to latest
- ✓ Eligibility resets to 90 days from new date

**Pass/Fail:** ___

### 4. Donor Directory

#### Test 4.1: View All Donors
**Steps:**
1. Register 3-4 test donors with different blood groups
2. From home screen, click "Donor Directory"
3. Observe the list

**Expected Result:**
- ✓ All eligible donors displayed
- ✓ Each card shows: name, blood group, location, phone, donations
- ✓ Blood group prominently displayed
- ✓ Count shows: "X eligible donor(s) found"

**Pass/Fail:** ___

#### Test 4.2: Filter by Blood Group
**Steps:**
1. In Donor Directory, click filter icon
2. Select blood group "O+"
3. Observe filtered results

**Expected Result:**
- ✓ Only O+ donors shown
- ✓ Count updates correctly
- ✓ Other blood groups hidden

**Pass/Fail:** ___

#### Test 4.3: Filter by Location
**Steps:**
1. In Donor Directory filters
2. Enter location: "Test Town"
3. Observe filtered results

**Expected Result:**
- ✓ Only donors from "Test Town" shown
- ✓ Partial matches work (case-insensitive)
- ✓ Count updates correctly

**Pass/Fail:** ___

#### Test 4.4: Combined Filters
**Steps:**
1. Select blood group: "O+"
2. Enter location: "Test Town"
3. Observe results

**Expected Result:**
- ✓ Only O+ donors from Test Town shown
- ✓ Both filters applied simultaneously
- ✓ Clear filters button resets both

**Pass/Fail:** ___

#### Test 4.5: No Results Scenario
**Steps:**
1. Filter by rare blood group (e.g., "AB-")
2. Filter by non-existent location

**Expected Result:**
- ✓ Empty state shown
- ✓ Icon and message: "No Eligible Donors Found"
- ✓ Suggestion: "Try adjusting your filters"

**Pass/Fail:** ___

### 5. Profile Management

#### Test 5.1: View Profile
**Steps:**
1. From home screen, click "My Profile"
2. Observe profile details

**Expected Result:**
- ✓ Profile icon and name displayed
- ✓ Phone number shown
- ✓ Blood group prominently displayed with heart icon
- ✓ Age and location shown
- ✓ Total donations count
- ✓ Eligibility status card

**Pass/Fail:** ___

#### Test 5.2: Profile After Donation
**Steps:**
1. Mark a donation
2. View profile immediately

**Expected Result:**
- ✓ Last donation date shows today
- ✓ Eligibility card turns red/orange
- ✓ Shows "Not Eligible Yet"
- ✓ Shows next eligible date (+90 days)
- ✓ Total donations incremented

**Pass/Fail:** ___

### 6. UI/UX Color Standards ✅
**Critical Success Criterion: Red for emergencies, Green for ready**

#### Test 6.1: Color Usage Audit
**Steps:**
1. Navigate through all screens
2. Note color usage

**Expected Color Usage:**
- ✓ Emergency alerts screen: Red header
- ✓ Emergency alert cards: Red background/accents
- ✓ Post emergency button: Red
- ✓ "Ready to Donate" status: Green
- ✓ "I'm Coming" button: Green
- ✓ "Not Eligible" status: Gray/Orange (not red)
- ✓ Blood group badges: Red
- ✓ Regular navigation: Primary blue/purple

**Pass/Fail:** ___

### 7. Navigation & User Flow

#### Test 7.1: Home Screen Navigation
**Steps:**
1. From home screen, test all navigation options
2. Click each card and button

**Expected Result:**
- ✓ Emergency Alerts → Opens alerts screen
- ✓ Donor Directory → Opens directory screen
- ✓ My Profile → Opens profile screen
- ✓ Post Emergency FAB → Opens post emergency screen
- ✓ Back buttons return to home
- ✓ Logout button returns to auth screen

**Pass/Fail:** ___

#### Test 7.2: Deep Navigation
**Steps:**
1. Home → Emergency Alerts → Back
2. Home → Donor Directory → Filter → Back
3. Home → Profile → Mark Donation → Back

**Expected Result:**
- ✓ All back buttons work correctly
- ✓ No navigation stack issues
- ✓ Data persists across navigation

**Pass/Fail:** ___

### 8. Data Persistence

#### Test 8.1: Firestore Integration
**Steps:**
1. Register a donor
2. Open Firebase Console → Firestore
3. Check "donors" collection

**Expected Result:**
- ✓ Donor document created
- ✓ All fields populated correctly
- ✓ Document ID matches donor ID in app

**Pass/Fail:** ___

#### Test 8.2: Real-time Updates
**Steps:**
1. Open app on Device A
2. Post emergency alert on Device B
3. Observe Device A (on alerts screen)

**Expected Result:**
- ✓ New alert appears automatically
- ✓ No manual refresh needed
- ✓ Real-time listener working

**Pass/Fail:** ___

### 9. Edge Cases & Error Handling

#### Test 9.1: No Internet Connection
**Steps:**
1. Disable internet on device
2. Try to register/post alert

**Expected Result:**
- ✓ Error message shown
- ✓ App doesn't crash
- ✓ Graceful degradation

**Pass/Fail:** ___

#### Test 9.2: Invalid Input
**Steps:**
1. Try to register with age < 18
2. Try to post alert with empty fields

**Expected Result:**
- ✓ Submit button disabled for invalid input
- ✓ Validation prevents submission
- ✓ Clear error messages

**Pass/Fail:** ___

#### Test 9.3: Concurrent Responses
**Steps:**
1. Multiple donors respond to same alert
2. Check alert details

**Expected Result:**
- ✓ All responses recorded
- ✓ Count shows correct number
- ✓ No duplicate responses from same donor

**Pass/Fail:** ___

### 10. Performance Testing

#### Test 10.1: App Launch Time
**Steps:**
1. Close app completely
2. Launch app
3. Measure time to home screen

**Expected Result:**
- ✓ Launch time < 3 seconds
- ✓ Smooth animations
- ✓ No lag or stuttering

**Actual Time:** ___ seconds

**Pass/Fail:** ___

#### Test 10.2: Large Data Sets
**Steps:**
1. Create 50+ donor records in Firestore
2. Open Donor Directory
3. Observe performance

**Expected Result:**
- ✓ List loads smoothly
- ✓ Scrolling is smooth
- ✓ No lag or crashes

**Pass/Fail:** ___

## Summary Report

### Critical Success Criteria Results

| Criterion | Target | Result | Pass/Fail |
|-----------|--------|--------|-----------|
| Emergency alert notification time | < 5 seconds | ___ seconds | ___ |
| Eligibility date calculation | Exactly +90 days | ___ | ___ |
| Red color for emergencies only | Yes | ___ | ___ |
| Green color for ready status | Yes | ___ | ___ |

### Overall Test Results

- Total Tests: 35
- Passed: ___
- Failed: ___
- Pass Rate: ___%

### Issues Found

1. ___
2. ___
3. ___

### Recommendations

1. ___
2. ___
3. ___

---

**Tested By:** _______________
**Date:** _______________
**Device/Emulator:** _______________
**Android Version:** _______________
