SQL QUESTION 2
SQL Question: Days Between Patient Appointments
Marks: 20 Difficulty: Medium Database: PATIENT_APPOINTMENTS_DB
Schema Details
Table Column Type Description
Patients patient_id INT (PK) Unique patient ID
Patients patient_name VARCHAR(100) Patient name
Appointments appointment_id INT (PK) Unique appointment ID
Appointments patient_id INT (FK) Patient attending
Appointments appointment_date DATE Appointment date
Appointments fee DECIMAL(10,2) Appointment fee
Problem Statement
For each appointment, calculate the number of days that have passed since the patient's immediately previous
appointment.
• Process appointments separately for each patient using patient_id.
• Use LAG() to retrieve the immediately previous appointment_date for each patient.
• Determine appointment sequence using appointment_date and then appointment_id.
• Calculate the number of days between the current and previous appointment.
• For the first appointment of each patient, days_since_previous_appointment should be NULL.
• Sort by patient_name, appointment_date, appointment_id.
Required Output Columns
• patient_name
• appointment_id
• appointment_date
• days_since_previous_appointment
Sample Data
patient_id patient_name
1 Meera
2 Rohan
3 Sahil
appointment_id patient_id appointment_date fee
801 1 2026-08-02 700.00
806 1 2026-08-08 850.00
814 1 2026-08-21 700.00
902 2 2026-08-04 600.00
909 2 2026-08-18 750.00
1001 3 2026-08-06 900.00
1008 3 2026-08-16 900.00
