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

How can we not do this? We need to be able to share passwords over Google
Sheets, but not as plain text.

> Encrypt the password with a shared secret.

If we can use a shared secret to encrypt the password and save the encrypted
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

# Let's get started

### Step 1 - Create a Google Sheet

This might be the easiest of all. Just create a new Google Sheet. Head to
[sheets.google.com](https://sheets.google.com) and start a new blank speadsheet.

![Create a new Google Sheet](/assets/uploads/posts/pwd-manager/create_new_sheet.png)

### Step 2 - Add header row

Create the header row for the columns. The headings should be:

- Name
- URL
- Username
- Password

These columns in each row will be managed by our script. You can also
make the header row bold and freeze it.

You can name the sheet as you wish.

![Google Sheet headers](/assets/uploads/posts/pwd-manager/sheet_headers.png)

### Step 3 - Open the script editor

Next step is to open the scipt editor where we will write the code 
required for our tool.

Click on `Tools -> Script Editor` on the menubar. This will open up
the script editor in a new tab.

![Google Script Editor](/assets/uploads/posts/pwd-manager/scripts_home.png)

Give the script an appropriate name. This script is linked to the
sheet now. All changes saved here will be available later also.

### Step 4 - Create files

I have open sourced the code required for the tool at [`shahidhk/google-sheets-password-manager`](https://github.com/shahidhk/google-sheets-password-manager).
You can copy the script files one by one to the script editor window.

`Code.gs` is the main Apps script file. All functions that interact with
the Google Sheets are written here. You can get it from __[`Code.gs`](https://github.com/shahidhk/google-sheets-password-manager/blob/master/Code.gs)__
on GitHub and paste it into the editor window. You can remove the function that
is already present in the editor. Once pasted, press <kbd>Ctrl+S</kbd> to save.


![Code.gs](/assets/uploads/posts/pwd-manager/code_gs.png)

Next, create a new file called `newEntry.html` by going to the
`File -> New -> HTML File` menu. Remove the existing HTML add code from
__[`newEntry.html`](https://github.com/shahidhk/google-sheets-password-manager/blob/master/newEntry.html)__. Press <kbd>Ctrl+S</kbd> to save.

![newEntry.html](/assets/uploads/posts/pwd-manager/new_entry_html.png)

Similarly, create new files and copy contents from
__[`decryptPassword.html`](https://github.com/shahidhk/google-sheets-password-manager/blob/master/decryptPassword.html)__ and
__[`styles.html`](https://github.com/shahidhk/google-sheets-password-manager/blob/master/styles.html)__.

We have the following files in our script editor now:

1. `Code.gs`
2. `newEntry.html`
3. `decryptPassword.html`
4. `styles.html`

### Step 5 - Run the script

Open `Code.gs` and use the `Select function` dropdown on the toolbar to select
the `onOpen` function. This function is executed everytime the the Google Sheet
is opened. Click on the `Run` button (play icon) next to the dropdown to run
the function.

![run function](/assets/uploads/posts/pwd-manager/run_function.png)

You will now be prompted to grant permissions for the script to access the sheet.
If you get a "This app isn't verified" screen, you can safely over-ride by
clicking the "Advanced" link at the bottom as you're the developer for the app
(since you just copy pasted the code).

_I'm not saying copy pasting the code is safe, I am assuming you can go through
couple of lines of code and figure out if it's doing something fishy._

Once you allow the permission request and the function is ran (you might need
to click the Run button agian), a new menu item will appear on the Sheet.

![password manager menu](/assets/uploads/posts/pwd-manager/pwd_mgr_menu.png)

### Step 6 - Add a password

Let's save a password now. Click on `Password Manager -> Add new password`.
This will open a new dialog box with several input fields.