# HerSpace
A comprehensive women's health platform connecting tracking, healthcare access, and community support
# HerSpace

## What is HerSpace?

HerSpace is a comprehensive women's health platform that brings together everything women need to manage their reproductive health journey - all in one place. No more juggling between period trackers, pregnancy apps, hospital searches, and support groups.

## Why HerSpace Matters

Women currently use 7+ different apps to manage their health:
- Period tracking apps
- Pregnancy monitoring apps
- Google Maps for hospital locations
- Ride-sharing apps for appointments
- WhatsApp groups for support
- Separate telemedicine platforms

HerSpace connects all these scattered pieces into one seamless ecosystem.

## Who Is It For?

- **Women tracking their menstrual cycle** - Get accurate predictions, symptom logging, and wellness insights
- **Women who are pregnant** - Track your pregnancy journey with weekly updates and appointment management
- **Partners** - Understand and support your partner through shared calendars and educational content
- **Women seeking healthcare** - Find specialists, book appointments, and access AI-powered health insights

## Key Features

### Three View Structure
1. **Period Lady View** - Complete cycle tracking with mood and symptom logging
2. **Pregnant Woman View** - Pregnancy journey tracking with trimester-specific insights
3. **Partner View (HisSpace)** - Shared dashboard for partners to stay informed and supportive

### Premium+ Features
- **Meet Your Specialist** - Connect with gynecologists virtually or find nearby hospitals with integrated maps and ride-booking
- **AI Diagnostics** - Log symptoms and get possible insights (with professional consultation recommendations)
- **Daily Diary** - Share your journey anonymously or publicly with the community
- **Community & Counselors** - Connect with verified counselors and join support groups

## Technologies Used

- **Frontend:** React Native (mobile apps for iOS & Android)
- **Backend:** Node.js with Express
- **Database:** MongoDB
- **AI:** Machine learning models for predictive tracking
- **Integrations:** Google Maps API, Bolt/InDrive APIs, Video calling SDKs

## Our Mission

To make women's health support visible, accessible, and connected - creating a world where women can relate to one another and access comprehensive care without the fragmentation.

*Making history, one woman at a time.*
## User Flow Diagram
mermaid
graph TD
    A[Download App] --> B{User Type?}
    B -->|Period Tracking| C[Period Lady View]
    B -->|Pregnant| D[Pregnant Woman View]
    B -->|Partner| E[Partner View]
    
    C --> F[Log Symptoms]
    D --> G[Track Pregnancy]
    E --> H[View Shared Calendar]
    
    F --> I[Access Premium Features]
    G --> I
    H --> I
    
    I --> J[Meet Specialist]
    I --> K[AI Diagnostics]
    I --> L[Community]