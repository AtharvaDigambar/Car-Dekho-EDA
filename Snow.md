WITH ordered_appointments AS (
SELECT
p.patient_name,
a.appointment_id,
a.appointment_date,
LAG(a.appointment_date) OVER (
PARTITION BY a.patient_id
ORDER BY a.appointment_date, a.appointment_id
) AS previous_appointment_date
FROM Appointments AS a
JOIN Patients AS p
ON a.patient_id = p.patient_id
)
SELECT
patient_name,
appointment_id,
appointment_date,
DATEDIFF(
appointment_date,
previous_appointment_date
) AS days_since_previous_appointment
FROM ordered_appointments
ORDER BY
patient_name,
appointment_date,
appointment_id;
