# Airbnb Clone - User Stories

## Overview
This document contains user stories derived from the use case diagram, capturing the core interactions and functionalities of the Airbnb Clone system.

---

## User Registration and Authentication

### US-001: User Registration
**As a** new user  
**I want to** create an account  
**So that** I can access the platform and book properties or list my own properties

**Acceptance Criteria:**
- User can register with email and password
- Email validation is performed
- Password must meet security requirements (min 8 characters, special characters)
- User receives verification email
- User can choose role (guest or host)

---

### US-002: User Login
**As a** registered user  
**I want to** log in to my account  
**So that** I can access my profile and use platform features

**Acceptance Criteria:**
- User can log in with email and password
- Invalid credentials show appropriate error message
- Successful login generates authentication token
- User is redirected to dashboard after login
- Option for "remember me" functionality

---

### US-003: Password Reset
**As a** user who forgot my password  
**I want to** reset my password  
**So that** I can regain access to my account

**Acceptance Criteria:**
- User can request password reset via email
- Reset link is sent to registered email
- Link expires after 1 hour
- User can set new password
- Old password is invalidated

---

## Property Search and Discovery

### US-004: Search Properties
**As a** guest  
**I want to** search for properties by location and dates  
**So that** I can find accommodations for my trip

**Acceptance Criteria:**
- User can enter location (city, address, coordinates)
- User can select check-in and check-out dates
- User can specify number of guests
- Search returns list of available properties
- Results show key property details and photos

---

### US-005: Filter Search Results
**As a** guest  
**I want to** filter and sort search results  
**So that** I can find properties that meet my specific needs

**Acceptance Criteria:**
- User can filter by price range
- User can filter by property type
- User can filter by amenities (WiFi, parking, pool, etc.)
- User can sort by price (low to high, high to low)
- User can sort by rating
- Filters update results in real-time

---

### US-006: View Property Details
**As a** guest  
**I want to** view detailed information about a property  
**So that** I can make an informed booking decision

**Acceptance Criteria:**
- Property page shows all photos
- Property details include description, amenities, location
- Property page shows host information
- Property page displays reviews and ratings
- Property page shows availability calendar
- Property page shows exact location on map

---

## Property Listing Management

### US-007: Create Property Listing
**As a** host  
**I want to** list my property on the platform  
**So that** I can rent it out to guests and earn income

**Acceptance Criteria:**
- Host can add property title and description
- Host can upload multiple photos (minimum 5)
- Host can set location and address
- Host can specify property type and capacity
- Host can list amenities
- Host can set pricing per night
- Listing is saved as draft or published immediately

---

### US-008: Manage Property Listing
**As a** host  
**I want to** edit my property details  
**So that** I can keep information accurate and up-to-date

**Acceptance Criteria:**
- Host can edit all property fields
- Host can add/remove photos
- Host can update pricing
- Host can deactivate/reactivate listing
- Host can delete listing
- Changes are reflected immediately

---

### US-009: Set Property Availability
**As a** host  
**I want to** manage my property's availability calendar  
**So that** I can control when my property is available for booking

**Acceptance Criteria:**
- Host can block specific dates
- Host can set recurring unavailable dates
- Host can sync with external calendars
- Blocked dates cannot be booked by guests
- Calendar shows existing bookings

---

## Booking Management

### US-010: Make a Booking
**As a** guest  
**I want to** book a property for specific dates  
**So that** I can secure accommodation for my trip

**Acceptance Criteria:**
- Guest can select check-in and check-out dates
- System validates availability
- Guest can see total price breakdown
- Guest can add special requests
- Guest receives booking confirmation
- Host receives booking notification

---

### US-011: View Booking Details
**As a** guest  
**I want to** view my booking information  
**So that** I can see all details about my reservation

**Acceptance Criteria:**
- Guest can see booking confirmation number
- Guest can see property details
- Guest can see check-in/check-out dates
- Guest can see total amount paid
- Guest can see host contact information
- Guest can see cancellation policy

---

### US-012: Cancel Booking
**As a** guest  
**I want to** cancel my booking  
**So that** I can get a refund if my plans change

**Acceptance Criteria:**
- Guest can cancel booking before check-in
- System calculates refund based on cancellation policy
- Guest receives refund confirmation
- Host receives cancellation notification
- Property becomes available again
- Cancellation is reflected in guest's booking history

---

### US-013: Accept/Decline Booking Request
**As a** host  
**I want to** review and respond to booking requests  
**So that** I can control who stays at my property

**Acceptance Criteria:**
- Host receives notification of new booking request
- Host can view guest profile and reviews
- Host can accept or decline within 24 hours
- Guest receives notification of host's decision
- Accepted bookings proceed to payment
- Declined bookings free up the dates

---

### US-014: View Booking History
**As a** user (guest or host)  
**I want to** see my past and upcoming bookings  
**So that** I can track my reservations

**Acceptance Criteria:**
- User can see list of all bookings
- Bookings are categorized (upcoming, past, cancelled)
- Each booking shows key details
- User can filter by date range
- User can search bookings
- User can download booking receipts

---

## Payment Processing

### US-015: Make Payment
**As a** guest  
**I want to** pay for my booking securely  
**So that** I can confirm my reservation

**Acceptance Criteria:**
- Guest can choose payment method (credit card, PayPal, Stripe)
- Payment form is secure (HTTPS, encrypted)
- System validates payment information
- Payment is processed immediately
- Guest receives payment confirmation
- Receipt is emailed to guest

---

### US-016: View Payment History
**As a** user  
**I want to** see my payment history  
**So that** I can track my transactions

