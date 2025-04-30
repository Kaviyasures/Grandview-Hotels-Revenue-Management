Grandview Hotel Revenue Management Dashboard Using Power BI
---
This Power BI project aims to optimize revenue management for Grandview Hotels by analyzing key booking data. The interactive dashboard provides insights into booking performance, customer ratings, cancellations, occupancy trends, and revenue generation. It helps hotel management make data-driven decisions to improve operations and customer satisfaction.

## Key Metrics & DAX Measures 

Customer Ratings:

rating = COALESCE(fact_bookings[ratings_given], AVERAGE(fact_bookings[ratings_given]))

average rating = AVERAGE(fact_bookings[rating])

Star Rating (Visual Display):

average rating star rating = 
VAR __MAX_NUMBER_OF_STARS = 5
VAR __MIN_RATED_VALUE = 1
VAR __MAX_RATED_VALUE = 5
VAR __BASE_VALUE = [average rating]
VAR __NORMALIZED_BASE_VALUE =
    MIN(
        MAX(
            DIVIDE(__BASE_VALUE - __MIN_RATED_VALUE, __MAX_RATED_VALUE - __MIN_RATED_VALUE),
            0
        ),
        1
    )
VAR _STAR_RATING = ROUND(_NORMALIZED_BASE_VALUE * __MAX_NUMBER_OF_STARS, 0)
RETURN
    IF(
        NOT ISBLANK(__BASE_VALUE),
        REPT(UNICHAR(9733), _STAR_RATING) &
        REPT(UNICHAR(9734), __MAX_NUMBER_OF_STARS - _STAR_RATING)
    )
    
Revenue:

revenue = SUM(fact_bookings[revenue_realized])

Month Formatting:

Month = FORMAT('dim_date (2) (1)'[date], "mmmm yy")

Cancellation Metrics:

total booking = COUNT(fact_bookings[booking_id])

total cancelled booking = 
    CALCULATE(COUNT(fact_bookings[booking_status]), fact_bookings[booking_status] = "Cancelled")
    
cancellation rate = 
    DIVIDE([total cancelled booking], [total booking])
    
Occupancy Metrics:

total capacity = SUM(fact_aggregated_bookings[capacity])

occupancy % = DIVIDE([total booking], [total capacity])

Successful Bookings:

total successful booking = [total booking] - [total cancelled booking]

Weekday vs Weekend Indicator:

IsWeekend = 
IF(
    WEEKDAY(fact_bookings[check_in_date], 2) > 5,
    "Weekend",
    "Weekday"
)

## Dashboard Features:

Slicers to filter by branch, booking status, and date range.

Card Visuals for KPIs like revenue, bookings, occupancy %, cancellation rate.

Bar/Column Charts showing bookings by booking platforms and property.

Gauge Visuals for average star rating.

Pie Chart to show booking revenue across city and category.

## Tools & Technologies:
Power BI Desktop

DAX (Data Analysis Expressions)

Data modeling & data transformation

