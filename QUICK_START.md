# 🚀 Jeeva-Bindu Quick Start Guide

Get the app running in 15 minutes!

## ⚡ Prerequisites (5 minutes)

1. **Install Android Studio**
   - Download from [developer.android.com/studio](https://developer.android.com/studio)
   - Install with default settings

2. **Create Firebase Project**
   - Go to [console.firebase.google.com](https://console.firebase.google.com)
   - Click "Add project" → Name it "Jeeva-Bindu"
   - Disable Google Analytics → Create

## 🔥 Firebase Setup (5 minutes)

### Step 1: Add Android App
1. In Firebase Console, click Android icon
2. Package name: `com.healthcare.jeevabindu`
3. Download `google-services.json`
4. Place in `app/` folder (replace existing file)

### Step 2: Enable Services
1. **Firestore Database**
   - Click "Firestore Database" → "Create database"
   - Select "Test mode" → Choose location → Enable

2. **Authentication**
   - Click "Authentication" → "Get started"
   - Enable "Phone" provider → Save

3. **Cloud Messaging**
   - Already enabled by default ✓

## 📱 Run the App (5 minutes)

### Option A: Using Emulator
1. Open Android Studio
2. Open this project folder
3. Wait for Gradle sync (2-3 minutes)
4. Click "Device Manager" → "Create Device"
5. Select "Pixel 5" → Download "API 33" → Finish
6. Click green "Run" button
7. Select your emulator

### Option B: Using Physical Device
1. Enable Developer Options on your phone:
   - Settings → About Phone → Tap "Build Number" 7 times
2. Enable USB Debugging:
   - Settings → Developer Options → USB Debugging
3. Connect phone via USB
4. Open Android Studio → Open project
5. Wait for Gradle sync
6. Click "Run" → Select your device

## ✅ Test the App (2 minutes)

### Quick Test Flow
1. **Register**
   - Enter phone: `1234567890`
   - Click "Verify" (auto-verifies in 1 second)
   - Fill form: Name, Blood Group (O+), Age (25), Location
   - Click "Register"

2. **Post Emergency**
   - Click red "Post Emergency" button
   - Hospital: "Test Hospital"
   - Blood Group: O+
   - Contact: 9876543210
   - Click "Post Alert"

3. **View Alert**
   - Click "Emergency Alerts"
   - See your alert
   - Click "I'm Coming"

4. **Check Profile**
   - Click "My Profile"
   - Click "Mark Donation"
   - See eligibility date (+90 days)

## 🎉 Success!

You now have a working blood donor app!

## 🐛 Common Issues

### "google-services.json not found"
- Ensure file is in `app/` folder, not `app/src/`

### "Gradle sync failed"
- Check internet connection
- File → Invalidate Caches → Restart

### "App crashes on launch"
- Check Logcat tab for errors
- Verify Firebase configuration

## 📚 Next Steps

- Read [README.md](README.md) for full features
- Follow [SETUP_GUIDE.md](SETUP_GUIDE.md) for detailed setup
- Use [TESTING_GUIDE.md](TESTING_GUIDE.md) to test all features
- Review [ARCHITECTURE.md](ARCHITECTURE.md) to understand code

## 🆘 Need Help?

1. Check error in Logcat (bottom of Android Studio)
2. Search error on Stack Overflow
3. Review Firebase Console for configuration
4. Ensure all prerequisites installed

---

**Happy Coding! 🩸**
