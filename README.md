# Jeeva-Bindu - Rapid Response Blood Donor Directory

## 🩸 About the App

Jeeva-Bindu is a community-owned blood donor directory designed to solve the critical problem of finding blood donors for rare blood groups in taluka-level hospitals. The app focuses on "Location-First" matching to ensure donors can reach hospitals within the "Golden Hour."

## 🎯 Problem Statement

In rural hospitals, finding a blood donor for rare groups like O-negative is a crisis. People post on WhatsApp, but those messages often reach the wrong people or arrive too late. There is no "Live Directory" of volunteers ready to donate in specific towns.

## ✨ Features

### 1. Donor Registry
- Register with blood group, age, and location (Panchayat/Town)
- Phone number verification (simulated)
- Profile management

### 2. Emergency Alerts
- Post urgent blood requirements
- Real-time notifications via Firebase Cloud Messaging (FCM)
- Location-based filtering
- Blood group matching

### 3. "I'm Coming" Response System
- Donors can signal they're on their way
- Prevents duplicate efforts
- Shows responding donors count

### 4. Donor Health Tracker
- Tracks last donation date
- Calculates eligibility based on 90-day rule
- Shows next eligible donation date
- Maintains donation history

### 5. Donor Directory
- Search available donors by blood group
- Filter by location
- View donor contact information
- Shows only eligible donors

## 🏗️ Technical Architecture

### Technology Stack
- **Language**: Kotlin
- **UI Framework**: Jetpack Compose (Material 3)
- **Architecture**: MVVM (Model-View-ViewModel)
- **Backend**: Firebase
  - Firebase Authentication (Phone verification)
  - Cloud Firestore (Database)
  - Firebase Cloud Messaging (Push notifications)
- **Navigation**: Jetpack Navigation Compose
- **Async**: Kotlin Coroutines & Flow

### Project Structure
```
app/
├── src/main/java/com/healthcare/jeevabindu/
│   ├── model/
│   │   ├── Donor.kt
│   │   └── EmergencyAlert.kt
│   ├── repository/
│   │   ├── DonorRepository.kt
│   │   └── EmergencyRepository.kt
│   ├── service/
│   │   ├── FCMService.kt
│   │   └── NotificationService.kt
│   ├── viewmodel/
│   │   ├── AuthViewModel.kt
│   │   ├── DonorViewModel.kt
│   │   └── EmergencyViewModel.kt
│   ├── ui/
│   │   ├── screens/
│   │   │   ├── AuthScreen.kt
│   │   │   ├── RegistrationScreen.kt
│   │   │   ├── HomeScreen.kt
│   │   │   ├── EmergencyAlertsScreen.kt
│   │   │   ├── DonorDirectoryScreen.kt
│   │   │   ├── PostEmergencyScreen.kt
│   │   │   └── ProfileScreen.kt
│   │   └── theme/
│   │       ├── Color.kt
│   │       ├── Theme.kt
│   │       └── Type.kt
│   └── MainActivity.kt
```

## 🚀 Setup Instructions

### Prerequisites
1. Android Studio (Latest version)
2. JDK 8 or higher
3. Android SDK (API 24+)
4. Firebase account

### Firebase Setup

