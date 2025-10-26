# Data Flow Diagram - Airbnb Clone Backend

## Level 0 - Context Diagram

```mermaid
graph LR
    Guest[Guest User] -->|Registration, Bookings, Payments| System[Airbnb Clone System]
    Host[Host User] -->|Property Listings, Availability| System
    Admin[Admin] -->|User/Property Management| System
    System -->|Booking Confirmations, Notifications| Guest
    System -->|Booking Requests, Payouts| Host
    System -->|Reports, Analytics| Admin
    PaymentGateway[Payment Gateway] -->|Payment Confirmation| System
    System -->|Payment Processing| PaymentGateway
    EmailService[Email Service] -->|Email Delivery Status| System
    System -->|Email Notifications| EmailService
```

## Level 1 - Major Processes

```mermaid
graph TB
    subgraph External Entities
        Guest[Guest]
        Host[Host]
        Admin[Admin]
        PayGW[Payment Gateway]
        Email[Email Service]
    end
    
    subgraph Processes
        P1[1.0 User Authentication]
        P2[2.0 Property Management]
        P3[3.0 Booking Management]
        P4[4.0 Payment Processing]
        P5[5.0 Review Management]
        P6[6.0 Messaging System]
    end
    
    subgraph Data Stores
        DS1[(User Database)]
        DS2[(Property Database)]
        DS3[(Booking Database)]
        DS4[(Payment Database)]
        DS5[(Review Database)]
        DS6[(Message Database)]
    end
    
    Guest -->|Login Credentials| P1
    Guest -->|Search Criteria| P2
    Guest -->|Booking Request| P3
    Guest -->|Payment Info| P4
    Guest -->|Review Data| P5
    Guest -->|Messages| P6
    
    Host -->|Property Details| P2
    Host -->|Availability| P3
    Host -->|Messages| P6
    
    Admin -->|Management Commands| P1
    Admin -->|Management Commands| P2
    Admin -->|Management Commands| P3
    
    P1 -->|User Data| DS1
    P2 -->|Property Data| DS2
    P3 -->|Booking Data| DS3
    P4 -->|Payment Data| DS4
    P5 -->|Review Data| DS5
    P6 -->|Message Data| DS6
    
    P4 <-->|Payment Request/Response| PayGW
    P1 -->|Email Request| Email
    P3 -->|Notification Request| Email
    P4 -->|Receipt Email| Email
```

## Level 2 - Detailed Process: User Authentication

```mermaid
graph TB
    Guest[Guest] -->|Registration Data| P1.1[1.1 Validate Input]
    P1.1 -->|Valid Data| P1.2[1.2 Hash Password]
    P1.2 -->|Hashed Password| P1.3[1.3 Store User]
    P1.3 -->|User Record| DS1[(User DB)]
    P1.3 -->|Verification Email| Email[Email Service]
    
    Guest2[User] -->|Login Credentials| P1.4[1.4 Verify Credentials]
    DS1 -->|User Data| P1.4
    P1.4 -->|Valid User| P1.5[1.5 Generate JWT]
    P1.5 -->|Access Token| Guest2
```

## Level 2 - Detailed Process: Booking Management

```mermaid
graph TB
    Guest[Guest] -->|Booking Request| P3.1[3.1 Validate Dates]
    P3.1 -->|Valid Dates| P3.2[3.2 Check Availability]
    DS2[(Property DB)] -->|Property Data| P3.2
    DS3[(Booking DB)] -->|Existing Bookings| P3.2
    P3.2 -->|Available| P3.3[3.3 Calculate Price]
    P3.3 -->|Total Amount| P3.4[3.4 Create Booking]
    P3.4 -->|Booking Record| DS3
    P3.4 -->|Booking Confirmation| Guest
    P3.4 -->|Booking Notification| Host[Host]
    P3.4 -->|Payment Required| P4[4.0 Payment Processing]
```

## Level 2 - Detailed Process: Property Management

