# contact-management-system-
#include <iostream>
#include <vector>
#include <fstream>
#include <string>
using namespace std;

struct Contact {
    string name;
    string phone;
    string email;
    string address;
};

// Display one contact
void displayContact(const Contact& c) {
    cout << "\nName    : " << c.name;
    cout << "\nPhone   : " << c.phone;
    cout << "\nEmail   : " << c.email;
    cout << "\nAddress : " << c.address << "\n";
}

// Save contacts to file
void saveContacts(const vector<Contact>& contacts) {
    ofstream file("contacts.txt");

    for (const Contact& c : contacts) {
        file << c.name << "|"
             << c.phone << "|"
             << c.email << "|"
             << c.address << "\n";
    }

    file.close();
}

// Load contacts from file
void loadContacts(vector<Contact>& contacts) {
    ifstream file("contacts.txt");

    if (!file)
        return;

    Contact c;
    string line;

    while (getline(file, line)) {
        size_t p1 = line.find('|');
        size_t p2 = line.find('|', p1 + 1);
        size_t p3 = line.find('|', p2 + 1);

        if (p1 == string::npos ||
            p2 == string::npos ||
            p3 == string::npos)
            continue;

        c.name = line.substr(0, p1);
        c.phone = line.substr(p1 + 1, p2 - p1 - 1);
        c.email = line.substr(p2 + 1, p3 - p2 - 1);
        c.address = line.substr(p3 + 1);

        contacts.push_back(c);
    }

    file.close();
}

// Add contact
void addContact(vector<Contact>& contacts) {
    Contact c;

    cin.ignore();

    cout << "\nEnter name: ";
    getline(cin, c.name);

    cout << "Enter phone: ";
    getline(cin, c.phone);

    cout << "Enter email: ";
    getline(cin, c.email);

    cout << "Enter address: ";
    getline(cin, c.address);

    contacts.push_back(c);
    saveContacts(contacts);

    cout << "\nContact added successfully!\n";
}

// Display all contacts
void displayAll(const vector<Contact>& contacts) {
    if (contacts.empty()) {
        cout << "\