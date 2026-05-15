# Jeeva-Bindu Setup Guide

## Step-by-Step Setup Instructions

### 1. Install Prerequisites

#### Android Studio
1. Download Android Studio from [developer.android.com](https://developer.android.com/studio)
2. Install with default settings
3. Open Android Studio and complete the setup wizard
4. Install Android SDK (API 24 to 34)

#### Java Development Kit (JDK)
- Android Studio includes JDK, but ensure JDK 8+ is installed
- Verify: Open terminal and run `java -version`

### 2. Firebase Project Setup

#### Create Firebase Project
1. Go to [Firebase Console](https://console.firebase.google.com/)
2. Click "Add project"
3. Enter project name: "Jeeva-Bindu"
4. Disable Google Analytics (optional for this project)
5. Click "Create project"

#### Add Android App to Firebase
1. In Firebase Console, click "Add app" → Android icon
2. Enter package name: `com.healthcare.jeevabindu`
3. Enter app nickname: "Jeeva-Bindu"
4. Leave SHA-1 empty for now (needed for production)
5. Click "Register app"
6. Download `google-services.json`
7. Place it in `app/` directory (replace the placeholder file)

#### Enable Firebase Authentication
1. In Firebase Console, go to "Authentication"
2. Click "Get started"
3. Go to "Sign-in method" tab
4. Enable "Phone" provider
5. Click "Save"

**Note**: For testing, you can add test phone numbers:
- Go to Authentication → Sign-in method → Phone
- Scroll to "Phone numbers for testing"
- Add: `+911234567890` with code `123456`

#### Setup Cloud Firestore
1. In Firebase Console, go to "Firestore Database"
2. Click "Create database"
3. Select "Start in test mode" (for development)
4. Choose location (closest to your region)
5. Click "Enable"

**Create Collections** (Optional - app will create automatically):
- Collection: `donors`
- Collection: `emergency_alerts`

#### Setup Cloud Messaging (FCM)
1. In Firebase Console, go to "Cloud Messaging"
2. FCM is automatically enabled
3. Note: For production, you'll need to setup a backend server to send notifications

### 3. Project Setup in Android Studio

#### Open Project
1. Open Android Studio
2. Click "Open an Existing Project"
3. Navigate to the JeevaBindu folder
4. Click "OK"

#### Gradle Sync
1. Wait for Gradle sync to complete (may take 5-10 minutes first time)
2. If sync fails:
   - Check internet connection
   - Click "File" → "Sync Project with Gradle Files"
   - Check for error messages in "Build" tab

#### Update google-services.json
1. Ensure your downloaded `google-services.json` is in `app/` directory
2. Open the file and verify it has your project details
3. Sync Gradle again

### 4. Configure Android Emulator (Optional)

#### Create Virtual Device
1. In Android Studio, click "Device Manager" (phone icon)
2. Click "Create Device"
3. Select "Pixel 5" or any modern device
4. Click "Next"
5. Download system image: "API 33" (Android 13)
6. Click "Next" → "Finish"

#### Start Emulator
1. Click play button next to your virtual device
2. Wait for emulator to boot (2-3 minutes first time)

### 5. Run the Application

#### First Run
1. Ensure emulator is running OR physical device is connected
2. Click green "Run" button (or press Shift+F10)
3. Select your device
4. Wait for app to build and install (3-5 minutes first time)

#### Verify Installation
- App should launch automatically
- You should see the authentication screen
- App icon should appear in device app drawer

### 6. Testing the App

#### Test User Registration
1. Enter a 10-digit phone number (e.g., `1234567890`)
2. Click "Verify Phone Number"
3. App will simulate verification (no actual OTP)
4. Fill registration form:
   - Full Name: "Test User"
   - Blood Group: Select any
   - Age: 25
   - Location: "Test Town"
5. Click "Register"

#### Test Emergency Alert
1. From home screen, click "Post Emergency" (red button)
2. Fill details:
   - Hospital Name: "Test Hospital"
   - Blood Group: O+
   - Contact: 9876543210
   - Location: "Test Town"
3. Click "Post Emergency Alert"
4. Navigate to "Emergency Alerts" to see your alert

#### Test Donor Directory
1. From home screen, click "Donor Directory"
2. Use filters to search by blood group
3. View registered donors

#### Test Profile
1. From home screen, click "My Profile"
2. View your donation status
3. Click "Mark Donation" to simulate a donation
4. Verify eligibility date updates to +90 days

### 7. Troubleshooting

#### Build Errors

**Error: "google-services.json not found"**
- Solution: Ensure file is in `app/` directory, not `app/src/`

**Error: "Failed to resolve: com.google.firebase"**
- Solution: Check internet connection and sync Gradle again

**Error: "Manifest merger failed"**
- Solution: Clean project (Build → Clean Project) and rebuild

#### Runtime Errors

**App crashes on launch**
- Check Logcat for error messages
- Verify Firebase configuration
- Ensure all dependencies are synced

**Notifications not working**
- Ensure notification permission is granted
- Check FCM setup in Firebase Console
- Note: Actual FCM requires backend server (simulated in this app)

**Data not saving to Firestore**
- Check internet connection
- Verify Firestore rules allow writes
- Check Logcat for Firebase errors

### 8. Firebase Console Monitoring

#### View Registered Users
1. Go to Firestore Database
2. Click "donors" collection
3. View all registered donors

#### View Emergency Alerts
1. Go to Firestore Database
2. Click "emergency_alerts" collection
3. View all posted alerts

#### Monitor Authentication
1. Go to Authentication
2. View "Users" tab
3. See authenticated users (if using real Firebase Auth)

### 9. Testing Notifications (Advanced)

#### Send Test Notification from Firebase Console
1. Go to Cloud Messaging
2. Click "Send your first message"
3. Enter notification title and text
4. Click "Send test message"
5. Enter your FCM token (found in app logs)
6. Click "Test"

#### Get FCM Token
Add this to MainActivity.onCreate():
```kotlin
FirebaseMessaging.getInstance().token.addOnCompleteListener { task ->
    if (task.isSuccessful) {
        val token = task.result
        Log.d("FCM_TOKEN", "Token: $token")
    }
}
```

### 10. Production Deployment Checklist

Before deploying to production:

- [ ] Replace test Firebase project with production project
- [ ] Implement real Firebase Phone Authentication
- [ ] Update Firestore security rules
- [ ] Setup backend server for FCM notifications
- [ ] Add proper error handling
- [ ] Implement data validation
- [ ] Add privacy policy and terms of service
- [ ] Test on multiple devices
- [ ] Setup crash reporting (Firebase Crashlytics)
- [ ] Add analytics (Firebase Analytics)
- [ ] Generate signed APK/AAB
- [ ] Test release build thoroughly

### 11. Common Issues and Solutions

#### Issue: "Execution failed for task ':app:processDebugGoogleServices'"
**Solution**: Ensure google-services.json has correct package name

#### Issue: Gradle sync takes too long
**Solution**: 
- Check internet speed
- Clear Gradle cache: `~/.gradle/caches/`
- Restart Android Studio

#### Issue: Emulator is slow
**Solution**:
- Enable hardware acceleration (HAXM on Windows, KVM on Linux)
- Allocate more RAM to emulator
- Use a physical device instead

#### Issue: App not connecting to Firebase
**Solution**:
- Verify google-services.json is correct
- Check internet connection
- Verify Firebase project is active
- Check Logcat for specific errors

### 12. Development Tips

#### Enable Debug Logging
Add to MainActivity:
```kotlin
FirebaseFirestore.setLoggingEnabled(true)
```

#### View Logcat
- In Android Studio, click "Logcat" tab at bottom
- Filter by package name: `com.healthcare.jeevabindu`
- Look for errors (red) and warnings (orange)

#### Hot Reload
- Jetpack Compose supports hot reload
- Make UI changes and see them instantly
- Click "Apply Changes" (Ctrl+F10) instead of full rebuild

### 13. Next Steps

After successful setup:
1. Explore the codebase
2. Understand MVVM architecture
3. Modify UI colors and themes
4. Add new features
5. Implement real authentication
6. Deploy to Play Store (optional)

### 14. Resources

- [Android Developer Documentation](https://developer.android.com/)
- [Firebase Documentation](https://firebase.google.com/docs)
- [Jetpack Compose Tutorial](https://developer.android.com/jetpack/compose/tutorial)
- [Kotlin Documentation](https://kotlinlang.org/docs/home.html)

---

**Need Help?**
- Check Logcat for errors
- Search error messages on Stack Overflow
- Review Firebase Console for configuration issues
- Ensure all prerequisites are installed correctly

**Happy Coding! 🚀**
