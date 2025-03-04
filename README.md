# assgn1
#include <iostream>
#include <vector>
#include <memory> // For smart pointers

using namespace std;

// Base Class: Employee
class Employee {
protected:
    string name;
    int employeeID;
    double salary;

public:
    // Constructor
    Employee(string empName, int empID, double empSalary)
        : name(empName), employeeID(empID), salary(empSalary) {}

    // Virtual method for polymorphism
    virtual void displayDetails() const {
        cout << "\nEmployee ID: " << employeeID << endl;
        cout << "Name: " << name << endl;
        cout << "Salary: $" << salary << endl;
    }

    // Getter for Employee ID
    int getEmployeeID() const {
        return employeeID;
    }

    // Virtual destructor
    virtual ~Employee() {}
};

// Derived Class: Manager (inherits from Employee)
class Manager : public Employee {
private:
    string department;
    double bonus;

public:
    // Constructor
    Manager(string empName, int empID, double empSalary, string dept, double empBonus)
        : Employee(empName, empID, empSalary), department(dept), bonus(empBonus) {}

    // Override display method
    void displayDetails() const override {
        Employee::displayDetails();
        cout << "Department: " << department << endl;
        cout << "Bonus: $" << bonus << endl;
    }
};

// Derived Class: Engineer (inherits from Employee)
class Engineer : public Employee {
private:
    string specialization;
    string projectAssigned;

public:
    // Constructor
    Engineer(string empName, int empID, double empSalary, string spec, string project)
        : Employee(empName, empID, empSalary), specialization(spec), projectAssigned(project) {}

    // Override display method
    void displayDetails() const override {
        Employee::displayDetails();
        cout << "Specialization: " << specialization << endl;
        cout << "Project Assigned: " << projectAssigned << endl;
    }
};

// Employee Management System Class
class EmployeeManagementSystem {
private:
    vector<shared_ptr<Employee>> employees; // Stores Employee objects dynamically

public:
    // Method to add Employee dynamically
    void addEmployee() {
        int choice;
        string name;
        int id;
        double salary;

        cout << "\nEnter Employee Name: ";
        cin.ignore();
        getline(cin, name);
        cout << "Enter Employee ID: ";
        cin >> id;
        cout << "Enter Salary: $";
        cin >> salary;

        cout << "Choose Employee Type:\n";
        cout << "1. Manager\n2. Engineer\n";
        cout << "Enter your choice: ";
        cin >> choice;

        if (choice == 1) {
            string department;
            double bonus;
            cout << "Enter Department: ";
            cin.ignore();
            getline(cin, department);
            cout << "Enter Bonus: $";
            cin >> bonus;
            employees.push_back(make_shared<Manager>(name, id, salary, department, bonus));
        }
        else if (choice == 2) {
            string specialization, project;
            cout << "Enter Specialization: ";
            cin.ignore();
            getline(cin, specialization);
            cout << "Enter Project Assigned: ";
            getline(cin, project);
            employees.push_back(make_shared<Engineer>(name, id, salary, specialization, project));
        }
        else {
            cout << "Invalid choice! Employee not added.\n";
            return;
        }

        cout << "Employee added successfully!\n";
    }

    // Display all Employees
    void displayAllEmployees() const {
        if (employees.empty()) {
            cout << "\nNo employees in the system.\n";
            return;
        }
        for (const auto &emp : employees) {
            cout << "--------------------------";
            emp->displayDetails();
        }
    }

    // Search Employee by ID
    void searchEmployeeByID() const {
        int id;
        cout << "\nEnter Employee ID to search: ";
        cin >> id;

        for (const auto &emp : employees) {
            if (emp->getEmployeeID() == id) {
                cout << "Employee found:";
                emp->displayDetails();
                return;
            }
        }
        cout << "Employee with ID " << id << " not found.\n";
    }
};

// Main Function
int main() {
    EmployeeManagementSystem ems;
    int choice;

    while (true) {
        cout << "\n======= Employee Management System =======\n";
        cout << "1. Add Employee\n2. Display All Employees\n3. Search Employee by ID\n4. Exit\n";
        cout << "Enter your choice: ";
        cin >> choice;

        switch (choice) {
            case 1:
                ems.addEmployee();
                break;
            case 2:
                ems.displayAllEmployees();
                break;
            case 3:
                ems.searchEmployeeByID();
                break;
            case 4:
                cout << "Exiting program...\n";
                return 0;
            default:
                cout << "Invalid choice! Please try again.\n";
        }
    }

    return 0;
}
