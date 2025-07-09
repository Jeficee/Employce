#include <iostream>
#include <string>

using namespace std;

const int MAX_SIZE = 10; // Maximum number of students allowed

// Structure to represent a student
struct Student {
    int id;         // Student ID number
    string name;    // Student name
    float grade;    // Student grade
};

// Function to display a single student's details
// Takes a Student struct as parameter and prints its fields
void displayStudent(const Student& s) {
    cout << "ID: " << s.id << "\n";
    cout << "Name: " << s.name << "\n";
    cout << "Grade: " << s.grade << "\n";
    cout << "------------------------\n";
}

// Function to display all students in the array
// Iterates from 0 to current size and calls displayStudent() for each
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

// Function to add a new student to the array
// Prompts user input and inserts the new student at the end of the array
void addStudent(Student students[], int& size) {
    if (size >= MAX_SIZE) {
        cout << "Student list is full. Cannot add more students.\n";
        return;
    }

    Student s;
    cout << "\nEnter new student details:\n";
    cout << "Student ID: ";
    cin >> s.id;
    cin.ignore(); // Clear the newline character left in the input buffer
    cout << "Name: ";
    getline(cin, s.name); // Read full name with spaces
    cout << "Grade: ";
    cin >> s.grade;

    students[size] = s; // Add new student at index `size`
    size++;             // Increment current size
    cout << "Student added successfully!\n";
}

// Function to sort students by ID using Bubble Sort algorithm
// This is required before performing binary search
void bubbleSort(Student students[], int size) {
    for (int i = 0; i < size - 1; ++i) {
        for (int j = 0; j < size - i - 1; ++j) {
            if (students[j].id > students[j + 1].id) {
                // Swap students if out of order
                Student temp = students[j];
                students[j] = students[j + 1];
                students[j + 1] = temp;
            }
        }
    }
}

// Function to search for a student using linear (sequential) search
// Checks each element one by one until a match is found or end is reached
Student* linearSearch(Student students[], int size, int targetId) {
    for (int i = 0; i < size; ++i) {
        if (students[i].id == targetId) {
            return &students[i]; // Return pointer to the found student
        }
    }
    return nullptr; // Return nullptr if not found
}

// Function to search for a student using binary search
// Only works if the array is sorted by ID
Student* binarySearch(Student students[], int size, int targetId) {
    int left = 0, right = size - 1;
    while (left <= right) {
        int mid = (left + right) / 2; // Find middle index
        if (students[mid].id == targetId) {
            return &students[mid]; // Found
        }
        else if (students[mid].id < targetId) {
            left = mid + 1; // Search right half
        }
        else {
            right = mid - 1; // Search left half
        }
    }
    return nullptr; // Not found
}

// Main function with menu loop
int main() {
    Student students[MAX_SIZE]; // Array to store student records
    int currentSize = 0;        // Tracks number of students added so far
    int choice;

    do {
        // Display main menu
        cout << "\n=== Student Record Search System ===\n";
        cout << "1. Add Student\n";
        cout << "2. Sequential Search\n";
        cout << "3. Binary Search\n";
        cout << "4. Display All Students\n";
        cout << "5. Exit\n";
        cout << "Enter your choice: ";
        cin >> choice;

        if (choice == 1) {
            // Add new student
            addStudent(students, currentSize);
        }
        else if (choice == 2 || choice == 3) {
            // Perform search (linear or binary)
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
                bubbleSort(students, currentSize); // Ensure array is sorted
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
            // Show all student records
            displayAllStudents(students, currentSize);
        }
        else if (choice == 5) {
            // Exit the program
            cout << "Exiting program.\n";
        }
        else {
            cout << "Invalid choice. Please try again.\n";
        }

    } while (choice != 5); // Keep looping until user chooses to exit

    return 0;
}