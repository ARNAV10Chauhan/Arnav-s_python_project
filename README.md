import pandas as pd
import matplotlib.pyplot as plt
import os

 
FILE_NAME = "expenses.csv"

 
if not os.path.exists(FILE_NAME):
    df = pd.DataFrame(columns=["Category", "Amount"])
    df.to_csv(FILE_NAME, index=False)

def add_expense(category, amount):
    """Adds an expense to the CSV file."""
    df = pd.read_csv(FILE_NAME)
    new_data = pd.DataFrame({"Category": [category], "Amount": [amount]})
    df = pd.concat([df, new_data], ignore_index=True)
    df.to_csv(FILE_NAME, index=False)
    print(f"✅ Added expense: {category} - ${amount}")

def show_expenses():
    """Displays all expenses."""
    df = pd.read_csv(FILE_NAME)
    if df.empty:
        print("No expenses recorded yet.")
    else:
        print("\n📜 Expense List:")
        print(df)

def visualize_expenses():
    """Generates a bar chart and pie chart for expense categories."""
    df = pd.read_csv(FILE_NAME)
    if df.empty:
        print("No data to visualize.")
        return

    expense_summary = df.groupby("Category")["Amount"].sum()

    
    plt.figure(figsize=(8, 4))
    expense_summary.plot(kind="bar", color="skyblue", edgecolor="black")
    plt.title("Expense Breakdown by Category")
    plt.xlabel("Category")
    plt.ylabel("Amount ($)")
    plt.xticks(rotation=45)
    plt.grid(axis="y", linestyle="--", alpha=0.7)
    plt.show()

    # Pie Chart
    plt.figure(figsize=(6, 6))
    expense_summary.plot(kind="pie", autopct="%1.1f%%", startangle=90, colormap="Pastel1")
    plt.title("Expense Distribution")
    plt.ylabel("")  
    plt.show()

def main():
    """Main function to run the Expense Manager."""
    while True:
        print("\n📊 EXPENSE MANAGER 📊")
        print("1. Add Expense")
        print("2. Show Expenses")
        print("3. Visualize Expenses")
        print("4. Exit")
        
        choice = input("Enter choice (1-4): ")
        
        if choice == "1":
            category = input("Enter category (e.g., Food, Transport, Bills): ")
            try:
                amount = float(input("Enter amount ($): "))
                add_expense(category, amount)
            except ValueError:
                print("❌ Invalid amount. Please enter a number.")

        elif choice == "2":
            show_expenses()
        
        elif choice == "3":
            visualize_expenses()

        elif choice == "4":
            print("🔚 Exiting Expense Manager. Goodbye!")
            break
        
        else:
            print("❌ Invalid choice. Please enter a number between 1-4.")

if __name__ == "__main__":
    main()
