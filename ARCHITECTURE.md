# Architecture
# Project Overview

    This document explains the overall structure of the Workspace Booking System:
        how the data is organized, how booking conflicts are checked, how pricing is calculated, and a few ideas on how the system could scale later.
    I also added a note on where (if anywhere) AI can be useful in the future.

# Data Model
    # Room
    roomId (string)
    name (string)
    basePricePerHour (number)
    capacity (number)
    Booking
    bookingId
    roomId
    userName
    startTime
    endTime
    totalHours
    totalAmount
    status (confirmed / cancelled)

    Time fields are stored in ISO format to avoid timezone issues.

# Booking Conflict Logic

    A new booking is rejected if it overlaps with an existing one for the same room.
    The check works like this:
    (startA < endB) AND (endA > startB)
    # Where:

        A = new booking
        B = existing booking

        Even partial overlap (e.g., 10:00–11:00 overlaps 10:45–11:15) is considered invalid.
        This is simple but reliable since all comparisons are done using ISO timestamps.

# Pricing Method

    The system charges per hour based on two ranges:
    Peak hours (9 AM–5 PM): 250/hr
    Off-peak hours: 100/hr
    The calculation is done hour by hour.
    If a booking spans both peak and off-peak, the price is split accordingly.

    Example:
    8–10 AM → 1 hr off-peak + 1 hr peak
    Total = 100 + 250
    The system rounds hours to 0.5 increments.

# Cancellation Rule

    A booking can be cancelled only if:
    currentTime <= startTime - 2 hours
    Otherwise the system returns a simple error message.

# Scaling Ideas

    If the system grows and more users start booking at the same time, the following can help:

    1. Database Indexing

        Index on roomId
        Index on startTime
        Index on endTime
        This speeds up conflict checks.

    2. Queue-Based Booking

        For very high traffic, bookings can be processed through a queue (e.g., Redis) to avoid race conditions.

    3. Caching Frequently Used Data

        Room list
        Pricing rules
        Analytics summaries
        Redis or a simple in-memory cache works fine.

    4. Microservice Option (Later)

        Booking service
        Pricing service
        Availability service
        Analytics service
        Not needed now, but possible.
