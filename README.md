 //java-banking-project

import java.util.*;

// BankAccount Class
class BankAccount {
    private String accountNumber;
    private String name;
    private double balance;
    private String password;

    public BankAccount(String accountNumber, String name, double balance, String password) {
        this.accountNumber = accountNumber;
        this.name = name;
        this.balance = balance;
        this.password = password;
    }

    public boolean authenticate(String inputPassword) {
        return password.equals(inputPassword);
    }

    public void deposit(double amount) {
        if (amount > 0) {
            balance += amount;
            System.out.println("Deposited successfully.");
        }
    }

    public void withdraw(double amount) {
        if (amount <= balance) {
            balance -= amount;
            System.out.println("Withdrawal successful.");
        } else {
            System.out.println("Insufficient balance.");
        }
    }

    public void showDetails() {
        System.out.println("Account No: " + accountNumber + " | Name: " + name + " | Balance: ₹" + balance);
    }

    public String getAccountNumber() {
        return accountNumber;
    }

    public double getBalance() {
        return balance;
    }

    public void transfer(BankAccount receiver, double amount) {
        if (amount <= balance) {
            this.withdraw(amount);
            receiver.deposit(amount);
            System.out.println("Transferred successfully to " + receiver.getAccountNumber());
        } else {
            System.out.println("Transfer failed: Insufficient balance.");
        }
    }
}

// Bank Class
class Bank {
    private HashMap<String, BankAccount> accounts = new HashMap<>();

    public void createAccount(String accNo, String name, double initialAmount, String password) {
        if (accounts.containsKey(accNo)) {
            System.out.println("Account already exists.");
            return;
        }
        accounts.put(accNo, new BankAccount(accNo, name, initialAmount, password));
        System.out.println("Account created successfully.");
    }

    public BankAccount getAccount(String accNo) {
        return accounts.get(accNo);
    }

    public void showAllAccounts() {
        for (BankAccount acc : accounts.values()) {
            acc.showDetails();
        }
    }
}

// Main Class
public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        Bank bank = new Bank();

        while (true) {
            System.out.println("\n=== Bank Menu ===");
            System.out.println("1. Create Account");
            System.out.println("2. View Account Details ");
            System.out.println("3. Deposit ");
            System.out.println("4. Withdraw ");
            System.out.println("5. Transfer ");
            System.out.println("6. Show All Accounts ");
            System.out.println("7. Check Balance ");
            System.out.println("8. Exit");
            System.out.print("Choose option: ");
            int choice = sc.nextInt();

            String accNo;
            BankAccount acc;
            double amount;

            switch (choice) {
                case 1:
                    System.out.print("Enter Account Number: ");
                    accNo = sc.next();
                    System.out.print("Enter Name: ");
                    String name = sc.next();
                    System.out.print("Enter Initial Balance: ");
                    amount = sc.nextDouble();
                    System.out.print("Set Password: ");
                    String password = sc.next();
                    bank.createAccount(accNo, name, amount, password);
                    break;

                case 2:
                    System.out.print("Enter Account Number: ");
                    accNo = sc.next();
                    acc = bank.getAccount(accNo);
                    if (acc != null) {
                        System.out.print("Enter Password: ");
                        String pass = sc.next();
                        if (acc.authenticate(pass)) {
                            acc.showDetails();
                        } else {
                            System.out.println("Incorrect password.");
                        }
                    } else {
                        System.out.println("Account not found.");
                    }
                    break;

                case 3:
                    System.out.print("Enter Account Number: ");
                    accNo = sc.next();
                    acc = bank.getAccount(accNo);
                    if (acc != null) {
                        System.out.print("Enter amount to deposit: ");
                        amount = sc.nextDouble();
                        acc.deposit(amount);
                    } else {
                        System.out.println("Account not found.");
                    }
                    break;

                case 4:
                    System.out.print("Enter Account Number: ");
                    accNo = sc.next();
                    acc = bank.getAccount(accNo);
                    if (acc != null) {
                        System.out.print("Enter Password: ");
                        String pass = sc.next();
                        if (acc.authenticate(pass)) {
                            System.out.print("Enter amount to withdraw: ");
                            amount = sc.nextDouble();
                            acc.withdraw(amount);
                        } else {
                            System.out.println("Incorrect password.");
                        }
                    } else {
                        System.out.println("Account not found.");
                    }
                    break;

                case 5:
                    System.out.print("Sender Account No: ");
                    String senderNo = sc.next();
                    BankAccount sender = bank.getAccount(senderNo);
                    if (sender != null) {
                        System.out.print("Enter Password: ");
                        String pass = sc.next();
                        if (sender.authenticate(pass)) {
                            System.out.print("Receiver Account No: ");
                            String receiverNo = sc.next();
                            BankAccount receiver = bank.getAccount(receiverNo);
                            if (receiver != null) {
                                System.out.print("Amount to transfer: ");
                                amount = sc.nextDouble();
                                sender.transfer(receiver, amount);
                            } else {
                                System.out.println("Receiver account not found.");
                            }
                        } else {
                            System.out.println("Incorrect password.");
                        }
                    } else {
                        System.out.println("Sender account not found.");
                    }
                    break;

                case 6:
                    bank.showAllAccounts(); // Admin-like view
                    break;

                case 7:
                    System.out.print("Enter Account Number: ");
                    accNo = sc.next();
                    acc = bank.getAccount(accNo);
                    if (acc != null) {
                        System.out.print("Enter Password: ");
                        String pass = sc.next();
                        if (acc.authenticate(pass)) {
                            System.out.println("Current Balance: ₹" + acc.getBalance());
                        } else {
                            System.out.println("Incorrect password.");
                        }
                    } else {
                        System.out.println("Account not found.");
                    }
                    break;

                case 8:
                    System.out.println("Thanks for using our banking system.");
                    return;

                default:
                    System.out.println("Invalid option.");
            }
        }
    }
}
