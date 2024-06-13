# HealthTech---GlowRex-Company

Introduction
The primary goal of this project is to analyze the user journey within our skincare app, from the initial signup to the first appointment and first prescription order. By understanding these user interactions, the final report aims to provide insights that will help the company enhance user engagement, optimize service delivery, and increase overall revenue. This report is crucial for identifying bottlenecks in the user journey, understanding user behavior patterns, and making data-driven decisions to improve the user experience and operational efficiency.

Methodology
Data Collection
Data was extracted from multiple tables in the company's database, including user signup data, appointment records, and prescription orders. The primary tables used were:

extracted_users: Contains information on user signups.
extracted_appointments: Contains appointment details.
extracted_orders: Contains prescription order details.
Data Analysis
The analysis was structured in several steps:

User Signups: Identify the date and week of user signups.
First Completed Appointment: Determine the first completed appointment for each user.
First Prescription Order: Identify the first prescription order for each user.
Combining Insights: Merge the above datasets to create a comprehensive view of the user journey.
Rationale
The final report structure was designed to provide a clear and comprehensive view of the user journey:

By aggregating data on a weekly and monthly basis, we can spot trends and operational delays early.
Detailed metrics such as average time to key actions and percentages of users completing each step help in pinpointing areas for improvement.
Analysis Details
User Signups
-- Extract user signups
SELECT user_id, sign_up_date, TRUNC(sign_up_date, 'IW') AS sign_up_week
FROM users;
Logic: Extract the date and week of user signups to analyze signup trends over time.

Insights: Helps in understanding when users are most likely to sign up and identifying any seasonal trends.

First Completed Appointment
-- Determine the first completed appointment for each user
WITH first_appointments AS (
  SELECT user_id, MIN(appointment_date) AS first_appointment_date
  FROM appointments
  WHERE status = 'completed'
  GROUP BY user_id
)
Logic: Identify the first completed appointment for each user to understand the time it takes from signup to the first appointment.

Insights: Provides insights into user engagement and the efficiency of appointment scheduling.

First Prescription Order
-- Determine the first prescription order for each user
WITH first_orders AS (
  SELECT user_id, MIN(order_date) AS first_order_date
  FROM orders
  GROUP BY user_id
)
Logic: Identify the first prescription order for each user to understand the conversion from appointment to purchase.

Insights: Helps in understanding the effectiveness of appointments in driving sales.

Combining Insights
-- Combine user, appointment, and order data
WITH user_journey AS (
  SELECT u.user_id, u.sign_up_date, u.sign_up_week,
         a.first_appointment_date,
         DATEDIFF(a.first_appointment_date, u.sign_up_date) AS first_appointment_tat,
         o.first_order_date,
         DATEDIFF(o.first_order_date, a.first_appointment_date) AS first_rx_order_tat
  FROM users u
  LEFT JOIN first_appointments a ON u.user_id = a.user_id
  LEFT JOIN first_orders o ON u.user_id = o.user_id
)
SELECT * FROM user_journey;
Logic: Merge data from user signups, first appointments, and first orders to create a comprehensive view of the user journey.

Insights: Provides a holistic view of the user journey, identifying key touchpoints and potential drop-offs.

Results
The analysis revealed several key metrics and insights:

Average Time to First Appointment: The average number of days from signup to the first completed appointment.
Average Time to First Rx Order: The average number of days from the first appointment to the first prescription order.
Percentage of Users with Appointments: The proportion of users who booked at least one appointment.
Percentage of Users with Rx Orders: The proportion of users who placed at least one prescription order.
Average First Order Value: The average value of the first prescription order.
Notable trends observed included seasonal variations in user signups and appointment bookings, as well as varying conversion rates from appointments to prescription orders.

Conclusion and Recommendations
Based on the analysis, several conclusions and recommendations can be made:

Improve Appointment Scheduling: The time to the first appointment can be reduced by optimizing appointment scheduling processes.
Enhance User Engagement: Personalized follow-ups and reminders could increase the percentage of users completing appointments and placing orders.
Increase Conversion Rates: Offering incentives for first-time prescription orders could improve conversion rates from appointments to purchases.
Monitor and Address Bottlenecks: Regular monitoring of the user journey metrics will help in identifying and addressing bottlenecks promptly.

![SQL II_1](https://github.com/Swetha-Maadugula/HealthTech---GlowRex-Company/assets/168377003/b42bfbf4-a55f-4916-8d1a-0c5f2e1ac5d5)
![SQL II_2](https://github.com/Swetha-Maadugula/HealthTech---GlowRex-Company/assets/168377003/b739f81b-3fbe-48f2-a2fc-6272d4459af2)
![SQL II_3](https://github.com/Swetha-Maadugula/HealthTech---GlowRex-Company/assets/168377003/2b6b1395-329f-4fec-838a-e5a6e68a1b84)
![SQL II_4](https://github.com/Swetha-Maadugula/HealthTech---GlowRex-Company/assets/168377003/f302d1c4-07c0-450b-851c-46ea9f238b08)

