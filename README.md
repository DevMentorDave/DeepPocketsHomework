# * schema was not updated to fit explanation yet. Work in progress *
# Here is my database schema.

You can find the diagram [here](https://dbdiagram.io/d/DP-660b4a1a37b7e33fd740cea8)

![schema](schema.png)

# Explanation

## The whole picture. 
I see a budget as a higher layer of abstraction on top of money. A budget can be surplus (money left on account) or deficit (credit spent).
Budget is in fact a groupe af accounts and other budgets.
There are different types of budget (main, education, goal, etc.)

Money (accounts) and transactions are the base level of abstraction. Accounts only can store money. 
Each spending or earning is a transaction. Any transaction before it is done should be planned (`scheduled_transaction` table) within a budget. On planning step accounts that effected by the transaction are set. 
So a transaction itself belongs to a particular account only. A plan of a transaction belongs to a budget. 
Earning transactions belong to an account thar money go to.
Spending transactions belong to an account that money go from.
A transfer between accounts is a spending.
Transaction can be changed, info about changes is kept in dedicated table.

Each budget can have some accounts or use an account of a higher level budget. For example I have MAIN budget with account CHECKING ($1000 comes automatically weekly) and nested budgets FUN and TRANSPORT. FUN budget has an account in it where I transfer $100 automatically as my paycheck comes. When I spend money within FUN budget I use it's account by default. But if there is not enough money for something I can transfer more from higher level budget's account CHECKING manually (it keeps FUN surplus). Or I can spend money from higher level budget's account CHECKING. In that case FUN becomes deficit (borrows money from CHECKING). To make balance positive again I should transfer money to CHECKING back. If it is time for auto transaction of $100 from CHECKING to FUN and FUN ows $150 CHECKING the app should catch it and reduce amount FUN ows by $100 (by doing 2 auto transactions I think). TRANSPORT budget does not have any accounts. It uses higher level budget's account CHECKING with no limits directly.

## Tables (in progress)
Account keeps money and can belong to only budget.
There are different types of accounts (cash, credit, checking)

Scheduled transaction is planned automatic (weekly, biweekly, etc.) transaction
or single planned transaction.

There are `account_log` and `budget_log` to keep track of changes.
Maybe there should be a dedicated table for transferring account between budgets.

