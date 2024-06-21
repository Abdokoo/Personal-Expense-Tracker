import csv
import os
from datetime import datetime

EXPENSES_FILE = 'expenses.csv'
CATEGORIES = ['Food', 'Transportation', 'Utilities', 'Entertainment', 'Other']

def add_expense():
    amount = float(input("Enter amount: "))
    category = input(f"Enter category ({', '.join(CATEGORIES)}): ")
    date = input("Enter date (YYYY-MM-DD): ")
    description = input("Enter description: ")
    
    with open(EXPENSES_FILE, mode='a', newline='') as file:
        writer = csv.writer(file)
        writer.writerow([amount, category, date, description])
    
    print("Expense added successfully!")

def view_expenses():
    if not os.path.exists(EXPENSES_FILE):
        print("No expenses recorded yet.")
        return
    
    with open(EXPENSES_FILE, mode='r') as file:
        reader = csv.reader(file)
        for row in reader:
            print(f"Amount: {row[0]}, Category: {row[1]}, Date: {row[2]}, Description: {row[3]}")

def edit_expense():
    expenses = []
    if not os.path.exists(EXPENSES_FILE):
        print("No expenses recorded yet.")
        return
    
    with open(EXPENSES_FILE, mode='r') as file:
        reader = csv.reader(file)
        expenses = list(reader)
    
    view_expenses()
    index = int(input("Enter the index of the expense to edit: "))
    
    if 0 <= index < len(expenses):
        amount = float(input("Enter new amount: "))
        category = input(f"Enter new category ({', '.join(CATEGORIES)}): ")
        date = input("Enter new date (YYYY-MM-DD): ")
        description = input("Enter new description: ")
        
        expenses[index] = [amount, category, date, description]
        
        with open(EXPENSES_FILE, mode='w', newline='') as file:
            writer = csv.writer(file)
            writer.writerows(expenses)
        
        print("Expense updated successfully!")
    else:
        print("Invalid index!")

def delete_expense():
    expenses = []
    if not os.path.exists(EXPENSES_FILE):
        print("No expenses recorded yet.")
        return
    
    with open(EXPENSES_FILE, mode='r') as file:
        reader = csv.reader(file)
        expenses = list(reader)
    
    view_expenses()
    index = int(input("Enter the index of the expense to delete: "))
    
    if 0 <= index < len(expenses):
        del expenses[index]
        
        with open(EXPENSES_FILE, mode='w', newline='') as file:
            writer = csv.writer(file)
            writer.writerows(expenses)
        
        print("Expense deleted successfully!")
    else:
        print("Invalid index!")

def summary_statistics():
    if not os.path.exists(EXPENSES_FILE):
        print("No expenses recorded yet.")
        return
    
    total_expenses = 0.0
    category_totals = {category: 0.0 for category in CATEGORIES}
    date_expenses = {}
    
    with open(EXPENSES_FILE, mode='r') as file:
        reader = csv.reader(file)
        for row in reader:
            amount = float(row[0])
            category = row[1]
            date = row[2]
            
            total_expenses += amount
            category_totals[category] += amount
            
            if date not in date_expenses:
                date_expenses[date] = 0.0
            date_expenses[date] += amount
    
    average_daily_expenses = total_expenses / len(date_expenses)
    
    print(f"Total expenses: {total_expenses}")
    print(f"Average daily expenses: {average_daily_expenses}")
    print("Expenses by category:")
    for category, total in category_totals.items():
        print(f"  {category}: {total}")

def main_menu():
    while True:
        print("\nPersonal Expense Tracker")
        print("1. Add Expense")
        print("2. View Expenses")
        print("3. Edit Expense")
        print("4. Delete Expense")
        print("5. Summary Statistics")
        print("6. Exit")
        choice = input("Enter your choice: ")
        
        if choice == '1':
            add_expense()
        elif choice == '2':
            view_expenses()
        elif choice == '3':
            edit_expense()
        elif choice == '4':
            delete_expense()
        elif choice == '5':
            summary_statistics()
        elif choice == '6':
            break
        else:
            print("Invalid choice. Please try again.")

if __name__ == "__main__":
    main_menu()
