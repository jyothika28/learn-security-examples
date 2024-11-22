# Tampering

This example demonstrates tampering through script injection.

## Steps to reproduce

1. Install all dependencies

    `npm install`

2. Start the **insecure.ts** server

    `npx ts-node insecure.ts`

3. In the browser, type a potentially malicious script in the name field of the form

    ```
        <script> document.body.innerHTML = "<a href='https://google.com'> Gotcha </a>"</script>
    ```

4. Do you see the potentially malicious hyperlink being injected into the form?

## For you to do

Answer the following:

1. Briefly explain the potential vulnerabilities in **insecure.ts**
    In insecure.ts, the server does not sanitize user input before storing it in the session or displaying it back on the page. This allows malicious scripts entered in the form to be injected into the HTML response, leading to Cross-Site Scripting (XSS) attacks. An attacker can input harmful JavaScript code, which is then executed in the victim's browser when the page loads.
2. Briefly explain how a malicious attacker can exploit them.
    An attacker can exploit the vulnerability by:
    Entering a malicious script (e.g., <script>alert('Hacked!')</script>) in the name field of the form.
    This script is stored in the session and later displayed on the page without validation.
    When the page loads, the browser executes the malicious script, potentially stealing cookies, redirecting the user, or displaying fake content.
    For example:
    Submitting <script>document.body.innerHTML="<a href='https://malicious-site.com'>Click me</a>"</script> could redirect users to a malicious site.
3. Briefly explain why **secure.ts** does not have the same vulnerabilties?
    secure.ts addresses the vulnerability by using the escapeHTML function to sanitize user inputs. This function:
    Replaces special characters (<, >, ", ', &) with their HTML-encoded equivalents (e.g., < becomes &lt;).
    Prevents injected scripts from being interpreted as code by the browser.
    As a result:
    Malicious inputs like <script>alert('Hacked!')</script> are displayed as plain text (&lt;script&gt;alert('Hacked!')&lt;/script&gt;) rather than being executed.
    This ensures the app is safe from XSS attacks.