# Privilege Escalation

The example demonstrates a privilege escalation vulnerability and how to exploit it.

## Steps to reproduce

1. Install all dependencies

    `$ npm install`

2. Start the **insecure.ts** server

    `$ npx ts-node insecure.ts`

3. In the browser, send a GET request

    ```
        http://localhost:3000/send-form
    ```

4. Try different UserIds and see which one gives you authorized access to change the role of that user.

## For you to do

Answer the following:

1. Briefly explain the potential vulnerabilities in **insecure.ts**
    - The application does not verify if the user making the request is logged in or authorized to perform the action.
    - The check for administrative privileges relies solely on the user.role provided in the request, which can be bypassed by manipulating the data.
    - The application does not track user sessions, making it impossible to reliably identify the requesting user.
    - The application allows any user with administrative privileges to modify roles without verifying the identity or permissions of the target user.
2. Briefly explain how a malicious attacker can exploit them.
    -  A malicious user can send a crafted request with a valid userId for an administrator and change their own or others roles.
    - The attacker can escalate their role to admin by exploiting the lack of proper authorization checks.
    - The user can manipulate the userId or newRole fields in the request payload to gain unauthorized privileges, such as modifying the role of other users or adding admin privileges to themselves.
3. Briefly explain the defensive techniques used in **secure.ts** to prevent the privilege escalation vulnerability?
    - The server uses express-session to establish and maintain user sessions, ensuring that only authenticated users can make role modifications.
    -  It checks the req.session.userId to ensure the request is initiated by a logged-in user. Unauthorized users receive a 401 status code.
    - The cookie settings ensure that the session data is secure with httpOnly (preventing client-side access) and sameSite (mitigating CSRF attacks).
    - The server responds with appropriate status codes and error messages to prevent information leakage.