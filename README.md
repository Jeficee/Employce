#include <iostream>
#include <string>

using namespace std;

const int MAX_SIZE = 10;

struct Student {
    int id;
    string name;
    float grade;
};

void displayStudent(const Student& s) {
    cout << "ID: " << s.id << "\n";
    cout << "Name: " << s.name << "\n";
    cout << "Grade: " << s.grade << "\n";
    cout << "------------------------\n";
}

void displayAllStudents(Student students[], int size) {
    if (size == 0) {
        cout << "No student records available.\n";
        return;
    }
    cout << "\nAll Students:\n";
    cout << "------------------------\n";
    for (int i = 0; i < size; ++i) {
        displayStudent(students[i]);
    }
}

void addStudent(Student students[], int& size) {
    if (size >= MAX_SIZE) {
        cout << "Student list is full. Cannot add more students.\n";
        return;
    }

    Student s;
    cout << "\nEnter new student details:\n";
    cout << "Student ID: ";
    cin >> s.id;
    cin.ignore();
    cout << "Name: ";
    getline(cin, s.name);
    cout << "Grade: ";
    cin >> s.grade;

    students[size] = s;
    size++;
    cout << "Student added successfully!\n";
}

void bubbleSort(Student students[], int size) {
    for (int i = 0; i < size - 1; ++i) {
        for (int j = 0; j < size - i - 1; ++j) {
            if (students[j].id > students[j + 1].id) {
                Student temp = students[j];
                students[j] = students[j + 1];
                students[j + 1] = temp;
            }
        }
    }
}

Student* linearSearch(Student students[], int size, int targetId) {
    for (int i = 0; i < size; ++i) {
        if (students[i].id == targetId) {
            return &students[i];
        }
    }
    return nullptr;
}

Student* binarySearch(Student students[], int size, int targetId) {
    int left = 0, right = size - 1;
    while (left <= right) {
        int mid = (left + right) / 2;
        if (students[mid].id == targetId) {
            return &students[mid];
        }
        else if (students[mid].id < targetId) {
            left = mid + 1;
        }
        else {
            right = mid - 1;
        }
    }
    return nullptr;
}

int main() {
    Student students[MAX_SIZE]; // empty array
    int currentSize = 0;        // no initial records
    int choice;

    do {
        cout << "\n=== Student Record Search System ===\n";
        cout << "1. Add Student\n";
        cout << "2. Sequential Search\n";
        cout << "3. Binary Search\n";
        cout << "4. Display All Students\n";
        cout << "5. Exit\n";
        cout << "Enter your choice: ";
        cin >> choice;

        if (choice == 1) {
            addStudent(students, currentSize);
        }
        else if (choice == 2 || choice == 3) {
            if (currentSize == 0) {
                cout << "No students available. Please add students first.\n";
                continue;
            }

            int searchId;
            cout << "Enter Student ID to search: ";
            cin >> searchId;

            Student* result = nullptr;

            if (choice == 2) {
                result = linearSearch(students, currentSize, searchId);
            }
            else {
                bubbleSort(students, currentSize);
                result = binarySearch(students, currentSize, searchId);
            }

            if (result) {
                cout << "\nStudent Found:\n";
                displayStudent(*result);
            }
            else {
                cout << "Student not found.\n";
            }
        }
        else if (choice == 4) {
            displayAllStudents(students, currentSize);
        }
        else if (choice == 5) {
            cout << "Exiting program.\n";
        }
        else {
            cout << "Invalid choice. Please try again.\n";
        }

    } while (choice != 5);

    return 0;
}