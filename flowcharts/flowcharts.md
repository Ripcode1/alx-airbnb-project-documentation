# Flowcharts - Airbnb Clone System Processes

## 1. User Registration Flowchart

```mermaid
flowchart TD
    Start([User Visits Registration Page]) --> Input[User Enters:<br/>- Email<br/>- Password<br/>- Name<br/>- Role]
    Input --> ValidateEmail{Is Email<br/>Valid Format?}
    
    ValidateEmail -->|No| ErrorEmail[Display: Invalid Email Format]
    ErrorEmail --> Input
    
    ValidateEmail -->|Yes| CheckUnique{Is Email<br/>Unique?}
    CheckUnique -->|No| ErrorExists[Display: Email Already Exists]
    ErrorExists --> Input
    
    CheckUnique -->|Yes| ValidatePassword{Password<br/>Meets Requirements?}
    ValidatePassword -->|No| ErrorPassword[Display: Password Too Weak]
    ErrorPassword --> Input
    
    ValidatePassword -->|Yes| HashPassword[Hash Password<br/>Using bcrypt]
    HashPassword --> SaveUser[Save User to Database]
    SaveUser --> GenerateToken[Generate Verification Token]
    GenerateToken --> SendEmail[Send Verification Email]
    SendEmail --> DisplaySuccess[Display: Registration Successful<br/>Check Your Email]
    DisplaySuccess --> End([End])
```

## 2. User Login Flowchart

```mermaid
flowchart TD
    Start([User Visits Login Page]) --> Input[User Enters:<br/>- Email<br/>- Password]
    Input --> CheckEmail{Email<br/>Exists?}
    
    CheckEmail -->|No| ErrorNotFound[Display: User Not Found]
    ErrorNotFound --> Input
    
    CheckEmail -->|Yes| CheckVerified{Account<br/>Verified?}
    CheckVerified -->|No| ErrorNotVerified[Display: Please Verify Email]
    ErrorNotVerified --> End1([End])
    
    CheckVerified -->|Yes| CheckSuspended{Account<br/>Suspended?}
    CheckSuspended -->|Yes| ErrorSuspended[Display: Account Suspended]
    ErrorSuspended --> End2([End])
    
    CheckSuspended -->|No| VerifyPassword{Password<br/>Correct?}
    VerifyPassword -->|No| IncrementFailed[Increment Failed Attempts]
    IncrementFailed --> CheckAttempts{Failed Attempts<br/>> 5?}
    CheckAttempts -->|Yes| LockAccount[Lock Account for 15 Minutes]
    LockAccount --> ErrorLocked[Display: Too Many Attempts<br/>Try Again Later]
    ErrorLocked --> End3([End])
    CheckAttempts -->|No| ErrorPassword[Display: Invalid Password]
    ErrorPassword --> Input
    
    VerifyPassword -->|Yes| ResetAttempts[Reset Failed Attempts to 0]
    ResetAttempts --> GenerateJWT[Generate JWT Tokens:<br/>- Access Token 15min<br/>- Refresh Token 7 days]
    GenerateJWT --> LogActivity[Log Login Activity]
    LogActivity --> ReturnTokens[Return Tokens to User]
    ReturnTokens --> Success[Display: Login Successful<br/>Redirect to Dashboard]
    Success --> End4([End])
```

## 3. Property Booking Flowchart

