# Spoofing

This example demonstrates spoofind through two ways -- Stealing cookies programmatically and cross site request forgery (CSRF).

## Steps to reproduce the vulnerability

1. Install dependencies

    `$ npx install`

2. Start the **insecure.ts** server

    `npx ts-node insecure.ts`

3. Start the malicious server **mal.ts**

    `npx ts-node mal.ts`

4. Open __http://localhost:8000__ in a browser, type a name and Submit.

5. Open the __Application__ tab in the Browser's inspect pane. Find the __Cookies__ under __Storage__. You should see a __connect.sid__ cookie being set.

6. Open the HTML file __mal-steal-cookie.html__ file in the same browser (different tab). Open inspect and view the console.

7. Click the link in the HTML file. Do you see the cookie being stolen in the console?

8. Open the HTML file __mal-csrf.html__ file in the same browser (different tab). What do you see if the user has not logged out of **insecure.ts**? What do you see if the user has logged out? 


## For you to answer

1. Briefly explain the spoofing vulnerability in **insecure.ts**.
    - The session cookies (connect.sid) are not secure. The httpOnly option for cookies is set to false, which allows JavaScript in the browser to access these cookies. This makes it easier for malicious scripts to steal them.
    - The sameSite attribute is not enabled, making the cookies vulnerable to cross-site request forgery (CSRF). This allows attackers to trick a logged-in user into performing unintended actions by sending requests from a malicious website.
2. Briefly explain different ways in which vulnerability can be exploited.
    - Stealing cookies programmatically:
    An attacker can create a malicious page (e.g., mal-steal-cookie.html) that uses JavaScript to access the cookie (connect.sid) and send it to the attacker's server. This allows the attacker to impersonate the user by using the stolen session cookie.

    - Cross-Site Request Forgery (CSRF):
    If a user is logged into the application and visits a malicious site (e.g., mal-csrf.html), the malicious site can send unauthorized requests to the insecure.ts server using the user's session. This could result in actions being performed on the user's behalf without their consent.
3. Briefly explain why **secure.ts** does not have the spoofing vulnerability in **insecure.ts**.
    secure.ts is protected against spoofing vulnerabilities because:
    The cookies are set with httpOnly: true, which prevents them from being accessed by JavaScript in the browser. This blocks cookie theft via malicious scripts.

    The sameSite: true setting ensures that cookies are only sent with requests originating from the same site as the server. This blocks CSRF attacks by preventing cookies from being sent with cross-origin requests.