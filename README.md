stock_portfolio_tracker.py"""
Stock Portfolio Tracker
------------------------
A beginner-friendly program that lets a user enter stock names and
quantities, calculates the total investment using hardcoded stock
prices, displays the result, and saves it to a text file.
"""

# -----------------------------
# SECTION 1: Hardcoded stock prices
# -----------------------------
# In a real app these prices would come from an API.
# Here we use a simple dictionary: { "STOCK_SYMBOL": price_per_share }
stock_prices = {
    "AAPL": 195.50,   # Apple
    "GOOGL": 175.20,  # Alphabet (Google)
    "MSFT": 420.10,   # Microsoft
    "TSLA": 245.75,   # Tesla
    "AMZN": 185.30,   # Amazon
}


# -----------------------------
# SECTION 2: Function to show available stocks
# -----------------------------
def show_available_stocks():
    """Print the list of stocks the user can invest in."""
    print("\nAvailable stocks and their prices:")
    for symbol, price in stock_prices.items():
        print(f"  {symbol}: ${price:.2f}")
    print()


# -----------------------------
# SECTION 3: Function to collect user input
# -----------------------------
def get_user_portfolio():
    """
    Ask the user to enter stock names and quantities.
    Returns a dictionary like: { "AAPL": 10, "TSLA": 5 }
    User types 'done' to stop entering stocks.
    """
    portfolio = {}

    while True:
        stock_name = input("Enter stock symbol (or 'done' to finish): ").upper().strip()

        # Exit condition
        if stock_name == "DONE":
            break

        # Validate the stock exists in our hardcoded price list
        if stock_name not in stock_prices:
            print(f"  '{stock_name}' not found in price list. Try again.\n")
            continue

        # Validate the quantity is a positive number
        try:
            quantity = int(input(f"Enter quantity of {stock_name}: "))
            if quantity <= 0:
                print("  Quantity must be greater than 0.\n")
                continue
        except ValueError:
            print("  Please enter a valid whole number for quantity.\n")
            continue

        # Add to portfolio (if stock entered twice, add to existing quantity)
        portfolio[stock_name] = portfolio.get(stock_name, 0) + quantity
        print(f"  Added {quantity} share(s) of {stock_name}.\n")

    return portfolio


# -----------------------------
# SECTION 4: Function to calculate total investment
# -----------------------------
def calculate_investment(portfolio):
    """
    Given a portfolio dictionary {symbol: quantity},
    returns:
      - a list of rows (symbol, quantity, price, cost) for display
      - the grand total investment amount
    """
    rows = []
    total = 0.0

    for symbol, quantity in portfolio.items():
        price = stock_prices[symbol]
        cost = price * quantity
        total += cost
        rows.append((symbol, quantity, price, cost))

    return rows, total


# -----------------------------
# SECTION 5: Function to display results on screen
# -----------------------------
def display_results(rows, total):
    """Print a formatted summary of the portfolio to the console."""
    print("\n===== PORTFOLIO SUMMARY =====")
    print(f"{'Stock':<8}{'Qty':<6}{'Price':<10}{'Cost':<10}")
    print("-" * 34)

    for symbol, quantity, price, cost in rows:
        print(f"{symbol:<8}{quantity:<6}${price:<9.2f}${cost:<9.2f}")

    print("-" * 34)
    print(f"TOTAL INVESTMENT: ${total:,.2f}")
    print("==============================\n")


# -----------------------------
# SECTION 6: Function to save results to a text file
# -----------------------------
def save_to_file(rows, total, filename="portfolio_report.txt"):
    """Write the same portfolio summary into a text file."""
    with open(filename, "w") as file:
        file.write("===== PORTFOLIO SUMMARY =====\n")
        file.write(f"{'Stock':<8}{'Qty':<6}{'Price':<10}{'Cost':<10}\n")
        file.write("-" * 34 + "\n")

        for symbol, quantity, price, cost in rows:
            file.write(f"{symbol:<8}{quantity:<6}${price:<9.2f}${cost:<9.2f}\n")

        file.write("-" * 34 + "\n")
        file.write(f"TOTAL INVESTMENT: ${total:,.2f}\n")
        file.write("==============================\n")

    print(f"Report saved to '{filename}'.")


# -----------------------------
# SECTION 7: Main program flow
# -----------------------------
def main():
    print("Welcome to the Stock Portfolio Tracker!")
    show_available_stocks()

    portfolio = get_user_portfolio()

    # If user didn't enter any valid stock, exit gracefully
    if not portfolio:
        print("No stocks entered. Exiting program.")
        return

    rows, total = calculate_investment(portfolio)
    display_results(rows, total)
    save_to_file(rows, total)


# Run the program only if this file is executed directly
if __name__ == "__main__":
    main()