```mermaid
flowchart TD
    Start([Guest Selects Property]) --> CheckAuth{User<br/>Logged In?}
    
    CheckAuth -->|No| RedirectLogin[Redirect to Login Page]
    RedirectLogin --> End1([End])
    
    CheckAuth -->|Yes| InputDates[User Enters:<br/>- Check-in Date<br/>- Check-out Date<br/>- Number of Guests]
    
    InputDates --> ValidateFuture{Check-in Date<br/>in Future?}
    ValidateFuture -->|No| ErrorDate1[Display: Check-in Must Be Future Date]
    ErrorDate1 --> InputDates
    
    ValidateFuture -->|Yes| ValidateCheckout{Check-out After<br/>Check-in?}
    ValidateCheckout -->|No| ErrorDate2[Display: Invalid Date Range]
    ErrorDate2 --> InputDates
    
    ValidateCheckout -->|Yes| ValidateGuests{Guests ≤<br/>Max Capacity?}
    ValidateGuests -->|No| ErrorGuests[Display: Too Many Guests]
    ErrorGuests --> InputDates
    
    ValidateGuests -->|Yes| CheckOwner{User is<br/>Property Owner?}
    CheckOwner -->|Yes| ErrorOwner[Display: Cannot Book Own Property]
    ErrorOwner --> End2([End])
    
    CheckOwner -->|No| StartTransaction[Begin Database Transaction]
    StartTransaction --> CheckAvailability{Property Available<br/>for Dates?}
    
    CheckAvailability -->|No| RollbackTrans1[Rollback Transaction]
    RollbackTrans1 --> ErrorUnavailable[Display: Property Not Available]
    ErrorUnavailable --> End3([End])
    
    CheckAvailability -->|Yes| LockDates[Lock Property Dates]
    LockDates --> CalculatePrice[Calculate:<br/>- Base Price<br/>- Service Fee 10%<br/>- Taxes 15%<br/>- Grand Total]
    
    CalculatePrice --> DisplayBreakdown[Display Price Breakdown<br/>to User]
    DisplayBreakdown --> ConfirmBooking{User Confirms<br/>Booking?}
    
    ConfirmBooking -->|No| RollbackTrans2[Rollback Transaction]
    RollbackTrans2 --> End4([End])
    
    ConfirmBooking -->|Yes| CreateBooking[Create Booking Record<br/>Status: Pending]
    CreateBooking --> CommitTrans[Commit Transaction]
    CommitTrans --> ProcessPayment[Redirect to Payment]
    
    ProcessPayment --> PaymentSuccess{Payment<br/>Successful?}
    
    PaymentSuccess -->|No| UpdateFailed[Update Booking Status: Failed]
    UpdateFailed --> ReleaseDates[Release Locked Dates]
    ReleaseDates --> ErrorPayment[Display: Payment Failed]
    ErrorPayment --> End5([End])
    
    PaymentSuccess -->|Yes| UpdateConfirmed[Update Booking Status: Confirmed]
    UpdateConfirmed --> SavePayment[Save Payment Record]
    SavePayment --> SendEmailGuest[Send Confirmation Email<br/>to Guest]
    SendEmailGuest --> SendEmailHost[Send Booking Notification<br/>to Host]
    SendEmailHost --> DisplaySuccess[Display: Booking Confirmed<br/>Show Booking Details]
    DisplaySuccess --> End6([End])
```

## 4. Property Search Flowchart

```mermaid
flowchart TD
    Start([User Opens Search Page]) --> InputCriteria[User Enters:<br/>- Location<br/>- Check-in/Check-out<br/>- Number of Guests<br/>- Optional Filters]
    
    InputCriteria --> ValidateLocation{Location<br/>Provided?}
    ValidateLocation -->|No| ErrorLocation[Display: Location Required]
    ErrorLocation --> InputCriteria
    
    ValidateLocation -->|Yes| ValidateDates{Dates<br/>Valid?}
    ValidateDates -->|No| ErrorDates[Display: Invalid Dates]
    ErrorDates --> InputCriteria
    
    ValidateDates -->|Yes| QueryDB[Query Database:<br/>1. Filter by Location<br/>2. Filter by Capacity<br/>3. Exclude Booked Dates]
    
    QueryDB --> ApplyFilters{Additional<br/>Filters Applied?}
    ApplyFilters -->|Yes| FilterPrice{Price Range?}
    FilterPrice -->|Yes| ApplyPriceFilter[Filter by Price Range]
    ApplyPriceFilter --> FilterType{Property Type?}
    FilterPrice -->|No| FilterType
    
    FilterType -->|Yes| ApplyTypeFilter[Filter by Property Type]
    ApplyTypeFilter --> FilterAmenities{Amenities?}
    FilterType -->|No| FilterAmenities
    
    FilterAmenities -->|Yes| ApplyAmenitiesFilter[Filter by Amenities]
    ApplyAmenitiesFilter --> SortResults
    FilterAmenities -->|No| SortResults[Sort Results]
    ApplyFilters -->|No| SortResults
    
    SortResults --> CheckCount{Results<br/>Found?}
    CheckCount -->|No| DisplayNoResults[Display: No Properties Found<br/>Suggest Broader Search]
    DisplayNoResults --> End1([End])
    
    CheckCount -->|Yes| Paginate[Apply Pagination<br/>20 Results per Page]
    Paginate --> DisplayResults[Display Property Cards:<br/>- Photo<br/>- Name<br/>- Price<br/>- Rating<br/>- Location]
    DisplayResults --> UserAction{User Action?}
    
    UserAction -->|Click Property| ShowDetails[Show Property Details]
    ShowDetails --> End2([End])
    
    UserAction -->|Change Filters| InputCriteria
    UserAction -->|Next Page| Paginate
    UserAction -->|Exit| End3([End])
```