```mermaid
graph TB
    Host[Host] -->|Property Details| P2.1[2.1 Validate Property Data]
    P2.1 -->|Valid Data| P2.2[2.2 Upload Photos]
    P2.2 -->|Photos| Storage[File Storage]
    Storage -->|Photo URLs| P2.3[2.3 Create Listing]
    P2.3 -->|Property Record| DS2[(Property DB)]
    P2.3 -->|Listing Confirmation| Host
    
    Guest[Guest] -->|Search Query| P2.4[2.4 Search Properties]
    DS2 -->|Property Data| P2.4
    P2.4 -->|Search Results| Guest
```

## Data Flow - Complete Booking Process

```mermaid
sequenceDiagram
    participant G as Guest
    participant S as System
    participant DB as Database
    participant PG as Payment Gateway
    participant H as Host
    participant E as Email Service

    G->>S: 1. Search Properties
    S->>DB: Query Available Properties
    DB-->>S: Property List
    S-->>G: Display Results
    
    G->>S: 2. Select Property & Dates
    S->>DB: Check Availability
    DB-->>S: Availability Confirmed
    S-->>G: Show Price Breakdown
    
    G->>S: 3. Create Booking
    S->>DB: Lock Dates (Transaction)
    DB-->>S: Dates Locked
    S-->>G: Booking Pending Payment
    
    G->>S: 4. Submit Payment
    S->>PG: Process Payment
    PG-->>S: Payment Confirmed
    S->>DB: Update Booking Status
    DB-->>S: Booking Confirmed
    
    S->>E: Send Confirmation Email
    E-->>G: Email Delivered
    S->>E: Notify Host
    E-->>H: Email Delivered
    
    S-->>G: Booking Complete
```

## Data Stores

### 1. User Database
**Stores:** User credentials, profiles, roles
**Accessed by:** Authentication, Profile Management, Booking processes
**Data:** user_id, email, password_hash, first_name, last_name, role, created_at

### 2. Property Database
**Stores:** Property listings, details, photos, amenities
**Accessed by:** Property Management, Search, Booking processes
**Data:** property_id, host_id, name, description, location, price, photos

### 3. Booking Database
**Stores:** Booking records, dates, status
**Accessed by:** Booking Management, Payment, Calendar processes
**Data:** booking_id, property_id, user_id, check_in, check_out, total_price, status

### 4. Payment Database
**Stores:** Payment transactions, receipts
**Accessed by:** Payment Processing, Refund processes
**Data:** payment_id, booking_id, amount, payment_method, status

### 5. Review Database
**Stores:** Guest reviews, ratings, comments
**Accessed by:** Review Management, Property Display
**Data:** review_id, property_id, user_id, rating, comment

### 6. Message Database
**Stores:** User-to-user messages
**Accessed by:** Messaging System
**Data:** message_id, sender_id, recipient_id, message_body, sent_at

## External Entities

### Input Data Flows
- **Guest → System:** Registration data, search queries, booking requests, payment info, reviews, messages
- **Host → System:** Property details, photos, availability updates, responses
- **Admin → System:** Management commands, user/property actions
- **Payment Gateway → System:** Payment confirmations, transaction status
- **Email Service → System:** Delivery status, bounce notifications

### Output Data Flows
- **System → Guest:** Search results, booking confirmations, payment receipts, notifications
- **System → Host:** Booking notifications, payment transfers, guest messages
- **System → Admin:** Analytics reports, system metrics
- **System → Payment Gateway:** Payment requests, refund requests
- **System → Email Service:** Notification emails, verification emails

## Data Flow Notation

```
┌─────────┐
│ Process │  = System Process (numbered)
└─────────┘

┌─────────┐
│ Entity  │  = External Entity
└─────────┘

  ┌───┐
  │DB │    = Data Store
  └───┘

  ───▶     = Data Flow (shows data movement)
```

---

**Author:** Jason Rippon  
**Date:** October 26, 2025  
**Project:** ALX Airbnb Clone Documentation
