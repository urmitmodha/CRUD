import sqlite3


conn = sqlite3.connect("students.db")
cursor = conn.cursor()


cursor.execute("""
CREATE TABLE IF NOT EXISTS students (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    name TEXT NOT NULL,
    course TEXT NOT NULL,
    marks REAL
)
""")

conn.commit()



def add_student():
    name = input("Enter student name: ")
    course = input("Enter course: ")
    marks = float(input("Enter marks: "))

    cursor.execute(
        "INSERT INTO students (name, course, marks) VALUES (?, ?, ?)",
        (name, course, marks)
    )

    conn.commit()
    print("Student added successfully!")



def view_students():
    cursor.execute("SELECT * FROM students")
    students = cursor.fetchall()

    if not students:
        print("No students found.")
    else:
        for student in students:
            print(student)



def search_student():
    student_id = int(input("Enter student ID: "))

    cursor.execute(
        "SELECT * FROM students WHERE id = ?",
        (student_id,)
    )

    student = cursor.fetchone()

    if student:
        print(student)
    else:
        print("Student not found.")



def delete_student():
    student_id = int(input("Enter student ID: "))

    cursor.execute(
        "DELETE FROM students WHERE id = ?",
        (student_id,)
    )

    conn.commit()
    print("Student deleted successfully!")



while True:
    print("\n===== Student Management System =====")
    print("1. Add Student")
    print("2. View Students")
    print("3. Search Student")
    print("4. Delete Student")
    print("5. Exit")

    choice = input("Enter your choice: ")

    if choice == "1":
        add_student()

    elif choice == "2":
        view_students()

    elif choice == "3":
        search_student()

    elif choice == "4":
        delete_student()

    elif choice == "5":
        print("Thank you!")
        break

    else:
        print("Invalid choice!")


conn.close()