## 5. Payment Processing Flowchart

```mermaid
flowchart TD
    Start([Payment Required]) --> CheckBooking{Booking<br/>Exists?}
    
    CheckBooking -->|No| Error1[Display: Invalid Booking]
    Error1 --> End1([End])
    
    CheckBooking -->|Yes| DisplayPayment[Display Payment Form:<br/>- Amount Due<br/>- Payment Methods<br/>- Card Details]
    
    DisplayPayment --> SelectMethod[User Selects<br/>Payment Method]
    SelectMethod --> IsCard{Credit/Debit<br/>Card?}
    
    IsCard -->|Yes| InputCard[User Enters:<br/>- Card Number<br/>- Expiry<br/>- CVV<br/>- Name]
    InputCard --> ValidateCard{Valid Card<br/>Format?}
    ValidateCard -->|No| ErrorCard[Display: Invalid Card Details]
    ErrorCard --> InputCard
    
    IsCard -->|No| IsPayPal{PayPal?}
    IsPayPal -->|Yes| RedirectPayPal[Redirect to PayPal]
    RedirectPayPal --> PayPalAuth[User Authenticates on PayPal]
    PayPalAuth --> ProcessPayPalPayment
    
    IsPayPal -->|No| IsStripe{Stripe?}
    IsStripe -->|Yes| RedirectStripe[Redirect to Stripe]
    RedirectStripe --> StripeAuth[User Completes Stripe Flow]
    StripeAuth --> ProcessStripePayment
    
    ValidateCard -->|Yes| EncryptData[Encrypt Payment Data]
    EncryptData --> SendToGateway[Send to Payment Gateway]
    SendToGateway --> GatewayProcess[Gateway Processes Payment]
    
    ProcessPayPalPayment[PayPal Processes Payment] --> CheckResult
    ProcessStripePayment[Stripe Processes Payment] --> CheckResult
    GatewayProcess --> CheckResult{Payment<br/>Successful?}
    
    CheckResult -->|No| LogFailed[Log Failed Payment]
    LogFailed --> CheckRetry{Retry<br/>Attempts < 3?}
    CheckRetry -->|Yes| ErrorRetry[Display: Payment Failed<br/>Try Again]
    ErrorRetry --> DisplayPayment
    CheckRetry -->|No| CancelBooking[Cancel Booking]
    CancelBooking --> ErrorFinal[Display: Payment Failed<br/>Booking Cancelled]
    ErrorFinal --> End2([End])
    
    CheckResult -->|Yes| SavePaymentRecord[Save Payment to Database]
    SavePaymentRecord --> UpdateBookingStatus[Update Booking Status:<br/>Pending → Confirmed]
    UpdateBookingStatus --> GenerateReceipt[Generate Receipt/Invoice]
    GenerateReceipt --> SendReceipt[Email Receipt to Guest]
    SendReceipt --> NotifyHost[Notify Host of Confirmed Booking]
    NotifyHost --> Success[Display: Payment Successful<br/>Show Booking Confirmation]
    Success --> End3([End])
```

## 6. Property Listing Creation Flowchart

