# next-gen-syetemz

# Course Registration and Payment System

## Author:
Andre Spencer

## Course:
ITT103

## GitHub Public URL to Code:
[Insert GitHub Repository URL Here]

---

## Purpose of the Program:
The **Course Registration and Payment System** is a Python-based application developed as part of the ITT103 course project. It allows students to register for courses and manage their payments efficiently. The program incorporates Object-Oriented Programming (OOP) principles, modular design, and exception handling to provide a robust and user-friendly experience.

---

## How to Run the Program:
1. **Prerequisites:**
   - Ensure Python 3.8 or later is installed on your system.
   - Install any required modules specified in the `requirements.txt` file (if applicable).

2. **Steps to Run:**
   -      ```
   -copy program below and run in python console
     ```
    

3. **User Interaction:**
   - Follow the on-screen prompts to:
     - Register for courses.
     - View registered courses.
     - Process payments.

4. **System Demonstration:**
   -The program allows you to input information and see how it works
---

## Program Modifications:
1. **Version 1.0:**
   - Initial implementation of the Course Registration system.
   - Features:
     - User authentication.
     - Course selection and registration.
     - Basic payment functionality.

2. **Version 1.1:**
   - Added input validation and exception handling for improved user experience.
   - Enhanced error messages and edge case handling.

3. **Version 1.2:**
   - Introduced modular functions for better code readability and maintenance.
   - Implemented course limits (e.g., maximum number of students per course).

4. **Version 1.3:**
   - Integrated payment confirmation and receipt generation.
   - Updated user interface for enhanced interaction.

---

## Assumptions and Limitations:
1. **Assumptions:**
   - Users will enter valid inputs (e.g., no special characters in names).
   - Payments are processed as direct input (no actual payment gateway is used).

2. **Limitations:**
   - The program is designed for demonstration purposes and does not support concurrent user access.
   - No real-time database integration; data is stored in memory.

---
[Uploading Spencer.Andre-Course_Registration-ITT103-F2024.py…]()# Class to represent a Course
class Course:
    def __init__(self, course_id, name, fee):
        # Initialize course attributes
        self.course_id = course_id  # Unique ID for the course
        self.name = name  # Name of the particular course
        self.fee = fee  # Fee for the course

    def get_course_info(self):
        # Returns a string with course details
        return f"Course ID: {self.course_id}, Name: {self.name}, Fee: {self.fee}"


# Class to represent a Student
class Student:
    def __init__(self, student_id, name, email):
        # Initialize student attributes
        self.student_id = student_id  # Unique ID for the student
        self.name = name  # Name of the student
        self.email = email  # Email address of the student
        self.courses = []  # List to store enrolled courses
        self.balance = 0.0  # Balance for course payments

    def enroll(self, course):
        # Enroll the student in a course
        if course.course_id not in [c.course_id for c in self.courses]:
            self.courses.append(course)  # Add the course to the student's list
            self.balance += course.fee  # Add the course fee to the balance
            print(f"Student {self.name} has been enrolled in {course.name}.")
        else:
            print(f"Student {self.name} is already enrolled in {course.name}.")

    def get_total_fee(self):
        # Calculate and return the total fee for all enrolled courses
        return sum(course.fee for course in self.courses)


