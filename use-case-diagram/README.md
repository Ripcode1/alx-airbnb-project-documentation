# Airbnb Clone - Use Case Diagram

## Overview
This use case diagram visualizes the interactions between different actors (users) and the Airbnb Clone system for key functionalities.

---

## Actors

### 1. Guest
A user who searches for and books properties.

### 2. Host
A user who lists and manages properties for rental.

### 3. Admin
System administrator who manages the platform.

### 4. System
The backend system that processes requests and manages data.

---

## Use Cases

### Guest Use Cases

#### 1. Register Account
- **Actor:** Guest
- **Description:** New user creates an account on the platform
- **Preconditions:** None
- **Postconditions:** User account is created and verified

#### 2. Login
- **Actor:** Guest, Host, Admin
- **Description:** User authenticates to access the system
- **Preconditions:** User must have a registered account
- **Postconditions:** User is authenticated and receives access token

#### 3. Search Properties
- **Actor:** Guest
- **Description:** Search for available properties based on criteria
- **Preconditions:** None (can search without login)
- **Postconditions:** List of matching properties is displayed

#### 4. View Property Details
- **Actor:** Guest
- **Description:** View detailed information about a specific property
- **Preconditions:** Property must exist
- **Postconditions:** Property details are displayed

#### 5. Make Booking
- **Actor:** Guest
- **Description:** Book a property for specific dates
- **Preconditions:** User must be logged in, property must be available
- **Postconditions:** Booking request is created

#### 6. Make Payment
- **Actor:** Guest
- **Description:** Process payment for a booking
- **Preconditions:** Booking must be confirmed
- **Postconditions:** Payment is processed and booking is paid

#### 7. Cancel Booking
- **Actor:** Guest
- **Description:** Cancel an existing booking
- **Preconditions:** Booking must exist and be cancellable
- **Postconditions:** Booking is cancelled, refund is processed

#### 8. Write Review
- **Actor:** Guest
- **Description:** Leave a review and rating for a stayed property
- **Preconditions:** Must have completed a stay
- **Postconditions:** Review is published

#### 9. Send Message to Host
- **Actor:** Guest
- **Description:** Communicate with property host
- **Preconditions:** Must be logged in
- **Postconditions:** Message is sent to host

#### 10. Manage Profile
- **Actor:** Guest, Host
- **Description:** Update personal information and settings
- **Preconditions:** Must be logged in
- **Postconditions:** Profile is updated

---

### Host Use Cases

#### 11. List Property
- **Actor:** Host
- **Description:** Create a new property listing
- **Preconditions:** User must be logged in as host
- **Postconditions:** Property is created and available for booking

#### 12. Manage Property
- **Actor:** Host
- **Description:** Edit or delete existing property listings
- **Preconditions:** Host must own the property
- **Postconditions:** Property information is updated or removed

#### 13. Set Pricing
- **Actor:** Host
- **Description:** Set and update property pricing
- **Preconditions:** Host must own the property
- **Postconditions:** Pricing is updated

#### 14. Manage Calendar
- **Actor:** Host
- **Description:** Set property availability
- **Preconditions:** Host must own the property
- **Postconditions:** Availability calendar is updated

#### 15. Accept/Decline Booking
- **Actor:** Host
- **Description:** Respond to booking requests
- **Preconditions:** Booking request must exist
- **Postconditions:** Booking is confirmed or declined

#### 16. View Bookings
- **Actor:** Host
- **Description:** See all bookings for owned properties
- **Preconditions:** Host must be logged in
- **Postconditions:** Booking list is displayed

#### 17. Respond to Reviews
- **Actor:** Host
- **Description:** Reply to guest reviews
- **Preconditions:** Review must exist for host's property
- **Postconditions:** Response is published

#### 18. Send Message to Guest
- **Actor:** Host
- **Description:** Communicate with guests
- **Preconditions:** Must be logged in
- **Postconditions:** Message is sent to guest

---

### Admin Use Cases

#### 19. Manage Users
- **Actor:** Admin
- **Description:** View, suspend, or delete user accounts
- **Preconditions:** Admin must be logged in
- **Postconditions:** User account status is modified

#### 20. Manage Properties
- **Actor:** Admin
- **Description:** Review, approve, or remove property listings
- **Preconditions:** Admin must be logged in
- **Postconditions:** Property status is modified

#### 21. Manage Bookings
- **Actor:** Admin
- **Description:** View and manage all bookings, handle disputes
- **Preconditions:** Admin must be logged in
- **Postconditions:** Booking status may be modified

#### 22. Process Refunds
- **Actor:** Admin
- **Description:** Manually process refunds for cancellations or disputes
- **Preconditions:** Admin must be logged in, refund request exists
- **Postconditions:** Refund is processed

#### 23. View Analytics
- **Actor:** Admin
- **Description:** Access system analytics and reports
- **Preconditions:** Admin must be logged in
- **Postconditions:** Analytics dashboard is displayed

#### 24. Manage Payments
- **Actor:** Admin
- **Description:** Monitor transactions and handle payment issues
- **Preconditions:** Admin must be logged in
- **Postconditions:** Payment issues are resolved

---

### System Use Cases

#### 25. Send Notifications
- **Actor:** System
- **Description:** Send automated notifications (email, SMS, push)
- **Triggered by:** Various user actions
- **Postconditions:** Users receive notifications

#### 26. Process Scheduled Payments
- **Actor:** System
- **Description:** Automatically process payouts to hosts
- **Triggered by:** Scheduled job after guest check-in
- **Postconditions:** Host receives payment

#### 27. Update Property Status
- **Actor:** System
- **Description:** Automatically update property availability
- **Triggered by:** Confirmed bookings
- **Postconditions:** Property calendar is updated

#### 28. Generate Reports
- **Actor:** System
- **Description:** Create automated reports and analytics
- **Triggered by:** Scheduled jobs
- **Postconditions:** Reports are generated

---

## Relationships

### Include Relationships
- Make Booking **includes** Make Payment
- Register Account **includes** Send Verification Email
- List Property **includes** Upload Photos
- Write Review **includes** Rate Property

### Extend Relationships
- Make Booking **extends** Apply Discount Code
- Search Properties **extends** Apply Filters
- Login **extends** Two-Factor Authentication
- Make Payment **extends** Choose Payment Method

### Generalization
- Guest, Host, Admin **generalize** User
- All user types inherit from base User actor

---

## Use Case Diagram

The use case diagram should include:
- All actors positioned on the left and right sides
- Use cases as ovals in the center
- Lines connecting actors to their use cases
- Include/extend relationships shown with dashed arrows
- System boundary box containing all use cases

**Note:** The actual diagram should be created in Draw.io and exported as a PNG file.

---

**Author:** Jason Rippon  
**Project:** ALX Airbnb Clone - Backend Documentation  
**Date:** October 26, 2025