```mermaid
flowchart TD
    Start([Host Clicks 'List Property']) --> CheckRole{User Role<br/>= Host?}
    
    CheckRole -->|No| Error1[Display: Only Hosts Can List Properties]
    Error1 --> End1([End])
    
    CheckRole -->|Yes| Step1[Step 1: Basic Information]
    Step1 --> InputBasic[Host Enters:<br/>- Property Name<br/>- Description<br/>- Property Type]
    InputBasic --> ValidateBasic{All Fields<br/>Complete?}
    ValidateBasic -->|No| ErrorBasic[Display: Complete All Fields]
    ErrorBasic --> InputBasic
    
    ValidateBasic -->|Yes| Step2[Step 2: Location]
    Step2 --> InputLocation[Host Enters:<br/>- Address<br/>- City<br/>- Country]
    InputLocation --> Geocode[Geocode Address<br/>Get Lat/Long]
    Geocode --> GeocodeSuccess{Geocoding<br/>Successful?}
    GeocodeSuccess -->|No| ErrorLocation[Display: Invalid Address]
    ErrorLocation --> InputLocation
    
    GeocodeSuccess -->|Yes| Step3[Step 3: Details]
    Step3 --> InputDetails[Host Enters:<br/>- Bedrooms<br/>- Bathrooms<br/>- Max Guests<br/>- Price per Night]
    InputDetails --> ValidateDetails{Valid<br/>Numbers?}
    ValidateDetails -->|No| ErrorDetails[Display: Invalid Values]
    ErrorDetails --> InputDetails
    
    ValidateDetails -->|Yes| Step4[Step 4: Amenities]
    Step4 --> SelectAmenities[Host Selects Amenities:<br/>- WiFi<br/>- Parking<br/>- Pool<br/>- etc.]
    SelectAmenities --> Step5[Step 5: Photos]
    Step5 --> UploadPhotos[Host Uploads Photos]
    
    UploadPhotos --> CheckPhotoCount{At Least<br/>5 Photos?}
    CheckPhotoCount -->|No| ErrorPhotos[Display: Minimum 5 Photos Required]
    ErrorPhotos --> UploadPhotos
    
    CheckPhotoCount -->|Yes| ValidatePhotos{All Photos<br/>Valid?}
    ValidatePhotos -->|No| ErrorPhotoFormat[Display: Invalid Photo Format/Size]
    ErrorPhotoFormat --> UploadPhotos
    
    ValidatePhotos -->|Yes| ProcessPhotos[Process Photos:<br/>- Resize<br/>- Compress<br/>- Upload to Storage]
    ProcessPhotos --> GetURLs[Get Photo URLs]
    GetURLs --> Review[Step 6: Review & Submit]
    Review --> ConfirmSubmit{Host<br/>Confirms?}
    
    ConfirmSubmit -->|No| SelectStep{Which Step<br/>to Edit?}
    SelectStep -->|Step 1| Step1
    SelectStep -->|Step 2| Step2
    SelectStep -->|Step 3| Step3
    SelectStep -->|Step 4| Step4
    SelectStep -->|Step 5| Step5
    
    ConfirmSubmit -->|Yes| SaveProperty[Save Property to Database<br/>Status: Active]
    SaveProperty --> SavePhotos[Save Photo Records]
    SavePhotos --> SaveAmenities[Save Amenity Records]
    SaveAmenities --> Success[Display: Property Listed Successfully]
    Success --> ShowManage[Show Property Management Page]
    ShowManage --> End2([End])
```

## 7. Review Submission Flowchart

```mermaid
flowchart TD
    Start([Guest Views Property]) --> CheckBooking{Guest Has<br/>Completed Stay?}
    
    CheckBooking -->|No| HideReview[Hide Review Option]
    HideReview --> End1([End])
    
    CheckBooking -->|Yes| CheckExisting{Review<br/>Already Exists?}
    CheckExisting -->|Yes| ShowExisting[Show Existing Review<br/>With Edit Option]
    ShowExisting --> End2([End])
    
    CheckExisting -->|No| ShowForm[Display Review Form]
    ShowForm --> InputRating[Guest Selects Rating<br/>1-5 Stars]
    InputRating --> InputComment[Guest Writes Comment]
    InputComment --> ValidateComment{Comment Length<br/>50-1000 chars?}
    
    ValidateComment -->|No| ErrorComment[Display: Comment Too Short/Long]
    ErrorComment --> InputComment
    
    ValidateComment -->|Yes| OptionalPhotos[Guest Optionally<br/>Uploads Photos]
    OptionalPhotos --> PreviewReview[Show Review Preview]
    PreviewReview --> ConfirmSubmit{Confirm<br/>Submission?}
    
    ConfirmSubmit -->|No| Edit{Edit Review?}
    Edit -->|Yes| InputRating
    Edit -->|No| End3([End])
    
    ConfirmSubmit -->|Yes| SaveReview[Save Review to Database]
    SaveReview --> UpdatePropertyRating[Recalculate Property<br/>Average Rating]
    UpdatePropertyRating --> NotifyHost[Send Notification to Host]
    NotifyHost --> Success[Display: Review Submitted<br/>Thank You!]
    Success --> End4([End])
```

## Flowchart Legend

```
┌─────────┐
│ Process │  = Action/Process Step
└─────────┘

  ◇
 ╱ ╲      = Decision Point
╱   ╲
─────

 ╭───╮
 │End│    = Start/End Point
 ╰───╯

  ───▶    = Flow Direction
```

---

**Author:** Jason Rippon  
**Date:** October 26, 2025  
**Project:** ALX Airbnb Clone Documentation
