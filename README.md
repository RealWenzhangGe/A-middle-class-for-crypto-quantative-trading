# A-middle-class-for-crypto-quantative-trading
Hi everyone, Wenzhang here.
For anyone who wants to trade cryptocurrency in a quantative manner, the process of the original APIs from crypto exchanges( Binance, OKCoin, etc.) couldnt be ignored. You need to deal with them to make your trade a reality. However, a quant should focus more on the stradegy level, instead being caught in endless details amid different API processes.
And that's why I'm here. Here's a middle class file based on https://www.fmz.com/, which is one of the biggest quantative crypto trading platform in China. One can easily use the middle class to deal with those APIs. And it would be really helpful no matter you choose to trade on https://www.fmz.com/ or on your own.
The codes lie down below. Enjoy :) Please star it to let me know if it's useful to you.

class midclass():
    def _init_(self, current_exchange):
        '''Initialized data to fill exchange information, get price or account information for the first time
        current_exchange is a structure returned by fmz api'''
        self.init_timestamp = time.time()
        self.exchange = current_exchange
        self.name = self.exchange.GetName()
        self.symbol = self.exchange.GetCurrency()

    def get_account(self):
        '''To get account information
        Returns True when successful, and returns False when failed to get account information'''
        try:
            self.account = self.exchange.GetAccount()
            self.balance = self.account['Balance']
            self.frozen_balance = self.account['FrozenBalance']
            self.stocks = self.account['FrozenStocks']
            return True
        except:
            return False

    def get_ticker(self):
        '''To get account information
        Returns True when successful, and returns False when failed to get ticker information'''
        try:
            self.ticker = self.exchange.GetTicker()

            self.high = self.ticker['High']
            self.low = self.ticker['Low']
            self.sell = self.ticker['Sell']
            self.buy = self.ticker['Buy']
            self.last = self.ticker['Last']
            self.volume = self.ticker['Volume']
            return True
        except:
            return False

    def get_depth(self):
        '''To get account information
        Returns True when successful, and returns False when failed to get depth information'''
        try:
            self.Depth = self.exchange.GetDepth()
            self.Ask = self.Depth['Asks']
            self.Bids = self.Depth['Bids']
            return True
        except:
            return False

    def get_records(self, period):
        '''To get candlestick information
        period here stands for candlestick chart period
        PERIOD_M30 for 30 minutes, PERIOD_H1 for 1 hour, PERIOD_D1 for 1 day, etc.'''
        self.records = exchange.GetRecords(period)

    def make_order(self, direction, price, amount):
        '''To make an order
        direction here stands for the order direction, buy for buy, and vire versa
        price and amount would be the order price and order amount
        Let's say you're trading with BTC/USDT, 1 amount is for 1 BTC
        Returns the order id in crypto exchange'''
        if direction = 'buy'
            try:
                order_id = self.exchange.Buy(price, amount)
            except:
                return False
        elif direction = 'sell'
            try:
                order_id = self.exchange.Buy(price, amount)
            except:
                return False

        return order_id

    def cancel_order(self, order_id):
        '''the order id is the id of the order that you want to cancel
        returns True if order canceled successfully, and False if not canceled'''
        return self.exchange.CancelOrder(order_id)

    def refresh_data(self):
        '''To fresh the data
        returns 'data refreshed successfully' if successful, and the corresponding error message if failed'''
        if not self.get_account()
            return 'Failed to get account.'

        if not self.get_ticker()
            return 'Failed to get ticker.'

        if not self.get_depth()
            return 'Failed to get depth.'

        try:
            self.get_records()
        except:
            return 'Failed to get records.'

        return 'Data refreshed successfully!'
