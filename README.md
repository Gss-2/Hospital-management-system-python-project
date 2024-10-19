# Hospital-management-system-python-project
HMS using oops and exception handling in python
# Defining the Custom Exceptions for error handling
class HospitalError(Exception):
    """Base class for exceptions in the hospital management system."""
    pass

class DoctorNotFoundError(HospitalError):
    """Raised when the specified doctor is not found."""
    def __init__(self, doctor_id):
        self.message = f"Doctor with ID {doctor_id} not found."
        # This super key word is used to inherit the message of parent class
        super().__init__(self.message)

class PatientNotFoundError(HospitalError):
    """Raised when the specified patient is not found."""
    def __init__(self, patient_id):
        self.message = f"Patient with ID {patient_id} not found."
        # This super key word is used to inherit the message of parent class
        super().__init__(self.message)

class AppointmentError(HospitalError):
    """Raised for errors related to appointments."""
    pass


# Defining the Doctor Class
class Doctor:
    def __init__(self, doctor_id, name, specialization):
        self.doctor_id = doctor_id
        self.name = name
        self.specialization = specialization
    # This method is used when we need to represent an object
    def __repr__(self):
        return f"Doctor({self.doctor_id}, {self.name}, {self.specialization})"


# Defining the Patient Class
class Patient:
    def __init__(self, patient_id, name, age, disease):
        self.patient_id = patient_id
        self.name = name
        self.age = age
        self.disease = disease

    def __repr__(self):
        return f"Patient({self.patient_id}, {self.name}, {self.age}, {self.disease})"


# Defining the Appointment Class
class Appointment:
    def __init__(self, appointment_id, doctor, patient, date):
        self.appointment_id = appointment_id
        self.doctor = doctor
        self.patient = patient
        self.date = date

    def __repr__(self):
        return f"Appointment({self.appointment_id}, Doctor: {self.doctor.name}, Patient: {self.patient.name}, Date: {self.date})"


# Defining Hospital Management System Class
class HospitalManagementSystem:
    def __init__(self):
        # Here I have used the empty dictionaries to add and store the data of the doctors, patients, and their appointments
        self.doctors = {}
        self.patients = {}
        self.appointments = {}
        
    #This method is used to add the doctors in the hospital or we can say here in empty dictionaries
    def add_doctor(self, doctor):
        self.doctors[doctor.doctor_id] = doctor
        
    #This method is used to add the patient in the hospital or we can say here in empty dictionaries
    def add_patient(self, patient):
        self.patients[patient.patient_id] = patient
        
    # Here I have used this method to schedule the appointment of the patient with the doctors
    def schedule_appointment(self, appointment):
        if appointment.doctor.doctor_id not in self.doctors:
            raise DoctorNotFoundError(appointment.doctor.doctor_id)
        
        if appointment.patient.patient_id not in self.patients:
            raise PatientNotFoundError(appointment.patient.patient_id)
        
        self.appointments[appointment.appointment_id] = appointment
        
    #This method is used to fetch the details of the doctor through his/her doctor id
    def find_doctor_by_id(self, doctor_id):
        if doctor_id not in self.doctors:
            raise DoctorNotFoundError(doctor_id)
        return self.doctors[doctor_id]
        
    #This method is used to fetch the details of the patient through his/her patient id
    def find_patient_by_id(self, patient_id):
        if patient_id not in self.patients:
            raise PatientNotFoundError(patient_id)
        return self.patients[patient_id]
        
    # This method will return all the valid appointment in a list
    def view_appointments(self):
        if not self.appointments:
            return "No appointments found."
        return list(self.appointments.values())



# Creating an instance hms of the hospital management system 
hms = HospitalManagementSystem()

# Adding the Doctors in the hospital 
doc1 = Doctor(1, "Dr. Rajendra sharma", "Cardiologist")
doc2 = Doctor(2, "Dr. Aditi", "Neurologist")
hms.add_doctor(doc1)
hms.add_doctor(doc2)

#Adding the patients in the hospital
pat1 = Patient(1, "Atul", 25, "Heart Disease")
pat2 = Patient(2, "Pradeep jii", 50, "Migraine")
hms.add_patient(pat1)
hms.add_patient(pat2)

# Scheduling the Appointments of the patients with the corresponding doctors

# Scheduling the Appointment 1
try:
    app1 = Appointment(1, doc1, pat1, "2024-10-14")
    hms.schedule_appointment(app1)
except HospitalError as e:
    print(e)

# Scheduling anathor Appointment 2
try:
    app2 = Appointment(2, doc2, pat2, "2024-10-18")
    hms.schedule_appointment(app2)
except HospitalError as e:
    print(e)

# Scheduling an invalid appointment (Doctor not found)
try:
    doc3 = Doctor(3, "Dr. DK tyagi", "Dermatologist")  # Not added to system
    app3 = Appointment(2, doc3, pat1, "2024-10-15")
    hms.schedule_appointment(app2)
except HospitalError as e:
    print(e)

# View all Appointments
print("Appointment details of the patient with the doctors :")
print("--"*50)
print(hms.view_appointments())

# finding the details of the doctor and the patient by their ids
print("___"*50)
print("Details of the doctors are given below : ")
print("--"*20)
print(hms.find_doctor_by_id(1))
print(hms.find_doctor_by_id(2))
print(" ")

print("Details of the patients are given below : ")
print("--"*20)
print(hms.find_patient_by_id(1))
print(hms.find_patient_by_id(2))
