# ATM-Machine-logic-in-CPP
I've build this code which has the same logic as ATM machine 
#include <iostream>
#include <string>
using namespace std;

class Customer {
private:
    string name;
    long long acc_num;
    int pin;
    int balance;

public:
    Customer() {} // default constructor
    Customer(string n, long long acc, int p, int bal) {
        name = n;
        acc_num = acc;
        pin = p;
        balance = bal;
    }

    long long getAccountNumber() {
        return acc_num;
    }

    bool verifyPin(int enteredPin) {
        return enteredPin == pin;
    }

    void withdraw(int amount) {
        if (amount <= balance) {
            balance -= amount;
            cout << "You have successfully withdrawn " << amount << " rupees.\n";
            cout << "Remaining balance: " << balance << endl;
        } else {
            cout << "Insufficient balance.\n";
        }
    }

    void showBalance() {
        cout << "Your current balance is: " << balance << endl;
    }

    string getName() {
        return name;
    }
};


class ATM {
public:
    void start(Customer customers[], int totalCustomers) {
        cout << "----------WELCOME TO CANARA BANK----------\n";
        cout << "Please insert your ATM card (press any number to continue): ";
        int pause;
        cin >> pause;

        long long enteredAcc;
        cout << "Enter your account number: ";
        cin >> enteredAcc;

        Customer* current = findCustomer(customers, totalCustomers, enteredAcc);

        if (current == nullptr) {
            cout << "Account not found.\n";
            return;
        }

        int pin;
        cout << "Enter your ATM PIN: ";
        cin >> pin;

        if (!current->verifyPin(pin)) {
            cout << "Incorrect PIN. Access Denied.\n";
            return;
        }

        cout << "\nWelcome, " << current->getName() << "!\n";
        int choice;
        cout << "Press 1 for Cash Withdrawal\n";
        cout << "Press 2 for Balance Enquiry\n";
        cout << "Enter your choice: ";
        cin >> choice;

        switch (choice) {
            case 1: {
                int amount;
                cout << "Enter the amount: ";
                cin >> amount;
                current->withdraw(amount);
                break;
            }
            case 2: {
                int verify;
                cout << "Please re-enter your PIN: ";
                cin >> verify;
                if (current->verifyPin(verify)) {
                    current->showBalance();
                } else {
                    cout << "Incorrect PIN. Access Denied.\n";
                }
                break;
            }
            default:
                cout << "Invalid option. Try again.\n";
        }
    }

private:
    Customer* findCustomer(Customer customers[], int total, long long accNum) {
        for (int i = 0; i < total; i++) {
            if (customers[i].getAccountNumber() == accNum)
                return &customers[i];
        }
        return nullptr;
    }
};


int main() {
    Customer customers[5] = {
        Customer("Anuj Gupta", 2503057, 2024, 16100),
        Customer("Bhupendra jogi", 2503058, 1234, 20000),
        Customer("Mohhamed malpura", 2503059, 5678, 9500),
        Customer("Tanvir Khan", 2503060, 4321, 30000),
        Customer("Manthan chhedq", 2503061, 9999, 5000)
    };

    ATM atm;
    atm.start(customers, 5);

    return 0;
}
