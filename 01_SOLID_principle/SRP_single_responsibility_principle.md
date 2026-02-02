# Definition 
A class should only have **one job, one responsibility, and one purpose.**

If a class takes more than one responsibility, it becomes coupled. This means that if one responsibility changes, the other responsibilities may also be affected, leading to a ripple effect of changes throughout the codebase. 

# Real Life Example 
Restraunt requires : Chef , Cleaner, Waiter, Manager. All there roles/ task are different, should be handled by different person **focus on their specific responsibility, leading to better results overall.** 

# SRP Example 
Writing a **complier**, consist of smaller classes 
, each with a single responsibility:

    1) **DriverCodeGenerator** - Responsible for adding driver code.
    2) **SyntaxChecker** - Responsible for performing syntax checks.
    3) **TestRunner** - Responsible for running code with test cases.
    4) **DatabaseManager** - Responsible for storing output in the database.
    5) **UserOutputHandler** - Responsible for returning output to the user.


Another class named **Coordinator** can be added to coordinate between all these classes/modules.

# Advantages of SRP
    1) Easy to **mantain** and update the single responsibility
    2) Readibility : focused smaller code, easier to read and understand. 
    3) Reusability : Classes can be reused in different context
    4) Facilites Unit testing.
    5) Lower risk in changing:  Any changes done will only impact this class, other parts of code are uneffected.  
    
# Common Mistakes When Violating SRP

There are a few common mistakes that developers make when violating the Single Responsibility Principle (SRP). Here are some examples:

    1) **Mixing Database Logic with Business Logic**: Putting both data access (e.g., SQL, JDBC) and core business rules in the same class. This makes it hard to change the database layer without affecting business logic.
    2) **Coupling UI Code with Business Logic**: Embedding application logic directly in the UI layer. This makes it tedious to change the UI without affecting the underlying logic.

# "Is SRP just for classes?"

The answer is no. SRP can be applied to methods, modules, microservices and even entire systems. The key is to ensure that each component has a single responsibility and that changes in one area do not affect others unnecessarily.
Hence, SRP is not just for classes. It's a mindset you can apply from the smallest method to the largest system design. 