1. **Create Firebase Project**
   - Go to [Firebase Console](https://console.firebase.google.com/)
   - Create a new project named "Jeeva-Bindu"

2. **Add Android App**
   - Click "Add app" → Android
   - Package name: `com.healthcare.jeevabindu`
   - Download `google-services.json`
   - Replace the placeholder file in `app/google-services.json`

3. **Enable Firebase Services**
   - **Authentication**: Enable Phone authentication
   - **Firestore Database**: Create database in test mode
   - **Cloud Messaging**: Enable FCM

4. **Firestore Security Rules**
```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /donors/{donorId} {
      allow read: if true;
      allow write: if request.auth != null;
    }
    match /emergency_alerts/{alertId} {
      allow read: if true;
      allow write: if request.auth != null;
    }
  }
}
```

### Build and Run

1. **Clone the repository**
```bash
git clone <repository-url>
cd JeevaBindu
```

2. **Open in Android Studio**
   - Open Android Studio
   - File → Open → Select project folder

3. **Sync Gradle**
   - Wait for Gradle sync to complete
   - Resolve any dependency issues

4. **Run the app**
   - Connect Android device or start emulator
   - Click Run button or press Shift+F10

## 🎨 UI/UX Design Principles

### Color Scheme
- **Red**: Used strictly for emergencies and urgent alerts
- **Green**: Indicates "Ready to Donate" status
- **Gray**: Shows "Not Eligible" status

### Key Screens
1. **Authentication**: Simple phone number verification
2. **Registration**: Donor profile creation
3. **Home**: Dashboard with quick actions
4. **Emergency Alerts**: Real-time blood requests
5. **Donor Directory**: Searchable donor list
6. **Profile**: Personal donation history and eligibility

## ✅ Success Criteria

### 1. Emergency Alert Performance
- ✓ Alert appears on donor's phone within 5 seconds
- ✓ FCM integration for instant notifications
- ✓ Location-based filtering

### 2. Eligibility Calculation
- ✓ Correctly calculates 90-day waiting period
- ✓ Shows next eligible donation date
- ✓ Prevents ineligible donors from responding

### 3. UI Color Standards
- ✓ Red for emergencies only
- ✓ Green for "Ready to Donate"
- ✓ Clear visual hierarchy

## 📊 Impact Goals

### Emergency Resilience
Building a localized, reliable emergency response system that connects donors with hospitals in minutes, not hours.

### Community Spirit
Transforming blood donation into a localized social duty, creating a network of life-savers in every town.

### Zero Delay
Reducing mortality rates due to blood unavailability in rural areas by ensuring rapid donor response.

## 🧪 Testing

### Manual Testing Checklist
- [ ] Phone number verification works
- [ ] Donor registration saves to Firestore
- [ ] Emergency alerts appear in real-time
- [ ] "I'm Coming" button updates alert
- [ ] Eligibility calculation is accurate
- [ ] Donor directory filters work correctly
- [ ] Profile shows correct donation history
- [ ] Notifications arrive within 5 seconds

### Test Scenarios
1. **New User Registration**
   - Enter phone number → Verify → Complete profile
   
2. **Post Emergency**
   - Navigate to Post Emergency → Fill details → Submit
   - Verify notification received by eligible donors

3. **Respond to Alert**
   - View emergency alert → Click "I'm Coming"
   - Verify response recorded

4. **Mark Donation**
   - Go to Profile → Mark Donation
   - Verify eligibility date updated to +90 days

## 🔐 Security Considerations

1. **Phone Verification**: Simulated for educational purposes (use Firebase Auth in production)
2. **Data Privacy**: Donor information visible to all users (consider privacy settings in production)
3. **Firestore Rules**: Currently in test mode (implement proper security rules for production)

## 🚧 Future Enhancements

1. **Real Firebase Authentication**: Implement actual OTP verification
2. **Push Notification Backend**: Server-side FCM message sending
3. **Blood Bank Integration**: Connect with local blood banks
4. **Donation Certificates**: Generate certificates for donors
5. **Gamification**: Badges and rewards for frequent donors
6. **Multi-language Support**: Regional language support
7. **Offline Mode**: Cache data for offline access
8. **Analytics**: Track response times and success rates

## 📱 Minimum Requirements

- Android 7.0 (API 24) or higher
- Internet connection
- Notification permissions
- Location permissions (optional, for better matching)

## 📄 License

This is an educational project for learning Android development with GenAI.

## 🤝 Contributing

This is a student project. Contributions and suggestions are welcome!

## 📞 Support

For issues or questions, please create an issue in the repository.

---

**Built with ❤️ for saving lives in rural India**
