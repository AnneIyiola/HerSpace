# HerSpace - System Architecture

## Overview

HerSpace is built as a cross-platform mobile application with a cloud-based backend infrastructure. The architecture supports three distinct user views (Period Lady, Pregnant Woman, Partner) while maintaining data synchronization and real-time updates.

## Technology Stack

### Frontend
- **Framework:** React Native
- **Why:** Single codebase for iOS and Android, native performance, large community support
- **UI Library:** React Native Elements + custom components
- **State Management:** Redux Toolkit for global state, React Context for view-specific state
- **Navigation:** React Navigation v6

### Backend
- **Runtime:** Node.js (v18 LTS)
- **Framework:** Express.js
- **Why:** JavaScript full-stack consistency, excellent for real-time features, large ecosystem
- **API Design:** RESTful APIs with JWT authentication
- **Real-time:** Socket.io for live notifications and community features

### Database
- **Primary Database:** MongoDB
- **Why:** Flexible schema for different user types (period tracking vs pregnancy), excellent for nested data structures
- **Caching:** Redis for session management and frequently accessed data
- **File Storage:** AWS S3 for user photos, diary videos, medical documents

### AI/ML Components
- **Cycle Prediction:** TensorFlow.js models trained on aggregated cycle data
- **Symptom Analysis:** Custom ML model using doctor-verified symptom-disease mappings
- **Deployment:** Cloud-based inference via AWS SageMaker

### Third-Party Integrations
- **Maps & Location:** Google Maps API
- **Ride Services:** Bolt API, InDrive API
- **Video Calls:** Twilio Video API
- **Push Notifications:** Firebase Cloud Messaging
- **Analytics:** Firebase Analytics + Mixpanel

## System Architecture Diagram
```
[Mobile Apps - iOS/Android]
         |
         | HTTPS/WSS
         ↓
[API Gateway / Load Balancer]
         |
         ↓
[Express.js Backend Servers]
    |    |    |    |
    ↓    ↓    ↓    ↓
[MongoDB] [Redis] [S3] [AI Models]
    |
    ↓
[External APIs: Maps, Rides, Video]
```

## Component Architecture

### 1. User Management Module
- **Authentication:** JWT-based with refresh tokens
- **User Types:** Enum-based role system (PeriodLady, PregnantWoman, Partner)
- **Partner Linking:** Unique invite codes for secure account linking
- **Profile Management:** Separate schemas for each user type

### 2. Tracking Module
**Period Tracking:**
- Daily symptom logging
- Cycle length calculation
- Ovulation prediction using historical data
- Mood and wellness tracking

**Pregnancy Tracking:**
- Week-by-week progress
- Trimester-specific insights
- Appointment scheduling
- Symptom logging with severity levels

### 3. AI Diagnostics Module
```
User Input (Symptoms) 
    → Backend Processing 
    → ML Model Inference 
    → Doctor-Verified Condition Matching 
    → Return Possible Causes + Specialist Recommendation
```

- **Data Flow:** Symptoms → Vector embedding → Similarity matching with verified conditions
- **Safety:** Always includes disclaimer and specialist recommendation
- **Learning:** Model improves with doctor consultations feedback

### 4. Healthcare Locator Module
- **Integration:** Google Maps Geocoding + Places API
- **Features:**
  - Nearby hospital search
  - Specialist filtering
  - Distance calculation
  - Reviews aggregation
- **Ride Booking:** Real-time price comparison from Bolt/InDrive APIs
- **Deep Linking:** Direct app-to-app ride booking

### 5. Community Module
- **Daily Diary:** Text/video posts with privacy controls (anonymous/public)
- **Support Groups:** Topic-based forums (PCOS, Pregnancy, etc.)
- **Real-time Chat:** Socket.io for instant messaging
- **Moderation:** Automated content filtering + human moderators

### 6. Telemedicine Module
- **Video Calls:** Twilio integration for doctor consultations
- **Scheduling:** Calendar sync with Google Calendar
- **Payment:** Stripe integration for consultation fees
- **Medical Records:** Secure document storage in encrypted S3 buckets

## Data Models

### User Schema (MongoDB)
```javascript
{
  _id: ObjectId,
  userType: "PeriodLady" | "PregnantWoman" | "Partner",
  profile: {
    name: String,
    age: Number,
    email: String,
    phone: String
  },
  linkedPartner: ObjectId,
  inviteCode: String,
  createdAt: Date
}
```

