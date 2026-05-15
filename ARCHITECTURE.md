# Jeeva-Bindu Architecture Documentation

## 📐 Architecture Overview

Jeeva-Bindu follows the **MVVM (Model-View-ViewModel)** architecture pattern with **Clean Architecture** principles, ensuring separation of concerns, testability, and maintainability.

## 🏛️ Architecture Layers

```
┌─────────────────────────────────────────┐
│           Presentation Layer            │
│  (UI - Jetpack Compose + ViewModels)    │
└─────────────────────────────────────────┘
                    ↓
┌─────────────────────────────────────────┐
│           Domain Layer                  │
│        (Business Logic)                 │
└─────────────────────────────────────────┘
                    ↓
┌─────────────────────────────────────────┐
│            Data Layer                   │
│  (Repositories + Data Sources)          │
└─────────────────────────────────────────┘
                    ↓
┌─────────────────────────────────────────┐
│         External Services               │
│  (Firebase - Firestore, Auth, FCM)      │
└─────────────────────────────────────────┘
```

## 📦 Layer Details

### 1. Presentation Layer

#### UI Components (Jetpack Compose)
- **AuthScreen**: Phone number verification
- **RegistrationScreen**: New donor registration
- **HomeScreen**: Main dashboard
- **EmergencyAlertsScreen**: View and respond to alerts
- **DonorDirectoryScreen**: Search and filter donors
- **PostEmergencyScreen**: Create emergency alerts
- **ProfileScreen**: View and manage donor profile

#### ViewModels
- **AuthViewModel**: Manages authentication state and user session
- **DonorViewModel**: Handles donor data and filtering
- **EmergencyViewModel**: Manages emergency alerts and responses

**Responsibilities:**
- Hold UI state
- Handle user interactions
- Communicate with repositories
- Expose data via StateFlow/Flow
- Survive configuration changes

### 2. Domain Layer

#### Models
```kotlin
data class Donor(
    val id: String,
    val phoneNumber: String,
    val fullName: String,
    val bloodGroup: BloodGroup,
    val age: Int,
    val location: String,
    val lastDonationDate: Date?,
    val totalDonations: Int,
    val fcmToken: String,
    val isAvailable: Boolean
)

data class EmergencyAlert(
    val id: String,
    val hospitalName: String,
    val requiredBloodGroup: BloodGroup,
    val contactNumber: String,
    val location: String,
    val postedBy: String,
    val createdAt: Date,
    val isActive: Boolean,
    val respondingDonors: List<RespondingDonor>
)
```

**Business Logic:**
- Eligibility calculation (90-day rule)
- Blood group matching
- Location-based filtering
- Donation tracking

### 3. Data Layer

#### Repositories
- **DonorRepository**: CRUD operations for donors
- **EmergencyRepository**: CRUD operations for emergency alerts

**Responsibilities:**
- Abstract data sources
- Provide clean API to ViewModels
- Handle data transformations
- Manage caching (if needed)

#### Data Sources
- **Firebase Firestore**: Primary database
- **Firebase Authentication**: User authentication
- **Firebase Cloud Messaging**: Push notifications

### 4. Service Layer

#### FCMService
- Receives push notifications
- Creates local notifications
- Handles notification clicks

#### NotificationService
- Sends emergency alerts to eligible donors
- Manages FCM tokens
- Handles topic subscriptions

## 🔄 Data Flow

### Example: Posting an Emergency Alert

```
User clicks "Post Alert"
        ↓
PostEmergencyScreen captures input
        ↓
EmergencyViewModel.postEmergencyAlert()
        ↓
EmergencyRepository.createEmergencyAlert()
        ↓
Firestore saves alert document
        ↓
NotificationService.sendEmergencyAlert()
        ↓
FCM sends notifications to eligible donors
        ↓
Donors receive notification via FCMService
        ↓
EmergencyRepository.getActiveAlertsFlow() emits update
        ↓
EmergencyViewModel updates UI state
        ↓
EmergencyAlertsScreen displays new alert
```

## 🔐 State Management

### StateFlow Pattern
```kotlin
class EmergencyViewModel : ViewModel() {
    private val _alerts = MutableStateFlow<List<EmergencyAlert>>(emptyList())
    val alerts: StateFlow<List<EmergencyAlert>> = _alerts.asStateFlow()
    
    init {
        observeAlerts()
    }
    
    private fun observeAlerts() {
        viewModelScope.launch {
            emergencyRepository.getActiveAlertsFlow().collect { alertList ->
                _alerts.value = alertList
            }
        }
    }
}
```

### UI State Observation
```kotlin
@Composable
fun EmergencyAlertsScreen(emergencyViewModel: EmergencyViewModel) {
    val alerts by emergencyViewModel.alerts.collectAsState()
    
    LazyColumn {
        items(alerts) { alert ->
            EmergencyAlertCard(alert)
        }
    }
}
```

## 🔥 Firebase Integration

