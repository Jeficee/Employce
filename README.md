#include <iostream>

using namespace std;

const int MAX_SIZE = 10;

struct Student {
    int id;
    string name;
    float grade;
};

// Display a single student
void displayStudent(const Student& s) {
    cout << "ID: " << s.id << "\n";
    cout << "Name: " << s.name << "\n";
    cout << "Grade: " << s.grade << "\n";
    cout << "------------------------\n";
}

// Display all students
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

// Add a student if space is available
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

// Bubble sort by ID
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

// Linear search
Student* linearSearch(Student students[], int size, int targetId) {
    for (int i = 0; i < size; ++i) {
        if (students[i].id == targetId) {
            return &students[i];
        }
    }
    return nullptr;
}

// Binary search
Student* binarySearch(Student students[], int size, int targetId) {
    int left = 0, right = size - 1;
    while (left <= right) {
        int mid = (left + right) / 2;
        if (students[mid].id == targetId) {
            return &students[mid];
        } else if (students[mid].id < targetId) {
            left = mid + 1;
        } else {
            right = mid - 1;
        }
    }
    return nullptr;
}

// Main
int main() {
    Student students[MAX_SIZE] = {
        {102, "Alice", 90.0},
        {101, "Bob", 85.5},
        {104, "Charlie", 78.0},
        {103, "Diana", 92.0},
        {105, "Ethan", 88.5}
    };
    int currentSize = 5;
    int choice;

    do {
        cout << "\n=== Student Record Search System ===\n";
        cout << "1. Linear Search (Unsorted)\n";
        cout << "2. Binary Search (Sorted)\n";
        cout << "3. Display All Students\n";
        cout << "4. Add Student\n";
        cout << "5. Exit\n";
        cout << "Enter your choice: ";
        cin >> choice;

        if (choice == 1 || choice == 2) {
            int searchId;
            cout << "Enter Student ID to search: ";
            cin >> searchId;

            Student* result = nullptr;

            if (choice == 1) {
                result = linearSearch(students, currentSize, searchId);
            } else {
                bubbleSort(students, currentSize); // Sort before binary search
                result = binarySearch(students, currentSize, searchId);
            }

            if (result) {
                cout << "\nStudent Found:\n";
                displayStudent(*result);
            } else {
                cout << "Student not found.\n";
            }

        } else if (choice == 3) {
            displayAllStudents(students, currentSize);
        } else if (choice == 4) {
            addStudent(students, currentSize);
        } else if (choice == 5) {
            cout << "Exiting program.\n";
        } else {
            cout << "Invalid choice. Please try again.\n";
        }

    } while (choice != 5);

    return 0;
}