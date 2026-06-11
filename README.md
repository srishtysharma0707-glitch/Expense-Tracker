
import csv                                                    # allows to read and write csv file
import os                                                     # used to check if a file already exists

FILE_NAME = "expenses.csv"                                    # file name is defined where expenses will be saved

expenses = []                                                 # defined as a list which stores all expenses
monthly_budget = 0                                            # store monthly budget of user
                                                              # global variables


# ---------------- LOAD EXPENSES FROM CSV ----------------
def load_expenses():                                          # define function to load old expenses
    if os.path.exists(FILE_NAME):                             # check if expenses.csv file exist
        with open(FILE_NAME, mode="r", newline="") as file:   # with function ensures the file closes automatically
            reader = csv.DictReader(file)                     # read csv data as dictionaries
            for row in reader:                                # for loop runs through each csv file
                row["amount"] = float(row["amount"])          # amount converted to float number
                expenses.append(row)                          # adds each row to the dictionary


# ---------------- SAVE EXPENSES TO CSV ----------------
def save_expenses():                                          # function to store all expenses
    with open(FILE_NAME, mode="w", newline="") as file:       # overwrite old data with updated data
        fieldnames = ["date", "category", "amount", "description"] #CSV column names defined
        writer = csv.DictWriter(file, fieldnames=fieldnames)  # writer object,allows writing dictionary data to csv 
        writer.writeheader()                                  # write first row
        for expense in expenses:                              # for loop through each expense dictionary
            writer.writerow(expense)                          # write one expense per row

    print("Expenses saved successfully!")


# ---------------- ADD EXPENSE ----------------
def add_expense():                                            # function defined to add user expense and store it
    date = input("Enter date (YYYY-MM-DD): ")                 # Input - Date
    category = input("Enter category (Food, Travel, etc): ")  # Input - Category
    amount = float(input("Enter amount spent: "))             # Input - Amount and then converts into float
    description = input("Enter description: ")                # Input - Description

    expense = {                                               # Dictionary created
        "date": date,
        "category": category,
        "amount": amount,
        "description": description
    }

    expenses.append(expense)                                  # Adds the dictionary to the expense list
    print("Expense added successfully!")


# ---------------- VIEW EXPENSES ----------------
def view_expenses():                                          # function to display stored expenses
    if not expenses:                                          # check if list is empty
        print("No expenses to display.")                      # output shows as no data exists and function stopped
        return

    print("\n--- Expense List ---")                           #next line
    for expense in expenses:                                  # For loop through each expense
        # Validation
        if (                                                  #Validation check
            not expense.get("date")
            or not expense.get("category")
            or not expense.get("amount")
            or not expense.get("description")
        ):
            print("Incomplete expense found. Skipping entry.") #skips invalid record
            continue

        print(                                                 # output displayed for expense
            f"Date: {expense['date']} | "
            f"Category: {expense['category']} | "
            f"Amount: ₹{expense['amount']} | "
            f"Description: {expense['description']}"
        )


# ---------------- SET BUDGET ----------------
def set_budget():                                              # Function defined to set budget
    global monthly_budget                                      # allows modification of global variable
    monthly_budget = float(input("Enter monthly budget: "))    # input from user 
    print("Monthly budget set successfully!")                  # output


# ---------------- TRACK BUDGET ----------------
def track_budget():                                            # function to compare expenses with budget
    total_spent = sum(expense["amount"] for expense in expenses) # calculate total expense

    print(f"Total expenses: ₹{total_spent}")                   # print total expense
    print(f"Monthly budget: ₹{monthly_budget}")                # print total budget

    if total_spent > monthly_budget:                           # if else condition to check if spent is more or less than montly budget
        print("⚠️ You have exceeded your budget!")             # Warning message
    else:
        remaining = monthly_budget - total_spent
        print(f"✅ You have ₹{remaining} left for the month")  # Remaining balance as output


