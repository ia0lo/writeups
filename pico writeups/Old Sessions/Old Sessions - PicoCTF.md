# Challenge 
![[0-des.png]]
## Description

Proper session timeout controls are critical for securing user accounts. If a user logs in on a public or shared computer but doesn’t explicitly log out (instead simply closing the browser tab), and session expiration dates are misconfigured, the session may remain active indefinitely.This then allows an attacker using the same browser later to access the user’s account without needing credentials, exploiting the fact that sessions never expire and remain authenticated.






# 1 - Exploring the site

The challenge provides the following URL:
http://dolphin-cove.picoctf.net:56101

When the machine started running, I began fuzzing the website using the **ffuf** tool. While the fuzzing process was running, I manually explored the website and found a login page.

![[03-ffuf-n.png]]

I attempted an SQL injection attack, but it turned out that the website was not vulnerable to SQL injection.
![[1-login.png]]

At that point, I noticed that the fuzzing results were not returning any useful output, so I suspected that the application might require an authenticated session (cookies).
To test this, I created a test account named "Ali" and logged in successfully. Then I navigated to the browser’s Inspect tool, opened the Storage section, and extracted the session cookie.
![[2-regestration.png]]
![[7-cook.png|601]]

I used this cookie in my fuzzing process while continuing manual exploration of the website.
![[03-fuff.png]]

While browsing the website, one of the comments caught my attention. It mentioned that someone had discovered something unusual — a directory named **/sessions**.
![[3-comint.png]]

# 2 - Discovering the Session Endpoint
This prompted me to investigate it further. Upon visiting the directory, I found that session cookies were being stored there. While examining its contents, I was able to locate the **admin session cookie**.
![[4-session.png]]
![[5-sessionn2.png]]

# 3 - Hijacking the Admin Session
I copied the admin session cookie and returned to the website. I opened the browser’s Inspect tool, navigated to the Storage section, and replaced my current session cookie with the admin session cookie.(This attack technique is known as **Session Hijacking**.)
![[7-cook.png]]

# 4 - Retrieving the Flag
After refreshing the page, I successfully gained access to the admin panel and was able to retrieve the flag.
![[8-finaly.png]]
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