# Flowcharts

This directory contains flowcharts for key backend processes of the Airbnb Clone project.

## Property Booking Flow
The flowchart (`data-flow-diagram.png`) visualizes the end-to-end process for booking a property:

1. Guest logs in and searches for listings.
2. System checks property availability.
3. Guest selects property and enters booking details.
4. Payment is processed.
5. On success, booking is confirmed, database is updated, and notifications are sent.
6. On failure, booking is canceled and guest is notified.