### Firestore Structure
```
firestore/
├── donors/
│   └── {donorId}/
│       ├── phoneNumber: String
│       ├── fullName: String
│       ├── bloodGroup: String
│       ├── age: Number
│       ├── location: String
│       ├── lastDonationDate: Timestamp
│       ├── totalDonations: Number
│       ├── fcmToken: String
│       ├── isAvailable: Boolean
│       └── createdAt: Timestamp
│
└── emergency_alerts/
    └── {alertId}/
        ├── hospitalName: String
        ├── requiredBloodGroup: String
        ├── contactNumber: String
        ├── location: String
        ├── postedBy: String
        ├── postedByName: String
        ├── createdAt: Timestamp
        ├── isActive: Boolean
        └── respondingDonors: Array
            └── {
                  donorId: String,
                  donorName: String,
                  donorPhone: String,
                  respondedAt: Timestamp
                }
```

### Real-time Listeners
```kotlin
fun getActiveAlertsFlow(): Flow<List<EmergencyAlert>> = callbackFlow {
    val listener = alertsCollection
        .whereEqualTo("isActive", true)
        .orderBy("createdAt", Query.Direction.DESCENDING)
        .addSnapshotListener { snapshot, error ->
            if (error != null) {
                close(error)
                return@addSnapshotListener
            }
            
            val alerts = snapshot?.documents?.mapNotNull { 
                it.toObject(EmergencyAlert::class.java) 
            } ?: emptyList()
            
            trySend(alerts)
        }
    
    awaitClose { listener.remove() }
}
```

## 🎯 Design Patterns Used

### 1. Repository Pattern
Abstracts data sources and provides a clean API to the domain layer.

### 2. Observer Pattern
ViewModels observe data changes and update UI automatically.

### 3. Singleton Pattern
Repositories and services are single instances.

### 4. Factory Pattern
ViewModels created via ViewModelProvider.

### 5. Dependency Injection (Manual)
Dependencies passed through constructors (can be upgraded to Hilt/Koin).

## 🧪 Testability

### Unit Testing ViewModels
```kotlin
@Test
fun `posting emergency alert updates state correctly`() = runTest {
    // Given
    val viewModel = EmergencyViewModel()
    
    // When
    viewModel.postEmergencyAlert(...)
    
    // Then
    assertEquals(PostAlertState.Success, viewModel.postAlertState.value)
}
```

### Testing Repositories
```kotlin
@Test
fun `getDonorByPhone returns correct donor`() = runTest {
    // Given
    val repository = DonorRepository()
    
    // When
    val donor = repository.getDonorByPhone("1234567890")
    
    // Then
    assertNotNull(donor)
    assertEquals("1234567890", donor?.phoneNumber)
}
```

## 🚀 Performance Optimizations

### 1. Lazy Loading
- Donors loaded on-demand
- Pagination can be added for large datasets

### 2. Efficient Queries
- Firestore queries use indexes
- Only active alerts fetched
- Location-based filtering at database level

### 3. Caching
- StateFlow caches latest state
- Reduces unnecessary recompositions

### 4. Coroutines
- Asynchronous operations don't block UI
- Structured concurrency with viewModelScope

## 🔒 Security Considerations

### 1. Authentication
- Phone number verification (simulated)
- Production: Use Firebase Authentication

### 2. Authorization
- Firestore security rules control access
- Only authenticated users can write

### 3. Data Validation
- Input validation in UI
- Server-side validation in Firestore rules

### 4. Privacy
- Donor information visible to all (consider privacy settings)
- Phone numbers displayed (consider masking)

## 📱 Offline Support (Future)

### Planned Implementation
```kotlin
// Cache donors locally
val cachedDonors = donorRepository.getCachedDonors()

// Sync when online
if (isOnline) {
    donorRepository.syncWithServer()
}
```

## 🔄 Migration Path

### Current: Simulated Auth
```kotlin
fun verifyPhoneNumber(phoneNumber: String) {
    // Simulated verification
    kotlinx.coroutines.delay(1000)
    val donor = donorRepository.getDonorByPhone(phoneNumber)
}
```

### Future: Real Firebase Auth
```kotlin
fun verifyPhoneNumber(phoneNumber: String) {
    FirebaseAuth.getInstance()
        .signInWithPhoneNumber(phoneNumber, callbacks)
}
```

## 📊 Scalability Considerations

### Current Limitations
- All donors loaded at once
- No pagination
- Simple filtering

### Scaling Solutions
1. **Pagination**: Load donors in batches
2. **Geohashing**: Efficient location queries
3. **Cloud Functions**: Server-side notification sending
4. **Caching**: Redis for frequently accessed data
5. **CDN**: Static assets delivery

## 🛠️ Technology Choices

### Why Jetpack Compose?
- Modern declarative UI
- Less boilerplate
- Better performance
- Easier testing

### Why MVVM?
- Clear separation of concerns
- Testable business logic
- Lifecycle-aware components
- Recommended by Google

### Why Firebase?
- Quick setup
- Real-time capabilities
- Scalable infrastructure
- Built-in authentication
- Push notifications

### Why Kotlin Coroutines?
- Simplified async code
- Structured concurrency
- Better than callbacks
- Native Android support

## 📚 Further Reading

- [Android Architecture Guide](https://developer.android.com/topic/architecture)
- [Jetpack Compose Documentation](https://developer.android.com/jetpack/compose)
- [Firebase Documentation](https://firebase.google.com/docs)
- [Kotlin Coroutines Guide](https://kotlinlang.org/docs/coroutines-guide.html)

---

**Architecture designed for scalability, maintainability, and testability**
