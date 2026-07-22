# [atet](https://github.com/atet) / [**_eml_**](https://github.com/atet/eml/blob/main/README.md#atet--eml)

[![.img/eml_logo.png](.img/eml_logo.png)](#nolink)

# CSV Templates to Email Drafts†

Did you always wonder why mail merging in popular office suites are so limited? Well wonder no more! 

Generating bulk email drafts usually means limited mail merging features, paying for a SaaS subscription, or trusting your sensitive contact lists to a third-party server.


 **Simple EML Creator** changes that. 

This simple app will allow you to convert a `*.csv` file of templates, email addresses, and variables to `*.eml` draft emails that are ready to be sent by your favorite email client.

[†This was my Google AI Professional Certificate capstone project.](#other-resources)

----------------------------------------------------------------------------

## Table of Contents

* [0. Requirements](#0-requirements)
* [1. Instructions](#1-instructions)
* [2. Next Steps](#2-next-steps)

### Supplemental

* [Other Resources](#other-resources)
* [Troubleshooting](#troubleshooting)

—</br>

----------------------------------------------------------------------------

## 0. Requirements

This is a **100% client-side, offline-ready** Single-Page Application (SPA): Just open the `eml.html` file in your web browser and take a `*.csv` file with emails and instantly generate `*.eml` email drafts. 

### Why a Single-Page HTML App?

* **Ultimate Privacy:** Your data never leaves your browser. There is no backend, no database, and no API. 
* **Zero Maintenance:** No npm installs, no build steps, and no servers to maintain.
* **Extreme Portability:** The entire application is just one file. You can download the `eml.html` file, put it on a USB drive, and run it on a completely air-gapped computer.

—</br>
[Back to Top](#table-of-contents)

----------------------------------------------------------------------------

## 1. Instructions

### 1.1. Prepare your CSV

Create a spreadsheet and save it as a `*.csv` file (for Microsoft Word, use "`CSV (Comma delimited) (*.csv)`").

* **Required:** You must have a column named "`To`" and multiple emails can be separated by a semicolon ("`;`").
* ***Optional Recipients:*** You can also include "`CC`" and "`BCC`" columns. 
* **Variables:** Any other columns you add (e.g., "`FirstName`", "`InvoiceURL`", "`Department`", etc.) automatically become injectible variables!
* ***Optional Templates***: If you put "`Subject`" and "`Body`" columns in the first row of your CSV, the app will automatically embed your template.
   * To reference variables in your template, use double angle brackets, e.g., "`<<Variable>>`"
   * (In Microsoft Excel) To add newlines in your temple, do not use "`\n`", instead, use `ALT+ENTER` in the cell

Example:

[![.img/fig1_csv_example.jpg](.img/fig1_csv_example.jpg)](#nolink)

### 1.2. Upload & Configure

1.2.1. Open the `eml.html` file in any modern web browser and click on "`Upload File`" or drag-and-drop your CSV file into the upload zone.

1.2.2. Templates:
   * Templates in "`Subject`" and "`Body`" within the `*.csv` file will automatically be loaded
   * If you didn't have templates in the `*.csv` file, under **Template Configuration**, create your template and click the available variables to inject them into your Subject Line or Email Body.
   * Toggle between "`Plain Text`" and "`HTML`" formatting as needed.

### 1.3. Export & Send

1.3.1. Use the **Live Preview** sandbox to verify your generated drafts look correct.

1.3.2. Click **Download .eml** to download the current draft being viewed, or click **Pack Drafts** at the bottom to download a `*.zip` file containing all your draft emails from your `*.csv` file.

1.3.3. Extract the `*.zip` file and double-click any "`.eml`" file. Files will open directly in your native desktop mail client‡ (e.g., Outlook, Apple Mail, Thunderbird) in **Edit/Draft mode**—ready for you to hit "`Send`"!

‡You must have a default email client that can open "`*.eml`" files and send them out.

—</br>
[Back to Top](#table-of-contents)

----------------------------------------------------------------------------

## 2. Next Steps

### Build Your Own LLM Micro-Apps!

This app wasn't built by spending days setting up a complex JavaScript stack, it was built iteratively by communicating with a Large Language Model (LLM). 

You can build incredibly useful, customized single-page apps for your own daily workflows by leveraging LLMs. Here are some best practices on how to instruct an AI to build apps like this one:

* **Start Simple & Single-File:** Ask the AI to build your idea as a "single-file HTML/CSS/JS app." This eliminates deployment headaches and build steps.
* **Iterate Surgically:** Don't ask the AI to write a perfect app on the first prompt. Get a working prototype, then use prompts like: *"Just show me where to add/modify..."* (This prevents the AI from breaking working code or timing out).
* **Be Specific with Constraints:** State exactly what you want the logic to do. (e.g., *"Make sure it only reads columns named exactly 'To', 'CC'..."*)
* **Mock Up Features:** If you want a specific UI feature, describe it visually. (e.g., *"I want an accordion menu that's closed by default..."*)

The barrier to creating custom software is gone. What will you build next?

—</br>
[Back to Top](#table-of-contents)

----------------------------------------------------------------------------

## Other Resources

**Description** | **URL Link**
--- | ---
Google AI Professional Certificate (Coursera) | https://grow.google/ai-professional

—</br>
[Back to Top](#table-of-contents)

----------------------------------------------------------------------------

## Troubleshooting

Issue | Solution
--- | ---
**"It's not working!"** | This concise tutorial has distilled hours of sweat, tears, and troubleshooting; _it can't not work_

### Subject Line Base64 Encoding in `*.eml` File

The subject line in the output EML file is encoded to ensure that special characters, emojis, and international languages display correctly when the `*.eml` file is opened in an email client (like Microsoft Outlook, Apple Mail, or Thunderbird).

#### The Technical Reason (RFC 2047)

By default, the official Internet standard for email (RFC 2822) mandates that email headers (like "`To`", "`From`", and "`Subject`") can only contain basic 7-bit ASCII characters (standard English letters, numbers, and basic punctuation).

If a user generates a CSV where the subject line contains:

* Emojis (e.g., "Action Required 🚨")
* Accented characters (e.g., "Résumé for René")
* Non-Latin scripts (e.g., Japanese, Arabic, Chinese)

...and you inject them as raw text into the header, the email client will often mangle the text, display question marks (`???`), **or fail to parse the file entirely**.

#### How the Encoding Works

To get around this limitation, the email standard uses a MIME extension (RFC 2047) that allows non-ASCII text in headers using a very specific syntax:

```
=?charset?encoding?encoded-text?=
```

Breaking down the code `Subject: =?utf-8?B?...?=`:

* `=?` and `?=` tell the email client: "*Pay attention, the following text is encoded.*"
* `utf-8` specifies the character set, which supports virtually every character and emoji in existence.
* `B` stands for `Base64`. It tells the client how the text was scrambled into safe ASCII characters.
* `${encodeUtf8B64(subj)}` is your JavaScript translating the raw subject string into a safe `Base64` string.

#### Example

If your generated subject is "Café ☕":

* It is converted to UTF-8 bytes.
* It is converted to Base64: `Q2Fmw6kg4☕` (simplified example).
* It gets written to the file as: `Subject: =?utf-8?B?Q2Fmw6kg4☕?=`

When you double-click the downloaded `*.eml` file, the email client sees that string, decodes it behind the scenes, and perfectly renders "Café ☕" in the subject line.

—</br>
[Back to Top](#table-of-contents)

----------------------------------------------------------------------------

<p align="center">Copyright © 2026-∞ Athit Kao, <a href="http://www.athitkao.com/tos.html" target="_blank">Terms and Conditions</a></p>