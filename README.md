# tryhackme-ssrf-notes
Notes and learning from TryHackMe Intro to SSRF room
# TryHackMe — Intro to SSRF

## Overview
In this lab, I learned about **Server-Side Request Forgery (SSRF)**, a web vulnerability that allows an attacker to manipulate a server into making requests to unintended locations. SSRF can expose internal services, sensitive data, or restricted resources that are not normally accessible from outside the network.

Hands-on practice in this room helped me understand how attackers can exploit poorly validated user input to interact with internal systems through the vulnerable application.

---

## What is SSRF?

Server-Side Request Forgery (SSRF) is a vulnerability that occurs when a web application fetches a resource from a **user-controlled URL** without properly validating the input.

Instead of the attacker sending the request directly, the **server sends the request on behalf of the attacker**. Because the request originates from the server, it may bypass firewalls or security controls.

Example:

```
https://website.com/api?url=https://example.com
```

If the application does not validate the input properly, an attacker could modify the request like this:

```
https://website.com/api?url=http://localhost/admin
```

In this case, the server attempts to access its own internal admin panel.

---

## Why SSRF is Dangerous

SSRF vulnerabilities are dangerous because they may allow attackers to:

- Access internal services running on the server
- Interact with private network resources
- Retrieve sensitive data
- Bypass firewall protections
- Potentially escalate attacks depending on the internal service exposed

---

## Common SSRF Targets

Attackers commonly try to access internal resources such as:

```
http://localhost
http://127.0.0.1
http://0.0.0.0
```

These addresses often point to services running inside the server's local network.

---

## Techniques Used in the Lab

During this lab, I practiced several techniques that are commonly used during web security testing:

- Inspecting the page source
- Modifying input values in the browser using Developer Tools
- Using directory traversal to bypass restrictions
- Decoding Base64 encoded data

---

## Directory Traversal Example

The application allowed users to select an avatar image. By inspecting the page source, the avatar value could be modified.

Original value:

```
assets/avatars/1.png
```

Modified value:

```
x/../private
```

The `../` sequence moves up one directory level. As a result, the server interpreted the request as:

```
/private
```

This allowed access to a restricted directory.

---

## Retrieving the Flag

After modifying the avatar value and updating the request, the application returned data from the `/private` directory.

The returned content was encoded in **Base64** within the page source.

Example:

```
data:image/png;base64,ENCODED_DATA
```

By copying the encoded string and decoding it, the hidden flag was revealed.

Example command used in Kali Linux:

```
echo "encoded_string" | base64 -d
```

---

## Key Takeaways

- SSRF vulnerabilities occur when applications trust user-supplied URLs.
- Attackers can force the server to access internal resources.
- Directory traversal techniques can sometimes bypass security restrictions.
- Encoded responses such as Base64 may hide sensitive data.
- Inspecting page source and modifying inputs are important skills in web security testing.

---

## Tools Used

- Browser Developer Tools
- Kali Linux Terminal
- Base64 decoding

---

## Lab Platform

TryHackMe — Intro to SSRF
