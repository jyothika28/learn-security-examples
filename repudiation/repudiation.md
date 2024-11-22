# Repudiation

The example demonstrates a vulnerability that can lead to repudiation by malicious users attempting to access the services provided by a server.

## Steps to reproduce

1. Install all dependencies

    `$ npm install`

2. Run the server __insecure.ts__.

3. Pretend to be a malicous user and interact with the services by sending requests from the browser.

4. Do you think your actions can be repudiated?

## For you to do

1. Briefly explain the vulnerability.
    The application in insecure.ts does not log user interactions or enforce authentication. This leads to the following issues:

    A malicious user can perform actions (e.g., send messages or retrieve all messages) without authentication. Since the server does not log requests or associate actions with authenticated identities, it is impossible to trace or hold users accountable for their actions.
    Lack of audit trails makes it difficult to investigate unauthorized activities or determine the source of malicious actions.
2. Briefly explain why the vulnerability is addressed in __secure.ts__.
    The secure.ts implementation mitigates repudiation vulnerabilities by:
    Logging Requests: It logs all incoming requests, including metadata such as the timestamp, request method, endpoint, and client IP address, into a persistent server.log file. This provides an audit trail for all user activities.
    Authentication: It introduces a placeholder for an authentication mechanism to ensure only authorized users can access the /get-messages endpoint. Unauthorized access is explicitly denied, and attempts are logged.
    Error Logging: Any errors that occur are also logged with details about the timestamp and the error message, ensuring visibility into issues that arise.
3. Which design pattern is used in the secure version to address the vulnerability? Briefly explain how it works?
    The Interceptor Pattern is used in the secure version.
    How It Works?
    Middleware functions act as interceptors that process requests before they reach the actual route handlers.
    Logging Middleware: Logs details about incoming requests to the server.log file.
    Authentication Middleware: Validates user credentials before allowing access to sensitive endpoints.
    Error-Handling Middleware: Catches and logs errors during request processing, preventing them from crashing the application.
    By using this pattern, the secure implementation systematically applies security and traceability features across all requests, ensuring consistency and reducing vulnerabilities.