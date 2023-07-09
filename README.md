#include<iostream>
#include <string>

using namespace std;
class account {
private:
	string accountno;
	string accholdername;
	string acctype;
	double balance;
	double* transaction;
	int maxtransact, numtransact;

public:

	account() :balance(0), transaction(nullptr), numtransact(0), maxtransact(0) {}
	account(const string accno, const string accho, const string actype, double bal)
		:accountno(accno), accholdername(accho), acctype(actype), balance(bal), transaction(nullptr), numtransact(0), maxtransact(0) {}
	~account()
	{
		delete[]transaction;
	}
	string getaccno()const
	{
		return accountno;
	}
	string getaccholdername() const
	{
		return accholdername;
	}
	string getacctype() const
	{
		return acctype;
	}

	double getBalance() const
	{
		return balance;

	}
	void deposit(double amount) {
		if (amount > 0) {
			balance += amount;
			cout << "rs." << amount << "are deposited\n";
		}
		else
			cout << "invalid amount deposited" << endl;

	}
	void withdraw(double amount) {
		if (balance >= amount) {
			balance -= amount;
			cout << "rs." << amount << " are withdraw.\n";
		}
		else {
			cout << "Insufficient balance. Withdrawal failed.\n";
		}
	}
	void addtransaction(double amount) {
		if (numtransact == maxtransact) {
			maxtransact= (maxtransact == 0) ? 1 : maxtransact * 2;

			double* newtransact = new double[maxtransact];
			for (int i = 0; i < numtransact; i++) {
				newtransact[i] = transaction[i];

			}

			delete[]transaction;
			transaction = newtransact;
		}
		transaction[numtransact++] = amount;
	}
	void displayAccount() const {
		cout << "Account Number: " << accountno << endl;
		cout << "Account Holder Name: " << accholdername << endl;
		cout << "Account Type: " << acctype << endl;
		cout << "Balance: rs." << balance << endl;
		cout << "transaction history:" << endl;
		for (int i = 0; i < numtransact; i++) {
			cout << "transaction" << (i + 1) << ":rs." << transaction[i] << endl;
		}
	}
};

class Bank {
private:

	account* accounts;
	int numofaccounts;
	int maxaccounts;

public:
	Bank() : numofaccounts(0), accounts(nullptr), maxaccounts(0) {}
	~Bank() {
		delete[]accounts;

	}
	void createAccount(const string& accho, const string& actype, double bal) {
		string accno = newAccountNumber();
		account newAccount(accno, accho, actype, bal);
		addaccount(newAccount);
		cout << "Account created successfully. Account Number: " << accno << endl;
	}




	void addaccount( account acc) {
		if (numofaccounts == maxaccounts) {
			maxaccounts = (maxaccounts == 0) ? 1 : maxaccounts * 2;
			account* newaccount = new account[maxaccounts];
			for (int i = 1; i < numofaccounts; i++) {
				newaccount[i] = accounts[i];
			}


			delete[] accounts;
			accounts = newaccount;
		}
		accounts[numofaccounts++] = acc;
	}

	string newAccountNumber() {
		numofaccounts++;
		string accountNumber = "ACCT" + to_string(numofaccounts);
		return accountNumber;
	}

	void displayAllAccounts() const {
		if (numofaccounts == 0) {
			cout << "No accounts found." << endl;
		}
		else {
			cout << "----- All Accounts -----" << endl;
			for (int i = 0; i < numofaccounts; i++) {
				accounts[i].displayAccount();
				cout << "------------------------" << endl;
			}
		}
	}

	void updateAccount(const string accountno, const string accholdername) {
		for (int i = 0; i < numofaccounts; i++) {
			if (accounts[i].getaccno() == accountno) {
				accounts[i] = account(accountno, accholdername, accounts[i].getacctype(), accounts[i].getBalance());
				cout << "Account information updated successfully." << endl;
				return;
			}
		}
		cout << "Account not found." << endl;
	}

