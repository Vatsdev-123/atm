#include <iostream>
#include <unordered_map>
#include <string>
#include <iomanip>

using namespace std;

// User account structure
struct Account {
    string username;
    string password;
    double balance;
};

// Predefined accounts (for demonstration)
unordered_map<string, Account> accounts = {
    {"user1", {"user1", "pass123", 5000.0}},
    {"user2", {"user2", "secure456", 3000.0}},
    {"user3", {"user3", "mypin789", 10000.0}}
};

// Function prototypes
bool login(string &username);
void displayMenu();
void balanceInquiry(const Account &account);
void deposit(Account &account);
void withdraw(Account &account);
void transfer(Account &account);

int main() {
    string username;
    if (!login(username)) {
        cout << "Too many failed attempts. Exiting.\n";
        return 1;
    }

    Account &currentUser = accounts[username];
    int choice;

    do {
        displayMenu();
        cout << "Enter your choice: ";
        cin >> choice;

        switch (choice) {
            case 1:
                balanceInquiry(currentUser);
                break;
            case 2:
                deposit(currentUser);
                break;
            case 3:
                withdraw(currentUser);
                break;
            case 4:
                transfer(currentUser);
                break;
            case 5:
                cout << "Logging out. Thank you for using the ATM Simulator!\n";
                break;
            default:
                cout << "Invalid choice. Please try again.\n";
        }
    } while (choice != 5);

    return 0;
}

// Function to handle user login
bool login(string &username) {
    string inputUser, inputPass;
    int attempts = 3;

    while (attempts > 0) {
        cout << "=== ATM Simulator Login ===\n";
        cout << "Username: ";
        cin >> inputUser;
        cout << "Password: ";
        cin >> inputPass;

        if (accounts.find(inputUser) != accounts.end() && accounts[inputUser].password == inputPass) {
            username = inputUser;
            cout << "Login successful! Welcome, " << username << ".\n";
            return true;
        } else {
            attempts--;
            cout << "Invalid username or password. Attempts remaining: " << attempts << "\n";
        }
    }

    return false;
}

// Function to display the main menu
void displayMenu() {
    cout << "\n=== ATM Simulator Menu ===\n";
    cout << "1. Balance Inquiry\n";
    cout << "2. Deposit\n";
    cout << "3. Withdraw\n";
    cout << "4. Transfer Funds\n";
    cout << "5. Logout\n";
}

// Function to show account balance
void balanceInquiry(const Account &account) {
    cout << "Your current balance is: $" << fixed << setprecision(2) << account.balance << "\n";
}

// Function to handle deposits
void deposit(Account &account) {
    double amount;
    cout << "Enter amount to deposit: $";
    cin >> amount;

    if (amount <= 0) {
        cout << "Invalid deposit amount. Please enter a positive value.\n";
        return;
    }

    account.balance += amount;
    cout << "Deposit successful! New balance: $" << fixed << setprecision(2) << account.balance << "\n";
}

// Function to handle withdrawals
void withdraw(Account &account) {
    double amount;
    cout << "Enter amount to withdraw: $";
    cin >> amount;

    if (amount <= 0) {
        cout << "Invalid withdrawal amount. Please enter a positive value.\n";
    } else if (amount > account.balance) {
        cout << "Insufficient funds. Your balance is: $" << fixed << setprecision(2) << account.balance << "\n";
    } else {
        account.balance -= amount;
        cout << "Withdrawal successful! New balance: $" << fixed << setprecision(2) << account.balance << "\n";
    }
}

// Function to handle fund transfers
void transfer(Account &account) {
    string recipient;
    double amount;

    cout << "Enter recipient username: ";
    cin >> recipient;
    if (accounts.find(recipient) == accounts.end()) {
        cout << "Recipient account not found.\n";
        return;
    }

    cout << "Enter amount to transfer: $";
    cin >> amount;

    if (amount <= 0) {
        cout << "Invalid transfer amount. Please enter a positive value.\n";
    } else if (amount > account.balance) {
        cout << "Insufficient funds. Your balance is: $" << fixed << setprecision(2) << account.balance << "\n";
    } else {
        account.balance -= amount;
        accounts[recipient].balance += amount;
        cout << "Transfer successful! New balance: $" << fixed << setprecision(2) << account.balance << "\n";
    }
}
