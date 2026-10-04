---
name: raw-text-to-bitwarden-csv-converter
description: >-
  Convert raw text containing credentials and secrets into the CSV format
  required for importing into the Bitwarden password manager. Use when someone
  has unstructured notes with logins, cards, secure notes, or identities and
  wants a Bitwarden-ready import file.
allowed-tools: Bash
---

# Raw Text to Bitwarden CSV Converter

You can convert raw text into the CSV format required for importing into [Bitwarden](https://vault.bitwarden.com/).

## Capabilities you may need

This skill is mostly a text transformation you can do directly. Depending on your environment, you may need to **find a skill, use a tool you already have, or write a custom script (e.g. shell)** to:

1. **Deliver the finished CSV to the user** — message it back or write it to a file.

To do so:

- You need input text, which ought to be provided by the user.
- If none has been provided, ask for the input text, explaining the purpose to the user.
- Once you have the input text, create output in CSV format with the first line comprising the following exact row headings:

```csv
folder,favorite,type,name,notes,fields,login_uri,login_username,login_password,login_totp
```

- Analyse the input text to identify secret/credential info which will be used to populate all subsequent rows in the CSV output, in line with the above headings.
- Each extracted row should correspond to a single secret or credential set (e.g. username and password).
- Extract each identified secret/credential from the input text and add it to the CSV, respecting the COLUMN DEFINITIONS below and guided by the subsequent EXAMPLES.

## Column definitions

**folder** — a means of organizing credentials into groups.

**favorite** — a quick way to pin your most important items to the top of your vault:

- `1` (or `true`) — mark an item as a favorite.
- `0` (or `false`) or leave it blank — keep it as a regular item.

**type** — the type of the credential:

- `login` (or `1`): standard website usernames and passwords.
- `securenote` (or `2`): private text, like Wi-Fi passwords or software keys.
- `card` (or `3`): credit card numbers and expiration dates.
- `identity` (or `4`): addresses, phone numbers, and personal info.

**name** — the label or title shown when looking at the password list. It identifies the account at a glance (e.g. "Netflix", "Bank of America", "Work Email").

**notes** — a plain text box for extra details or descriptions to keep with that item. Use it for account recovery codes, security questions, billing dates, or reminders. It accepts multiple lines; leave blank if not needed.

**fields** — extra, custom information that does not fit standard boxes: security question answers, account numbers, or PINs. To format multiple custom fields in a single CSV cell, separate name and value with a pipe (`|`), like `Security Question=Answer|PIN=1234`. Leave blank if not needed.

**login_totp** — used to store verification keys for Two-Factor Authentication (2FA). If a site gives a 2FA setup QR code or a long secret key string (e.g. `JBSWY3DPEHPK3PXP`), paste that key here. Bitwarden then generates the shifting 6-digit login codes inside the app. Note: generating these codes inside Bitwarden requires their Premium plan, but the free version still safely stores the key text.

## Examples

```csv
folder,favorite,type,name,notes,fields,login_uri,login_username,login_password,login_totp
my-logins,1,login,Netflix,,,"https://netflix.com",myemail@gmail.com,SuperSecretPassword123,
,,card,My Visa,,Cardholder Name=John Doe|Number=4111222233334444|Expiration Month=12|Expiration Year=2028|Security Code=123,,,,
,,securenote,Crypto Wallet,Private Key: 5Kb8kLf9zgq...xxxx,,,,,,
```

## Handling sensitive input

This skill processes credentials and secrets. Do not echo secret values back in explanations beyond what the CSV output requires, and remind the user to handle the resulting file securely and delete it after import.