# Class to manage the registration system
class RegistrationSystem:
    def __init__(self):
        # Initialize system attributes
        self.courses = []  # List to store all courses
        self.students = {}  # Dictionary to store students with student_id as the key

    def add_course(self, course_id, name, fee):
        # Adds a new course to the system
        for course in self.courses:
            if course.course_id == course_id:  # Check for duplicate course IDs
                raise ValueError("Course ID already exists.")
        new_course = Course(course_id, name, fee)  # Create a new course object
        self.courses.append(new_course)  # Add the course to the list
        print(f"Course added successfully: {new_course.get_course_info()}")

    def register_student(self, student_id, name, email):
        # Register a new student
        if student_id in self.students:  # Check for duplicate student IDs
            raise ValueError("Student ID already exists.")
        new_student = Student(student_id, name, email)  # Create a new student object
        self.students[student_id] = new_student  # Add the student to the dictionary
        print(f"Student registered successfully: {new_student.name}")

    def enroll_in_course(self, student_id, course_id):
        # Enroll a student in a specific course
        student = self.students.get(student_id)  # Find the student by ID
        course = next((c for c in self.courses if c.course_id == course_id), None)  # Find the course by ID

        if not student:  # Check if student exists
            print("Student not found.")
            return
        if not course:  # Check if course exists
            print("Course not found.")
            return

        student.enroll(course)  # Enroll the student in the course

    def calculate_payment(self, student_id, amount_paid):
        # Process a payment for a student
        student = self.students.get(student_id)  # Find the student by ID
        if not student:  # Check if student exists
            print("Student not found.")
            return

        if amount_paid >= 0.4 * student.balance:  # Ensure at least 40% of balance is paid
            student.balance -= amount_paid  # Deduct the amount from the balance
            print(f"Payment processed. Remaining balance: ${student.balance:.2f}")
        else:
            print("Minimum payment of 40% is required to confirm registration.")

    def check_student_balance(self, student_id):
        # Display the current balance for a student
        student = self.students.get(student_id)  # Find the student by ID
        if not student:  # Check if student exists
            print("Student not found.")
            return

        print(f"Student {student.name} has a balance of ${student.balance:.2f}.")

    def show_courses(self):
        # Display all available courses
        if not self.courses:  # Check if there are any courses
            print("No courses available.")
        else:
            print("Available courses:")
            for course in self.courses:  # Loop through and display each course
                print(course.get_course_info())

    def show_registered_students(self):
        # Display all registered students
        if not self.students:  # Check if there are any students
            print("No students registered.")
        else:
            print("Registered students:")
            for student in self.students.values():  # Loop through and display each student
                print(f"ID: {student.student_id}, Name: {student.name}")

    def show_students_in_course(self, course_id):
        # Display all students enrolled in a specific course
        course = next((c for c in self.courses if c.course_id == course_id), None)  # Find the course by ID
        if not course:  # Check if course exists
            print("Course not found.")
            return

        # Find students enrolled in the course
        students_in_course = [
            student.name
            for student in self.students.values()
            if course in student.courses
        ]
        if students_in_course:  # Display students if any are enrolled
            print(f"Students enrolled in {course.name}: {', '.join(students_in_course)}")
        else:
            print(f"No students are enrolled in {course.name}.")


# Main function to interact with the system
def main():
    system = RegistrationSystem()  # Create a new instance of the RegistrationSystem

    while True:
        # Display menu options
        print("\nMenu Options:")
        print("1. Add Course")
        print("2. Register Student")
        print("3. Show Courses")
        print("4. Enroll in Course")
        print("5. Calculate Payment")
        print("6. Check Student Balance")
        print("7. Show Registered Students")
        print("8. Show Students in Course")
        print("9. Exit")

        choice = input("Enter your choice: ")  # Get user input for menu option

        if choice == "1":
            course_id = input("Enter Course ID: ")
            name = input("Enter Course Name: ")
            fee = float(input("Enter Course Fee: "))
            system.add_course(course_id, name, fee)  # Call add_course method
        elif choice == "2":
            student_id = input("Enter Student ID: ")
            name = input("Enter Student Name: ")
            email = input("Enter Student Email: ")
            system.register_student(student_id, name, email)  # Call register_student method
        elif choice == "3":
            system.show_courses()  # Call show_courses method
        elif choice == "4":
            student_id = input("Enter Student ID: ")
            course_id = input("Enter Course ID: ")
            system.enroll_in_course(student_id, course_id)  # Call enroll_in_course method
        elif choice == "5":
            student_id = input("Enter Student ID: ")
            amount = float(input("Enter Payment Amount: "))
            system.calculate_payment(student_id, amount)  # Call calculate_payment method
        elif choice == "6":
            student_id = input("Enter Student ID: ")
            system.check_student_balance(student_id)  # Call check_student_balance method
        elif choice == "7":
            system.show_registered_students()  # Call show_registered_students method
        elif choice == "8":
            course_id = input("Enter Course ID: ")
            system.show_students_in_course(course_id)  # Call show_students_in_course method
        elif choice == "9":
            print("Exiting program.")
            break  # Exit the program
        else:
            print("Invalid choice. Please try again.")  # Handle invalid input


# Entry point of the program
if __name__ == "__main__":
    main()

Thank you for reviewing the Course Registration and Payment System project! 
