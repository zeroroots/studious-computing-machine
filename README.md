# studious-computing-machine
using only C++ language for the program 
#include <iostream>
#include <vector>
using namespace std;

class Person {
protected:
    string name;
    int id;

public:
    void setPerson() {
        cout << "Enter Name: ";
        cin >> name;
        cout << "Enter ID: ";
        cin >> id;
    }

    string getName() {
        return name;
    }

    int getID() {
        return id;
    }

    virtual void display() {
        cout << "Name: " << name << endl;
        cout << "ID: " << id << endl;
    }
};

class Student : public Person {
private:
    vector<string> courses;
    vector<float> credits;
    vector<float> gpas;
    float cgpa;

public:
    void addCourses() {
        int n;
        cout << "Enter number of courses: ";
        cin >> n;

        for (int i = 0; i < n; i++) {
            string course;
            float credit, gpa;

            cout << "Course " << i + 1 << " name: ";
            cin >> course;
            courses.push_back(course);

            cout << "Credit: ";
            cin >> credit;
            credits.push_back(credit);

            cout << "GPA: ";
            cin >> gpa;
            gpas.push_back(gpa);
        }
    }

    void calculateCGPA() {
        float totalPoints = 0, totalCredits = 0;

        for (int i = 0; i < courses.size(); i++) {
            totalPoints += gpas[i] * credits[i];
            totalCredits += credits[i];
        }

        if (totalCredits == 0)
            cgpa = 0;
        else
            cgpa = totalPoints / totalCredits;
    }

    void display() override {
        cout << "\n===== Student Full Record =====\n";

        cout << "Name: " << name << endl;
        cout << "ID: " << id << endl;

        float totalCredits = 0;

        cout << "\nCourses:\n";
        for (int i = 0; i < courses.size(); i++) {
            cout << i + 1 << ". " << courses[i]
                 << " | Credit: " << credits[i]
                 << " | GPA: " << gpas[i] << endl;

            totalCredits += credits[i];
        }

        cout << "\nTotal Courses: " << courses.size() << endl;
        cout << "Total Credits: " << totalCredits << endl;
        cout << "Final CGPA: " << cgpa << endl;
    }
};

class University {
private:
    vector<Student> students;

public:
    void addStudent() {
        Student s;
        s.setPerson();
        s.addCourses();
        s.calculateCGPA();
        students.push_back(s);
    }

    void showStudents() {
        if (students.empty()) {
            cout << "No student data found!\n";
            return;
        }

        for (int i = 0; i < students.size(); i++) {
            cout << "\n>>> Student " << i + 1 << " <<<";
            students[i].display();
        }
    }

    void searchByID(int id) {
        bool found = false;

        for (int i = 0; i < students.size(); i++) {
            if (students[i].getID() == id) {
                cout << "\nStudent Found:\n";
                students[i].display();
                found = true;
                break;
            }
        }

        if (!found) {
            cout << "Student not found!\n";
        }
    }

    void searchByName(string name) {
        bool found = false;

        for (int i = 0; i < students.size(); i++) {
            if (students[i].getName() == name) {
                cout << "\nStudent Found:\n";
                students[i].display();
                found = true;
            }
        }

        if (!found) {
            cout << "Student not found!\n";
        }
    }

    void menu() {
        int choice;

        while (true) {
            cout << "\n CANADIAN UNIVERSITY MANAGEMENT SYSTEM \n";
            cout << "1. Add Student\n";
            cout << "2. Show All Students\n";
            cout << "3. Search by ID\n";
            cout << "4. Search by Name\n";
            cout << "5. Exit\n";
            cout << "Enter choice: ";
            cin >> choice;

            if (choice == 1) {
                addStudent();
            } 
            else if (choice == 2) {
                showStudents();
            } 
            else if (choice == 3) {
                int id;
                cout << "Enter ID: ";
                cin >> id;
                searchByID(id);
            } 
            else if (choice == 4) {
                string name;
                cout << "Enter Name: ";
                cin >> name;
                searchByName(name);
            } 
            else if (choice == 5) {
                cout << "Exiting...\n";
                break;
            } 
            else {
                cout << "Invalid choice!\n";
            }
        }
    }
};

int main() {
    University u;
    u.menu();
    return 0;
}
