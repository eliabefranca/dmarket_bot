# dmarket_bot
Bot for automatic trading on dmarket

### How to use:

- `git clone https://github.com/timagr615/dmarket_bot.git`
- `cd dmarket_bot`
- Create a virtual environment, for example: `python3 -m venv venv`
- Then activate the virtual environment: `. venv/bin/activate`
- `pip install -r requirements.txt`
- Create a file `credentials.py` in the root directory with the following content:

```python
PUBLIC_KEY = "your public api key"
SECRET_KEY = "your secret api key"
```

- You can start the bot using the `main.py` file.

## Features
- Multi-game trading. Supports all games available on dmarket.
- Automatic analysis of the skin database for each game.
- Placing orders selected by 15 different parameters. Competing for the top spot in orders.
- Automatic listing of skins for sale after purchase. Price adjustment according to settings and competition for the top position.

## Parameters
All bot parameters are located in the `config.py` file in the root directory of the bot.

### Detailed description of parameters:
- `logger_config` - logger configuration:
```python
logger_config = {
    "handlers": [
        {"sink": sys.stderr, 'colorize': True, 'level': 'INFO'},
        # {"sink": "log/debug.log", "serialize": False, 'level': 'DEBUG'},
        {"sink": "log/info.log", "serialize": False, 'level': 'INFO'},
    ]
}
logger.configure(**logger_config)
```
`"sink": sys.stderr` - logs output to the console  
`"sink": "log/info.log"` - logs output to a file  
`'level': 'INFO'` - log level. Possible levels: `TRACE, DEBUG, INFO, SUCCESS, WARNING, ERROR, CRITICAL`. Each subsequent level from left to right restricts logging of lower levels. For example, if the `INFO` level is set, messages with `TRACE, DEBUG` levels will not be logged.

- `GAMES = [Games.CS, Games.DOTA, Games.RUST]` - list of games to trade. Available values: `Games.CS, Games.DOTA, Games.RUST, Games.TF2`
- `PREV_BASE = 60 * 60 * 4` - update skin database every `PREV_BASE` seconds
- `ORDERS_BASE = 60 * 10` - update orders database every `ORDERS_BASE` seconds
- `BAD_ITEMS` - list of words. If a word is in the item's name, it will not be bought.

### BuyParams - Order placement parameters
- `STOP_ORDERS_BALANCE = 1000` - Stop placing orders when the balance is $10 below the minimum order price.
- `MIN_AVG_PRICE = 400` - Minimum average price for the last 20 item purchases in cents. Items with a lower price will not be added to the skin database.
- `MAX_AVG_PRICE = 3500` - Maximum average price for the last 20 item purchases in cents. Items with a higher price will not be added to the skin database.
- `FREQUENCY = True` - Enable high-frequency trading algorithm. If set to True, the `PROFIT_PERCENT = 6` parameter should be specified or less, and the `GOOD_POINTS_PERCENT = 50` parameter should be higher or equal.
- `MIN_PRICE = 300` - Minimum average price. Orders will not be placed below this price.
- `MAX_PRICE = 3000` - Maximum average price. Orders will not be placed above this price.

- `PROFIT_PERCENT = 7` - Minimum profit we want to get if we buy an item at the price of the current top order.
- `GOOD_POINTS_PERCENT = 50` - Percentage of points in the last 20 sales history that match the `PROFIT_PERCENT = 7` parameter. In this case, if less than 50% of the points were sold with a profit of less than 7%, an order will not be placed for such an item.
- `AVG_PRICE_COUNT = 7` - Calculate the average price for the last 7 sales to estimate the expected profit.
- `ALL_SALES = 100` - Minimum number of sales for the entire period, below which an order will not be placed.
- `DAYS_COUNT = 20` - At least `SALE_COUNT = 15` sales in `DAYS_COUNT = 20` days. Popularity selection.
- `SALE_COUNT = 15` - At least `SALE_COUNT = 15` sales in `DAYS_COUNT = 20` days. Popularity selection.
- `LAST_SALE = 2` - The last sale no later than 2 days ago.
- `FIRST_SALE = 15` - The first purchase no later than 15 days ago.

- `MAX_COUNT_SELL_OFFERS = 30` - Maximum number of items listed for sale. Orders will not be placed if the number exceeds 30.

- `BOOST_PERCENT = 24` - Remove up to 3 points that are above the average price by 24%.
- `BOOST_POINTS = 3` - Remove up to 3 points that are above the average price by 24%.

- `MAX_THRESHOLD = 1` - Maximum price increase by 1% from the current order. Maximum increase in our order price from the current top order price.
- `MIN_THRESHOLD = 3` - Maximum price decrease for our order from the current one. These set the price change limits for the order.

### SellParams - Parameters for placing items for sale
- `MIN_PERCENT = 4` - Minimum profit percentage.
- `MAX_PERCENT = 12` - Maximum profit percentage.
