import yfinance as yf

class StockPortfolio:
    def __init__(self):
        self.portfolio = {}

    def add_stock(self, ticker, shares):
        if ticker in self.portfolio:
            self.portfolio[ticker]['shares'] += shares
        else:
            self.portfolio[ticker] = {'shares': shares, 'data': yf.Ticker(ticker)}

    def remove_stock(self, ticker, shares):
        if ticker in self.portfolio:
            if self.portfolio[ticker]['shares'] >= shares:
                self.portfolio[ticker]['shares'] -= shares
            else:
                print("You don't have enough shares to remove.")
        else:
            print("You don't own any shares of this stock.")

    def track_performance(self):
        total_value = 0
        for ticker, info in self.portfolio.items():
            data = info['data']
            current_price = data.info['regularMarketPrice']
            total_value += current_price * info['shares']
            print(f"{ticker}: {info['shares']} shares, Current Price: {current_price}, Total Value: {current_price * info['shares']}")
        print(f"Total Portfolio Value: {total_value}")


def main():
    portfolio = StockPortfolio()
    while True:
        print("\nOptions:")
        print("1. Add Stock")
        print("2. Remove Stock")
        print("3. Track Performance")
        print("4. Exit")
        option = input("Choose an option: ")
        if option == "1":
            ticker = input("Enter the ticker symbol: ")
            shares = int(input("Enter the number of shares: "))
            portfolio.add_stock(ticker, shares)
        elif option == "2":
            ticker = input("Enter the ticker symbol: ")
            shares = int(input("Enter the number of shares to remove: "))
            portfolio.remove_stock(ticker, shares)
        elif option == "3":
            portfolio.track_performance()
        elif option == "4":
            break
        else:
            print("Invalid option. Please choose again.")


if __name__ == "__main__":
    main()
