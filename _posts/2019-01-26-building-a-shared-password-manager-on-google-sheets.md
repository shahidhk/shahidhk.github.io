---
layout:     post
title:      "Building a shared password manager on Google Sheets"
date:       2019-01-26 00:00:00
summary:    "Using Google Apps script, Google Sheets can be turned into a shared password manager, especially helpful to store passwords for shared accounts in a team"
categories: blog
thumbnail:  key
---

# Introduction

Passowrd managers have beome an inevitable part of our online lives. The only
way not to reuse a pasword is to ue a password manager. I am a big fan of
[LastPass](https://lastpass.com/). Have been using for the past couple of years
and it has been great. 

While working with a team, there might be accounts that are shared, and what
people typically do is to have a pattern to genereate passwords for these
accounts. For e.g., `contact@company.com` might have a password like
`#SecretCONTACTPassw0rd123$%^` and `support@company.com` might
have `#SecretSUPPORTPassw0rd123$%^`. This is a super insecure
way for setting passwords and most times people do this because they don't
want to get in to the mess of sharing passwords over a password manager.

Sharing over LastPass needs the user to know the other person's email address
and the receiving person need to have a LastPass account and have it installed.
While I am a strong advocate of using password managers, I understand that
everyone in a team might not be comfortable in doing so. Sure, they need to 
be educated and should be made aware of the threats of not using a manager.
But that is not the only difficulty, the other person might be using a
different password manager. Now, you'll have to resort to sharing plain-text
password over insecure channels.

## Google Sheets

Enter Google Sheets. Like all the other Google Suite of applications, Sheets
can be shared across a team. If your organisation is a GSuite customer, the
visibility can be set accordingly such that everyone in the organisation can 
view and even edit the sheet. Sheets looks like an ideal candidate to share 
passwords. And believe me when I say this, there are teams that share passwords
on Google Sheets as plain text.

How can we not do this? We need to be able to share passwords over Google Sheets, but not as plain text.

## Encrypt password with a shared secret

> Encrypt the password with a shared secret.

If we can use a shared secret and encrypt the password and save the encrypted
text in the sheet, anyone who is looking for that password can just decrypt
it using the shared secret. But everyone need to have the encrypt-decrypt tools
installed. This takes us back to the original problem - everyone need to have
the password manager installed.

What if we could do this in Google Sheets itself?

## Google Apps Script

Google released [Apps Script](https://developers.google.com/apps-script/) in
2009 as scripting-language for light-weight application development platform
which can integrate most of Google applications together. It is based on
JavaScript and runs on the Google Cloud. We'll use Google Apps Script to build
our encryption-decryption tool into Google Sheets.

# Let's get our hands dirty

WIP