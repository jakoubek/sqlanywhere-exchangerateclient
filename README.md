# Exchange rate client for SQL Anywhere

**Retrieve the exchange rate for a currency from inside a SQL Anywhere database**

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


## Installation

1. Create a new database (using `dbinit`) or reuse an existing one
2. Connect with iSQL to the database
3. Execute the script `exchangerateclient.sql`
4. Edit API key and currency symbol in the procedure *fetch_exchange_rate*

## Use

- call procedure *fetch_exchange_rate* manually in iSQL and check the new record in table *exchange_rate*
- enable the event *ExchangeRateFetcher* - it will run daily at 14:00 (2pm)
- optionally change the schedule for that event


## API response from currencylayer

If the API call to the currencylayer service was ok and you had a valid API key the response looks like this:

```json
{
    "success": true,
    "terms": "https://currencylayer.com/terms",
    "privacy": "https://currencylayer.com/privacy",
    "timestamp": 1555653247,
    "source": "USD",
    "quotes": {
        "USDCHF": 1.014845
    }
}
```

Otherwise you'll get:

```json
{
    "success": false,
    "error": {
        "code": 101,
        "type": "invalid_access_key",
        "info": "You have not supplied a valid API Access Key. [Technical Support: support@apilayer.com]"
    }
}
```
