# Commands
Here is a full list of commands for the [companies](/src/mech/economy/companies.md) portion of the [economy](/src/mech/economy.md) system. 

## General Commands
   
General commands for managing a company:
   
- `/company register [company name] [ticker symbol] [optional: # of shares]`: Registers a new company with a ticker symbol. Also takes a optional argument that changes the number of shares from the default (100,000).
   
## Stock Market Commands

To help players keep track of the stock market, we can provide them with a few more commands:

- `/company stocks [ticker symbol]`: Shows the current price and volume of the specified stock.
- `/company stocks`: Shows a list of all available stocks, their prices, and their volumes.
- `/company myshares`: Shows a list of all the stocks that the player owns and their current value.