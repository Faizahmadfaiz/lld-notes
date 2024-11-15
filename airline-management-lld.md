Here’s the entire note in markdown format:

# Airline Management System - Low-Level Design Notes

## 1. Key Functional Requirements
### Admin Actions:
- Add/update/remove aircraft, flights, and crew.
- Configure seat categories and pricing.
- Add/update/remove flight schedules.

### Customer Actions:
- Search for flights by source, destination, and date.
- Reserve seats and add passengers to the booking.
- Make payments and confirm bookings.
- Receive notifications (e.g., booking confirmation, flight delays).

### Other Functionalities:
- Crew can view assigned flight schedules.
- Handle flight delays and cancellations, updating impacted flights.
- Time-bound reservations (expire unpaid bookings and release seats).

---

## 2. Core Entities and Responsibilities
### **Aircraft**
- Tracks aircraft details like name, capacity, and engine type.
- Linked to airports and flights.

### **Airport**
- Represents source and destination locations for flights.
- Linked to flight schedules and aircraft.

### **Flight Schedule**
- Defines recurring flight schedules (e.g., daily, weekly).
- Contains details like source, destination, and scheduled departure/arrival times.

### **Flight Instance**
- Represents a specific instance of a flight schedule on a particular date.
- Tracks actual departure/arrival times, delays, and status (e.g., ON_TIME, DELAYED, CANCELLED).

### **Flight Seat**
- Tracks individual seats for flight instances.
- Stores availability status, price, and category.

### **Customer**
- Represents users making bookings.
- Linked to bookings and payment methods.

### **Booking**
- Represents a customer's reservation.
- Tracks associated passengers, selected seats, and special services.
- **Statuses**: `PENDING`, `CONFIRMED`, `EXPIRED`.

### **Booking Passenger**
- Stores passenger details for each booking (e.g., name, age, passport number).

### **Payment**
- Tracks payment status (`SUCCESS`, `FAILED`, `PENDING`), amount, and method.

### **Special Service**
- Defines additional services like extra baggage or priority check-in.
- Linked to bookings as `BookingSpecialService`.

---

## 3. Key Design Patterns
### **Strategy Pattern (Payment Processing)**
- Encapsulates payment methods like UPI, card, or wallet.

**Example:**
```java
PaymentStrategy paymentStrategy = PaymentStrategyFactory.getPaymentStrategy(paymentDetails.getPaymentMethod());
paymentStrategy.processPayment(paymentDetails);

Observer Pattern (Notifications)

	•	Sends booking or flight delay notifications to customers and crew.

Builder Pattern (Complex Object Creation)

	•	Simplifies creation of Booking with passengers, seats, and services.

Factory Method Pattern (Seat Creation)

	•	Dynamically creates seat objects based on categories (e.g., Economy, Business).

Singleton Pattern (Shared Services)

	•	Ensures single instances of shared services like NotificationService.

4. Database Design Highlights

Relationships:

	•	FLIGHT_SCHEDULE ↔ FLIGHT_INSTANCE: 1-to-Many.
	•	FLIGHT_INSTANCE ↔ FLIGHT_SEAT: 1-to-Many.
	•	BOOKING ↔ BOOKING_PASSENGER: 1-to-Many.
	•	BOOKING ↔ PAYMENT: 1-to-1.

Optimistic Locking for Seat Reservation:

	•	Adds a version column in the FLIGHT_SEAT table.
	•	Prevents race conditions when multiple users attempt to reserve the same seat.

Important Tables:

	•	FLIGHT_SCHEDULE: Stores recurring schedules.
	•	FLIGHT_INSTANCE: Tracks actual flight occurrences.
	•	FLIGHT_SEAT: Tracks seat availability and pricing.
	•	BOOKING: Tracks reservations with statuses like PENDING, CONFIRMED, and EXPIRED.

5. Workflow Highlights

Search for Flights:

	•	Query FLIGHT_SCHEDULE and FLIGHT_INSTANCE based on source, destination, and date.
	•	Optimize with indexing and caching.

Reserve Seats:

	•	Check availability in FLIGHT_SEAT and reserve seats using optimistic locking.
	•	Update booking status to PENDING.

Payment Processing:

	•	Use a strategy pattern for payment methods.
	•	Handle payment failures by allowing retries within a reservation window.

Expire Unpaid Bookings:

	•	Use a background job to check for expired bookings and release seats.

Notifications:

	•	Notify customers and crew about booking confirmations, delays, and cancellations.

6. Handling Edge Cases

Multiple Users Reserving the Same Seat:

	•	Resolved using optimistic locking (version column in FLIGHT_SEAT).

Payment Gateway Failures:

	•	Allow retries within the reservation window.
	•	If retries fail, mark the booking as EXPIRED.

Flight Delays Impacting Other Flights:

	•	Update FLIGHT_INSTANCE for impacted flights.
	•	Notify customers of updated schedules.

Reservation Expiry:

	•	Automatically release seats and mark bookings as EXPIRED if not paid within the allowed time.

7. Security Considerations

Payment Webhooks:

	•	Validate payload signatures from the payment gateway.
	•	Use HTTPS to secure webhook communication.

Seat Reservation:

	•	Prevent overbooking using database locks or optimistic locking.

Notifications:

	•	Throttle and queue notifications to avoid overwhelming systems during high activity.

8. Non-Functional Considerations

Scalability:

	•	Cache flight searches to handle high traffic.
	•	Partition data by airports or regions for distributed database setups.

Fault Tolerance:

	•	Use retries and dead-letter queues for failed payment or notification events.

Performance:

	•	Use indexes on frequently queried fields like source, destination, and date.
	•	Optimize queries with projections to avoid fetching unnecessary data.

