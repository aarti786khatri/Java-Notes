
Abstraction: Hiding the internal detail and showing only the functionality to the user.

1. Abstract Classes (Provides 0 to 100% partial abstraction) : Suppose there are account types like saving accounts, current accounts and so on, so some thing are common here like deposit, withdrawal but the calculation of interest is different, so we can create this calculate interest as a abstract method.
2. 
3. Interfaces (Provides 100% complete abstraction) :Imagine you go to a shop and buy items.

in a bill counter, the shop keeper care about only one thing.
The customer paid the Money or not.

The shopkeeper does NOT care about how you pay the money,

UPI

Debit Card

Cash

They only thing is payment paid in successfully.

Here a interface acts like a Rule in billing counter.
It only defines what must be done, not how it should be done.

Different payment methods follow the same rule, but each one works in its own way.

The shopkeeper does not need to change anything in the billing counter.

No matter how the customer pays, the system works the same.

So choice depends on the business usecase.