	void closeAccount(const string accountNumber) {
		for (int i = 0; i < numofaccounts; i++) {
			if (accounts[i].getaccno() == accountNumber) {
				for (int j = i; j < numofaccounts - 1; j++) {
					accounts[j] = accounts[j + 1];
				}
				numofaccounts--;
				cout << "Account closed successfully." << endl;
				return;
			}
		}
		cout << "Account not found. Close failed." << endl;
	}

	void deposit(const string accountno, double amount) {

		for (int i = 0; i < numofaccounts; i++) {
			if (accounts[i].getaccno() == accountno) {
				accounts[i].deposit(amount);

				return;
			}
		}
		cout << "Account not found. Deposit failed." << endl;
	}

	void withdraw(const string accountno, double amount) {
		for (int i = 0; i < numofaccounts; i++) {
			if (accounts[i].getaccno() == accountno) {
				accounts[i].withdraw(amount);
				return;
			}
		}
		cout << "Account not found. Withdrawal failed." << endl;
	}

	void checkBalance(const string accountno) const {
		for (int i = 0; i < numofaccounts; i++) {
			if (accounts[i].getaccno() == accountno) {
				cout << "Account Balance: Rs." << accounts[i].getBalance() << endl;
				return;
			}
		}
		cout << "Account not found." << endl;
	}
};

int main() {
	Bank bank;
	bank.createAccount("Tahreem Noman", "Saving", 300000.0);
	bank.createAccount("Rayan Qadir", "Current", 130000.0);
	bank.createAccount("Rabia Khalid", "Saving", 400000.0);


	while (true) {
		cout << "\n----- Banking Management System -----" << endl;
		cout << "1. Create New Bank Account" << endl;
		cout << "2. Deposit Amount to the Account" << endl;
		cout << "3. Withdraw Amount from the Account" << endl;
		cout << "4. Check Balance Inquiry" << endl;
		cout << "5. Display All Account Holders" << endl;
		cout << "6. Update Bank Account" << endl;
		cout << "7. Close Account" << endl;
		cout << "8. Exit" << endl;
		cout << "Enter your choice: ";

		int choice;
		cin >> choice;

		switch (choice) {
		case 1: {
			string accho, actype;
			double bal;

			cout << "Enter Account Holder Name: ";
			cin >> accho;

			cout << "Enter Account Type (Saving or Current): ";
			cin >> actype;

			cout << "Enter Initial Deposit Amount: ";
			cin >> bal;

			bank.createAccount(accho, actype, bal);
			break;
		}
		case 2: {
			string accountno;
			double depositamount;

			cout << "Enter Account Number: ";
			cin >> accountno;

			cout << "Enter Deposit Amount: ";
			cin >> depositamount;

			bank.deposit(accountno, depositamount);
			break;
		}
		case 3: {
			string accountno;
			double withdrawamount;

			cout << "Enter Account Number: ";
			cin >> accountno;

			cout << "Enter Withdraw Amount: ";
			cin >> withdrawamount;

			bank.withdraw(accountno, withdrawamount);
			break;
		}
		case 4: {
			string accountno;

			cout << "Enter Account Number: ";
			cin >> accountno;

			bank.checkBalance(accountno);
			break;
		}
		case 5:
			bank.displayAllAccounts();
			break;
		case 6: {
			string accountno, accho;

			cout << "Enter Account Number: ";
			cin >> accountno;

			cout << "Enter New Holder Name: ";
			cin >> accho;

			bank.updateAccount(accountno, accho);
			break;
		}
		case 7: {
			string accountno;

			cout << "Enter Account Number: ";
			cin >> accountno;

			bank.closeAccount(accountno);
			break;
		}
		case 8:
			cout << "Thank you for using  Banking Management System." << endl;
			return 0;
		default:
			cout << "Invalid choice. Please try again." << endl;
			break;
		}
	}

	return 0;
}
