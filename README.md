# Exchange rate client for SQL Anywhere

**Retrieve exchange rate for a currency from inside a SQL Anywhere database**

This example shows how to access a web service from a SQL Anywhere database to retrieve the exchange rate for a currency. It makes use of SQL Anywhere's web client feature.

You'll find just one SQL file with all statements to create the needed database objects. These are:

- one table *exchange_rate* to store the exchange rate for a given date
- one web client function *webclient_get_exchange_rate* to access the web service
- one procedure *fetch_exchange_rate*; it calls the web client function, parses the JSON response and insert or updates the exchange rate in the table
- one event definition *ExchangeRateFetcher* that runs once a day and takes care for the whole process

## Constraints

I'll use the exchange rate API from [currencylayer](https://currencylayer.com/). You have to register with this service and get your API key.

This example fetches the exchange rate for the Swiss franc in USD (symbol: CHF).

Both values, API key and currency symbol, can be edited in the procedure *fetch_exchange_rate*.

In a real use case you will likely want to store those values in a configuration table.


