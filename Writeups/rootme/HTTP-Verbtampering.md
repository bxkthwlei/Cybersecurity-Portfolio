Root-Me Write-up: HTTP - Verb tampering
Challenge: HTTP - Verb tampering

Category: Web-Server

Platform: Root-Me

Points: 15

Difficulty: Easy

Tools Used: Burp Suite

1. The Goal
The goal is simple: bypass the login security on the web server and get the flag. When you start the challenge, a login box (HTTP Basic Authentication) pops up. It locks you out because you do not have a username or password.

2. The Vulnerability (What is happening?)
This challenge is about a classic flaw called HTTP Verb Tampering. This happens when the server's security rules are too specific.

When developers set up security (like in a .htaccess file), they sometimes try to protect a folder by naming only the specific HTTP methods they want to block. For example, they might write:

Apache
<Limit GET>
    Require valid-user
</Limit>
The big mistake here is that the server only checks for a password if the request is a GET request. If an attacker sends a request using a different HTTP method—like POST, HEAD, OPTIONS, or PUT—the security rule does not trigger. The server completely skips the login check and shows the page anyway.

3. Step-by-Step Solution
Step 1: Intercepting the Traffic
First, I opened Burp Suite, turned on Intercept, and refreshed the challenge page. As expected, the original request captured by Burp Suite was a normal GET request:

HTTP
GET /en/Challenges/Web-Server/HTTP-verb-tampering HTTP/1.1
Host: challenge01.root-me.org
...
Step 2: Changing the HTTP Method
Since the server only checks GET requests, I decided to change the HTTP method. I deleted GET and replaced it with OPTIONS:

HTTP
OPTIONS /en/Challenges/Web-Server/HTTP-verb-tampering HTTP/1.1
Host: challenge01.root-me.org
...
Step 3: Getting the Flag
I clicked Forward to send the modified request to the server. Because the server's security setup did not include the OPTIONS method, it let the request pass without asking for a password.

The server loaded the page and sent the Flag directly back in the response text.