**Acceptance Criteria:**
- User can see list of all payments
- Each payment shows date, amount, booking
- User can download receipts/invoices
- Payments are categorized (successful, failed, refunded)
- User can filter by date range

---

### US-017: Process Refund
**As a** guest  
**I want to** receive a refund for cancelled bookings  
**So that** I can get my money back according to the cancellation policy

**Acceptance Criteria:**
- Refund is calculated based on cancellation policy
- Refund is processed to original payment method
- Guest receives refund notification
- Refund appears in payment history
- Refund processing time is communicated

---

### US-018: Receive Payout
**As a** host  
**I want to** receive payment for completed bookings  
**So that** I can earn income from my property

**Acceptance Criteria:**
- Payout is automatically processed after guest check-in
- Service fees are deducted
- Host can see payout breakdown
- Host receives payout notification
- Payout is sent to host's bank account or PayPal
- Host can track payout status

---

## Reviews and Ratings

### US-019: Write Property Review
**As a** guest  
**I want to** leave a review for a property I stayed at  
**So that** I can share my experience with other users

**Acceptance Criteria:**
- Guest can review only after stay is completed
- Guest can rate property (1-5 stars)
- Guest can rate specific categories (cleanliness, location, etc.)
- Guest can write detailed comment
- Guest can upload photos
- Review is published after submission
- Host receives notification of new review

---

### US-020: Respond to Review
**As a** host  
**I want to** respond to guest reviews  
**So that** I can address feedback and clarify any issues

**Acceptance Criteria:**
- Host can see all reviews for their properties
- Host can write one response per review
- Response is displayed with the review
- Host has 30 days to respond
- Response cannot be edited after posting

---

### US-021: View Reviews
**As a** guest  
**I want to** read reviews for a property  
**So that** I can make an informed booking decision

**Acceptance Criteria:**
- All property reviews are visible on property page
- Reviews show rating, comment, date, and reviewer name
- Reviews are sorted by most recent first
- Guest can filter reviews by rating
- Average rating is prominently displayed
- Total number of reviews is shown

---

## Messaging and Communication

### US-022: Send Message to Host
**As a** guest  
**I want to** message the host before booking  
**So that** I can ask questions about the property

**Acceptance Criteria:**
- Guest can send message from property page
- Message is delivered to host in real-time
- Host receives email notification
- Guest can see message history
- Guest can attach photos to messages

---

### US-023: Send Message to Guest
**As a** host  
**I want to** message guests  
**So that** I can provide check-in instructions and answer questions

**Acceptance Criteria:**
- Host can send messages to guests with bookings
- Message is delivered in real-time
- Guest receives notification
- Host can see all conversations
- Host can attach documents to messages

---

### US-024: Receive Notifications
**As a** user  
**I want to** receive notifications about important events  
**So that** I stay informed about my bookings and messages

**Acceptance Criteria:**
- User receives email notifications
- User receives in-app notifications
- User can customize notification preferences
- Notifications include: booking confirmations, messages, reviews, payment confirmations
- User can enable/disable specific notification types

---

## Admin Functions

### US-025: Manage Users
**As an** admin  
**I want to** view and manage user accounts  
**So that** I can maintain platform quality and handle issues

**Acceptance Criteria:**
- Admin can view list of all users
- Admin can search users by email or name
- Admin can view user details and activity
- Admin can suspend or delete user accounts
- Admin can verify host accounts
- Admin actions are logged

---

### US-026: Manage Properties
**As an** admin  
**I want to** review and manage property listings  
**So that** I can ensure quality and remove inappropriate content

**Acceptance Criteria:**
- Admin can view all property listings
- Admin can approve/reject new listings
- Admin can deactivate inappropriate listings
- Admin can edit property details if necessary
- Admin actions are logged and notified to hosts

---

### US-027: Handle Disputes
**As an** admin  
**I want to** resolve disputes between guests and hosts  
**So that** I can maintain trust and fairness on the platform

**Acceptance Criteria:**
- Admin can view dispute reports
- Admin can communicate with both parties
- Admin can cancel bookings
- Admin can process refunds
- Admin decisions are documented
- Both parties receive notification of resolution

---

### US-028: View Analytics
**As an** admin  
**I want to** access platform analytics  
**So that** I can monitor business performance and make data-driven decisions

**Acceptance Criteria:**
- Admin can view user registration trends
- Admin can see booking statistics
- Admin can track revenue metrics
- Admin can see popular locations and properties
- Admin can export reports
- Dashboard shows key metrics at a glance

---

## Additional Features

### US-029: Add to Wishlist
**As a** guest  
**I want to** save properties to a wishlist  
**So that** I can easily find and compare them later

**Acceptance Criteria:**
- Guest can add property to wishlist from property page
- Guest can create multiple wishlists
- Guest can view all wishlist items
- Guest can remove items from wishlist
- Guest receives notifications about price changes on wishlist items

---

### US-030: Update Profile
**As a** user  
**I want to** update my profile information  
**So that** I can keep my account details current

**Acceptance Criteria:**
- User can edit name, email, phone number
- User can upload profile photo
- User can update password
- User can set language preference
- User can set currency preference
- Changes are saved immediately

---

**Total User Stories:** 30

**Priority Levels:**
- **High Priority (Must Have):** US-001 to US-018 (Core functionality)
- **Medium Priority (Should Have):** US-019 to US-024 (Enhanced user experience)
- **Low Priority (Nice to Have):** US-025 to US-030 (Admin and additional features)

---

**Author:** Jason Rippon  
**Project:** ALX Airbnb Clone - Backend Documentation  
**Date:** October 26, 2025
