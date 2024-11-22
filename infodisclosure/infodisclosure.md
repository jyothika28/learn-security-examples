# Tampering

This example demonstrates information disclosure by injecting malicious query objects to a NoSQL database.

## Steps to reproduce

1. Install all dependencies

    `$ npm install`

2. Insert test data in the MongoDB database. Make sure the mongod is up and running by typing the `mongosh` command in the termainal. If mongod process is up then you will see that the connection was successful. Command to insert test data:

    `$ npx ts-node insert-test-users.ts`

This will create a database in MongoDB called __infodisclosure__. Verify its presence by connecting with mongosh and running the command `show dbs;`.

2. Start the **insecure.ts** server

    `$ npx ts-node insecure.ts`

3. In the browser, pretend to be a hacker and type a malicious request

    ```
        http://localhost:3000/userinfo?username[$ne]=
    ```

4. Do you see user information being displayed despite the malicious request not having a valid username in the request?

## For you to do

Answer the following:

1. Briefly explain the potential vulnerabilities in **insecure.ts**
    Potential Vulnerabilities in insecure.ts:
    1) NoSQL Injection: The query to the database directly uses user-supplied input (username) without any sanitization or validation. This allows attackers to craft malicious queries that manipulate the database query logic.
    2) Improper Input Validation: The username parameter is not checked for type or content, making it vulnerable to injection attacks.
    3) Errors in the database query (e.g invalid or malformed inputs) might be exposed to the user, which could provide clues for further attacks.

2. Briefly explain how a malicious attacker can exploit them.
    A malicious attacker can exploit the NoSQL injection vulnerability to retrieve sensitive data from the database. For example:

    By sending the request http://localhost:3000/userinfo?username[$ne]=, the attacker leverages MongoDB’s $ne operator to bypass the condition in findOne. Since $ne means "not equal to," the query matches all documents where username is not equal to an empty string.
    As a result, the query returns sensitive user data without requiring a valid username, thus disclosing confidential information.
3. Briefly explain the defensive techniques used in **secure.ts** to prevent the information disclosure vulnerability?
    1. Input Validation: The code ensures the username parameter is of the expected type (string) and rejects requests with invalid formats. This prevents non-string inputs, like malicious objects, from being processed.
    2. Sanitization: The username input is sanitized to remove potentially dangerous characters, such as $ or { }, which are commonly used in NoSQL injection attacks.
    3. Errors during database queries are caught and logged without exposing detailed information to the user. This limits the attacker's ability to gather insights about the system’s internals.