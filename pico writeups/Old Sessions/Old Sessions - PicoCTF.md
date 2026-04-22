# Challenge 
<img width="966" height="691" alt="0-des" src="https://github.com/user-attachments/assets/3935d468-4ea3-476e-bceb-da752f18d2c8" />

## Description

Proper session timeout controls are critical for securing user accounts. If a user logs in on a public or shared computer but doesn’t explicitly log out (instead simply closing the browser tab), and session expiration dates are misconfigured, the session may remain active indefinitely.This then allows an attacker using the same browser later to access the user’s account without needing credentials, exploiting the fact that sessions never expire and remain authenticated.



# 1 - Exploring the site

The challenge provides the following URL:
http://dolphin-cove.picoctf.net:56101

When the machine started running, I began fuzzing the website using the **ffuf** tool. While the fuzzing process was running, I manually explored the website and found a login page.
<img width="1010" height="451" alt="03-ffuf-n" src="https://github.com/user-attachments/assets/d87f126d-c20b-4533-bea4-80c89fb5bed8" />

I attempted an SQL injection attack, but it turned out that the website was not vulnerable to SQL injection.
<img width="1919" height="938" alt="1-login" src="https://github.com/user-attachments/assets/efb802a6-4bde-4fb3-8932-9393cc8fd936" />

At that point, I noticed that the fuzzing results were not returning any useful output, so I suspected that the application might require an authenticated session (cookies).
To test this, I created a test account named "Ali" and logged in successfully. Then I navigated to the browser’s Inspect tool, opened the Storage section, and extracted the session cookie.
<img width="1919" height="938" alt="2-regestration" src="https://github.com/user-attachments/assets/0acfbfd5-1ff4-4f5b-8ba6-334817833114" />
<img width="1920" height="944" alt="7-cook" src="https://github.com/user-attachments/assets/569d4dfb-26db-45a2-97ef-68c015672e03" />

I used this cookie in my fuzzing process while continuing manual exploration of the website.
<img width="1920" height="567" alt="03-fuff" src="https://github.com/user-attachments/assets/69ebaed9-d415-47c6-b579-bd1bcc2a8191" />

While browsing the website, one of the comments caught my attention. It mentioned that someone had discovered something unusual — a directory named **/sessions**.
<img width="1919" height="938" alt="3-comint" src="https://github.com/user-attachments/assets/09a79847-5956-48d7-9f10-2f1d78d341d5" />

# 2 - Discovering the Session Endpoint
This prompted me to investigate it further. Upon visiting the directory, I found that session cookies were being stored there. While examining its contents, I was able to locate the **admin session cookie**.
<img width="1020" height="135" alt="4-session" src="https://github.com/user-attachments/assets/78e4ec09-74ce-484b-a0fc-838536be9fa4" />
<img width="1920" height="946" alt="5-sessionn2" src="https://github.com/user-attachments/assets/f9b70a1d-872f-42e9-a9cc-01f3e809a064" />

# 3 - Hijacking the Admin Session
I copied the admin session cookie and returned to the website. I opened the browser’s Inspect tool, navigated to the Storage section, and replaced my current session cookie with the admin session cookie.(This attack technique is known as **Session Hijacking**.)
<img width="1920" height="944" alt="7-cook" src="https://github.com/user-attachments/assets/ae80d77d-cfde-4756-8dfc-dec0939cee01" />

# 4 - Retrieving the Flag
After refreshing the page, I successfully gained access to the admin panel and was able to retrieve the flag.
<img width="1920" height="944" alt="8-finaly" src="https://github.com/user-attachments/assets/4382eced-4ad0-4435-a2f5-bf84eb68b938" />
```flag
picoCTF{s3t_s3ss10n_3xp1rat10n5_7139c037}
```



# Root Cause Analysis

The vulnerability stems from improper session management and insecure design decisions within the application. Several critical issues collectively contribute to the successful session hijacking attack:

## Persistent Sessions Without Expiry

The application fails to enforce session expiration, allowing session tokens to remain valid for an unlimited period. This significantly increases the attack window, as compromised sessions can be reused at any time without restriction.

## Insecure Exposure of Session Data

A sensitive endpoint (`/sessions`) is improperly exposed, enabling unauthorized users to access active session identifiers. This represents a direct information disclosure vulnerability, as session tokens should never be publicly accessible.

## Weak Authentication Model

The application relies solely on client-side session cookies for authentication. There is no additional server-side verification (such as IP binding, user-agent validation, or token rotation), making it trivial to impersonate other users once a valid session token is obtained.

# Security Impact

These vulnerabilities allow an attacker to perform **session hijacking** with minimal effort. By obtaining a valid session token (e.g., from the exposed endpoint), an attacker can impersonate privileged users — including administrators — without needing to bypass authentication mechanisms such as login forms or credentials.

# Recommendations

To mitigate these issues, the following security controls should be implemented:

- Enforce **strict session lifetime policies** with automatic expiration and inactivity timeouts
- Restrict access to sensitive endpoints and ensure **session data is never exposed publicly**
- Implement **session regeneration** after authentication and privilege changes
- Apply **server-side validation mechanisms** (e.g., binding sessions to user context or rotating tokens)
- Use secure cookie attributes such as `HttpOnly`, `Secure`, and `SameSite`


# Conclusion

This challenge demonstrates how poor session management can completely undermine an application’s authentication system. Even in the absence of traditional vulnerabilities like SQL injection, attackers can still achieve full account takeover through exposed and improperly handled session tokens.
