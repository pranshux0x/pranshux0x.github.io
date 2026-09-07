---
title: Account Takeover on an Eight-Year-Old Public Program
date: 2024-08-14 11:05:04 +0530
description: How an email-verification race condition and a crafted email value combined to enable a developer-account takeover.
categories: [Bug Bounty, Account Takeover]
tags: [account-takeover, bug-bounty, bug-bounty-tips, hacking, vulnerability]
image:
  path: /assets/img/posts/account-takeover-on-8-years-old-public-program/cover.jpeg
  alt: Account Takeover — Using Unicode email fuzzing
---

> This article was originally published on [Medium](https://medium.com/@pranshux0x/account-takeover-on-8-years-old-public-program-c0c0a30cfdd2) on August 14, 2024. The target is represented with example domains.
{: .prompt-info }

This is the story of an account takeover I found on an eight-year-old public bug bounty program.

## Acknowledgements

Thanks to:

- [0xacb](https://x.com/0xacb) for the recollapse tool.
- [PortSwigger](https://x.com/PortSwigger) for its race-condition research and Burp Suite tooling.

## Understanding the application flow

The application had two related websites:

- `www.example.com`, where users could create and access their main accounts.
- `developer.example.com`, where users could sign in with those main accounts.

The developer-site login flow worked like this:

1. Go to `developer.example.com`.
2. Select **Sign in**.
3. Choose an account from the main website.
4. If the main account's email address is unverified, the developer site displays a verification prompt. It sends a link to the email address, and the user must follow that link to verify it.
5. The user is then signed in to `developer.example.com`.

## Bypassing email verification

I tested whether I could bypass this email-verification step. The process was:

1. Create an account on `www.example.com` with the email address `just@gmail.com`.
2. On `developer.example.com`, select **Sign in**, choose that account, and request an email-verification link when the prompt appears.
3. Open the verification link while proxying traffic through Burp Suite. Send the verification request to Repeater, then hold the request.
4. While still proxying traffic, change the main account's email address to `just1@gmail.com`. Send this second email-verification request to Repeater and hold it as well.
5. Create a group in Repeater, move both requests into it, and send the requests in parallel.
6. The developer account is created with `just1@gmail.com` shown as its verified email address.

I reported the email-verification bypass. It was accepted as a P4 issue with a $200 reward.

## Escalating the issue to account takeover

I then considered whether the verification bypass could lead to account takeover. If I could create a developer account that resolved to a victim's email address, I could potentially verify it with the race-condition technique.

Changing my email address directly to `victim@gmail.com` on `www.example.com` failed with a generic error. I therefore fuzzed the victim email address with Unicode and control characters, using a technique that can also be explored with recollapse.

The application accepted this value:

```text
victim@gmail.com%0f
```

On the main website, `victim@gmail.com%0f` and `victim@gmail.com` remained separate accounts; neither account could access the other.

However, when I used `victim@gmail.com%0f` to sign in to `developer.example.com`, the developer site showed its email-verification prompt. I completed that verification using the earlier race-condition bypass.

The result was access to the victim's developer account: the two email values were distinct on the main site but were treated as the same identity in the developer flow.

Account takeover was complete.

If you have a question about this research, you can reach me on [X](https://x.com/pranshux0x).