# ---------------- MENU ----------------
def menu():                                                    # Main function
    load_expenses()                                            # load old expenses

    while True:                                                # While loop to run program - infinite
        print("\n===== Personal Expense Tracker =====")        # Display Menu
        print("1. Add expense")
        print("2. View expenses")
        print("3. Track budget")
        print("4. Save expenses")
        print("5. Exit")

        choice = input("Enter your choice (1-5): ")            # Shows user to input their choice

        if choice == "1":                                      # Add Expense
            add_expense()
        elif choice == "2":                                    # View Expense
            view_expenses()
        elif choice == "3":                                    # Set a budget
            set_budget()
            track_budget()
        elif choice == "4":                                    # Save Expense Manually
            save_expenses()
        elif choice == "5":                                    
            save_expenses()
            print("Exiting program.")                          # Save and exits program
            break
        else:
            print("Invalid choice. Please try again.")         # Output shown in case of wrong input


# ---------------- PROGRAM START ----------------
menu()                                                         # Program started
===== Personal Expense Tracker =====
1. Add expense
2. View expenses
3. Track budget
4. Save expenses
5. Exit
Enter your choice (1-5): 1
Enter date (YYYY-MM-DD): 2026-01-04
Enter category (Food, Travel, etc): Food
Enter amount spent: 1000
Enter description: Weekend Food - Mall
Expense added successfully!

===== Personal Expense Tracker =====
1. Add expense
2. View expenses
3. Track budget
4. Save expenses
5. Exit
Enter your choice (1-5): 1
Enter date (YYYY-MM-DD): 2026-01-08
Enter category (Food, Travel, etc): Travel
Enter amount spent: 9000
Enter description: Resort
Expense added successfully!

===== Personal Expense Tracker =====
1. Add expense
2. View expenses
3. Track budget
4. Save expenses
5. Exit
Enter your choice (1-5): 1
Enter date (YYYY-MM-DD): 2026-01-15
Enter category (Food, Travel, etc): Shopping
Enter amount spent: 10000
Enter description: Formal clothes
Expense added successfully!

===== Personal Expense Tracker =====
1. Add expense
2. View expenses
3. Track budget
4. Save expenses
5. Exit
Enter your choice (1-5): 1
Enter date (YYYY-MM-DD): 2026-01-24
Enter category (Food, Travel, etc): Petrol
Enter amount spent: 3000
Enter description: Miscellaneous
Expense added successfully!

===== Personal Expense Tracker =====
1. Add expense
2. View expenses
3. Track budget
4. Save expenses
5. Exit
Enter your choice (1-5): 2

--- Expense List ---
Date: 2026-01-04 | Category: Food | Amount: ₹1000.0 | Description: Weekend Food - Mall
Date: 2026-01-08 | Category: Travel | Amount: ₹9000.0 | Description: Resort
Date: 2026-01-15 | Category: Shopping | Amount: ₹10000.0 | Description: Formal clothes
Date: 2026-01-24 | Category: Petrol | Amount: ₹3000.0 | Description: Miscellaneous

===== Personal Expense Tracker =====
1. Add expense
2. View expenses
3. Track budget
4. Save expenses
5. Exit
Enter your choice (1-5): 3
Enter monthly budget: 15000
Monthly budget set successfully!
Total expenses: ₹23000.0
Monthly budget: ₹15000.0
⚠️ You have exceeded your budget!

===== Personal Expense Tracker =====
1. Add expense
2. View expenses
3. Track budget
4. Save expenses
5. Exit
Enter your choice (1-5): 4
Expenses saved successfully!

===== Personal Expense Tracker =====
1. Add expense
2. View expenses
3. Track budget
4. Save expenses
5. Exit
Enter your choice (1-5): 3
Enter monthly budget: 2
Monthly budget set successfully!
Total expenses: ₹23000.0
Monthly budget: ₹2.0
⚠️ You have exceeded your budget!

===== Personal Expense Tracker =====
1. Add expense
2. View expenses
3. Track budget
4. Save expenses
5. Exit
Enter your choice (1-5): 5
Expenses saved successfully!
Exiting program.
 
