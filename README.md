# SQL Injection

## Learning Goals

- Explain what a SQL Injection is.
- Learn ways to prevent a SQL Injection.

## What is a SQL Injection?

One of the most common attacks on applications is something called a **SQL**
**Injection**. This is an attack that consists of inserting or "injecting" a
malicious input into the application, which causes an unintended action. Since
this type of attack is so common, it is important that applications guard
themselves from this vulnerability.

But we may be thinking, how does one even perform a SQL injection? Take a look
at this link and follow along with the prompts to see how easily we can access
information from this fake online bank:
[SQL Injection Exercise by Hacksplaining](https://www.hacksplaining.com/exercises/sql-injection).

After following the link to see how a hacker might access an online bank
account, we might be thinking just how bad that could be in real life. In fact,
when a SQL injection is successful, it can exploit sensitive data from the
database (as we saw, we were able to see a customer's account) and even modify
the database data. This means, a hacker could delete data or even drop database
tables, corrupting the entire database. The hacker could also insert malicious
data that causes unexpected behaviors in an application.

## How to Prevent a SQL Injection

Guarding our application against SQL injections is very important. So now that
we know what a SQL injection is, how one could perform a SQL injection, and the
consequences... how do we prevent these attacks?

1. We want to make sure we are using the `PreparedStatement` if using JDBC, as
   we saw in earlier modules. We also want to make sure our parameterized
   statements are passed in a safe manner. For example:

    ```java
    public static Employee getEmployee(Connection connection, String email) {
        String selectStatement = "SELECT * FROM employee WHERE email = ?";
        try (
                // Create a PreparedStatement for sending parameterized SQL statements
                PreparedStatement preparedStmt = connection.prepareStatement(selectStatement);
            ) {
            // Bind email value into the statement at parameter index 1
            preparedStmt.setString(1, email);
        
            ResultSet results = preparedStmnt.executeQuery(selectStatement);
        
            // do something with the results to return an Employee
        }
    }
    ```

    The above code is the safe way to handle parameterized statements. The
    dangerous way would be something like this:

    ```java
    public static Employee getEmployee(Connection connection, String email) {
        try (
                Statement stmt = connection.createStatement();
            ) {
            String selectStatement = "SELECT * FROM employee WHERE email = '" + email + "'";
        
            ResultSet results = stmt.executeQuery(selectStatement);
        
            // do something with the results to return an Employee
        }
    }
    ```

    We should always use parameterized statements when possible to protect us
    from SQL injection attacks.

2. Use ORM. Object-Relational Mapping solves the impedance mismatch by defining
   how an object and its properties are related to one or more tables in the
   database. We can then map our Java objects to a table in a database and vice
   versa. This will also help generate the SQL to insert, update, and delete
   data when the application changes object state, and facilitates the querying
   of objects from the database. While this may be review we want to reiterate
   how we do not have to write our own SQL queries with ORM; this then is a
   safer route from SQL injections as they are usually caused when we start
   writing SQL statements that concatenate `String` objects.
3. "Sanitize the inputs." What we mean by that is we want to validate the user
   input. For example, if a field should not be `null` or empty, make sure we
   check for that. We have seen how we can do so using the Validation dependency
   in Spring. We may also want to check for certain patterns, like a password
   having at least 8 characters in length and also an uppercase, lowercase, and
   a numerical integer.

## Conclusion

This lesson was to provide a brief introductory to SQL injections. We have
explained what a SQL injection is, how to perform one, and the consequences of
this attack on an application. It is imperative that websites and other web
applications be on the defense for these types of attacks as they are the most
common attack that can cause grave damage to a company's reputation. We have
listed a few ways to guard against these types of attacks.

## Resources

- [Hacksplaining: Protecting Against SQL Injection](https://www.hacksplaining.com/prevention/sql-injection)
- [OWASP: SQL Injection](https://owasp.org/www-community/attacks/SQL_Injection)
