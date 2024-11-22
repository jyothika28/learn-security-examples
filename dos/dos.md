# Denial-of-Service (DoS)

This example demonstrates DoS vulnerabilities and how they can be exploited.

## Steps to reproduce

1. Install all dependencies

    `$ npm install`

2. Ignore if you have already done this once. Insert test data in the MongoDB database. Make sure the mongod is up and running by typing the `mongosh` command in the termainal. If mongod process is up then you will see that the connection was successful. Command to insert test data:

    `$ npx ts-node insert-test-users.ts`

This will create a database in MongoDB called __infodisclosure__. Verify its presence by connecting with mongosh and running the command `show dbs;`.

2. Start the **insecure.ts** server

    `$ npx ts-node insecure.ts`

3. In the browser, pretend to be a hacker and type a malicious request

    ```
        http://localhost:3000/userinfo?id[$ne]=
    ```

4. Do you see the server crashing?
Yes

## For you to do

Answer the following:

1. Briefly explain the potential vulnerabilities in **insecure.ts** that can lead to a DoS attack.
    - The id parameter is directly used in the database query without validation or sanitization. Attackers can send malformed or malicious inputs that crash the application, such as id[$ne]=.
    - The endpoint does not restrict the frequency or number of requests from a single IP. This opens the application to flooding attacks, where an attacker sends a high volume of requests to overload the server.
    - The query parameter is passed directly into the database query. MongoDB's flexible query language allows crafted inputs like $ne to exploit the system, potentially leading to resource exhaustion.
2. Briefly explain how a malicious attacker can exploit them.
    - By sending a request like http://localhost:3000/userinfo?id[$ne]=, the attacker uses MongoDB’s $ne operator to create a query that matches all documents. This can overload the database as it attempts to process a broad or invalid query.
    - The attacker sends multiple requests in rapid succession without any restriction, leading to server resource exhaustion and denial of service for legitimate users.
    - Invalid or malformed inputs can lead to errors that crash the server if not properly handled, making the service unavailable.
3. Briefly explain the defensive techniques used in **secure.ts** to prevent the DoS vulnerability?
    - Rate Limiting: The application limits the number of requests from a single IP within a specified time window using express-rate-limit.
    - A try-catch block ensures that invalid inputs or query errors are caught and logged, preventing crashes.
    - Although not explicitly shown in this secure version, a good practice would be to validate and sanitize the id parameter to ensure it is a valid MongoDB ObjectId. This prevents injection attacks and malformed input from being processed by the database.