### Cycle Tracking Schema
```javascript
{
  userId: ObjectId,
  cycles: [{
    startDate: Date,
    endDate: Date,
    flowIntensity: String,
    symptoms: [String],
    mood: String,
    notes: String
  }],
  predictions: {
    nextPeriod: Date,
    nextOvulation: Date,
    cycleLength: Number
  }
}
```

### Pregnancy Tracking Schema
```javascript
{
  userId: ObjectId,
  dueDate: Date,
  confirmationMethod: "doctor" | "test",
  currentWeek: Number,
  appointments: [{
    date: Date,
    doctor: String,
    notes: String,
    sharedWithPartner: Boolean
  }],
  weeklyLogs: [{
    week: Number,
    symptoms: [String],
    mood: String,
    insights: String
  }]
}
```

## API Structure

### Core Endpoints
```
POST   /api/auth/register
POST   /api/auth/login
POST   /api/auth/link-partner

GET    /api/tracking/cycle
POST   /api/tracking/cycle/log
GET    /api/tracking/pregnancy
POST   /api/tracking/pregnancy/log

POST   /api/ai/diagnose
GET    /api/ai/insights

GET    /api/healthcare/specialists
GET    /api/healthcare/hospitals
POST   /api/healthcare/book-appointment

POST   /api/community/post
GET    /api/community/feed
POST   /api/community/join-group

POST   /api/telemedicine/schedule
GET    /api/telemedicine/sessions
```

## Security Measures

1. **Authentication:** JWT with 15-minute access tokens, 7-day refresh tokens
2. **Data Encryption:** AES-256 for sensitive health data at rest
3. **HTTPS:** TLS 1.3 for all API communication
4. **Partner Access:** Read-only permissions with explicit sharing consent
5. **Medical Data:** HIPAA-compliant storage and transmission
6. **API Rate Limiting:** 100 requests/minute per user

## Scalability Approach

1. **Horizontal Scaling:** Load-balanced Express servers behind AWS ALB
2. **Database Sharding:** MongoDB sharding by user region
3. **Caching Strategy:** Redis for user sessions, cycle predictions, feed data
4. **CDN:** CloudFront for static assets and media files
5. **Async Processing:** Bull queue for AI inference and notification sending

## Development Workflow

1. **Version Control:** Git with feature branch strategy
2. **CI/CD:** GitHub Actions for automated testing and deployment
3. **Testing:** Jest for unit tests, Detox for E2E mobile testing
4. **Monitoring:** Sentry for error tracking, AWS CloudWatch for infrastructure
5. **Documentation:** Swagger/OpenAPI for API documentation

## Why This Architecture Works

1. **React Native:** Fastest path to both iOS and Android with single codebase
2. **MongoDB:** Perfect for flexible health data that varies by user type
3. **Node.js:** Enables real-time features crucial for partner sync and community
4. **Microservices-Ready:** Modular design allows extracting AI/telemedicine into separate services as we scale
5. **Cloud-Native:** AWS infrastructure provides reliability and global reach
6. **API-First:** Clean separation allows web version or third-party integrations later

## Future Technical Enhancements

- [ ] Wearable device integration (Apple Watch, Fitbit)
- [ ] Offline-first architecture with sync
- [ ] GraphQL API for more efficient data fetching
- [ ] Machine learning model for pregnancy complication prediction
- [ ] Multi-language support with i18n
- [ ] Voice-activated symptom logging

*Built with care for women's health and privacy.*
## System Architecture Diagram
```mermaid
graph TB
    subgraph Client
        A[Mobile App - iOS]
        B[Mobile App - Android]
    end
    
    subgraph Backend
        C[API Gateway]
        D[Express.js Server]
        E[Authentication Service]
        F[AI Service]
    end
    
    subgraph Database
        G[(MongoDB)]
        H[(Redis Cache)]
        I[AWS S3 Storage]
    end
    
    subgraph External
        J[Google Maps API]
        K[Bolt/InDrive API]
        L[Twilio Video]
    end
    
    A --> C
    B --> C
    C --> D
    D --> E
    D --> F
    D --> G
    D --> H
    D --> I
    D --> J
    D --> K
    D --> L