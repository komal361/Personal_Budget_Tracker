# Personal_Budget_Tracker_project




import json

def add_transaction(transactions, transaction_type, category, amount):
    transactions.append({"type": transaction_type, "category": category, "amount": amount})

def calculate_budget(transactions):
    total_income = sum(transaction["amount"] for transaction in transactions if transaction["type"] == "income")
    total_expenses = sum(transaction["amount"] for transaction in transactions if transaction["type"] == "expense")
    remaining_budget = total_income - total_expenses
    return remaining_budget

def categorize_expenses(transactions):
    categories = {}
    for transaction in transactions:
        category = transaction["category"]
        amount = transaction["amount"]
        if category in categories:
            categories[category] += amount
        else:
            categories[category] = amount
    return categories

def save_transactions(transactions):
    with open('transactions.json', 'w') as file:
        json.dump(transactions, file)

def load_transactions():
    try:
        with open('transactions.json', 'r') as file:
            return json.load(file)
    except FileNotFoundError:
        return []

transactions = load_transactions()

while True:
    print("1. Add Transaction")
    print("2. Calculate Remaining Budget")
    print("3. Categorize Expenses")
    print("4. Save and Exit")

    choice = int(input('Enter your choice: '))

    if choice == 1:
        transaction_type = input("Enter transaction type (income/expense): ")
        category = input("Enter category: ")
        amount = float(input("Enter amount: "))
        add_transaction(transactions, transaction_type, category, amount)
    elif choice == 2:
        remaining_budget = calculate_budget(transactions)
        print(f"Remaining Budget: {remaining_budget}")
    elif choice == 3:
        categories = categorize_expenses(transactions)
        print("Expenses by Category:")
        for category, amount in categories.items():
            print(f"{category}: {amount}")
    elif choice == 4:
        save_transactions(transactions)
        break
