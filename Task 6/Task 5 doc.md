# 🔐 Task 6: Create a Strong Password and Evaluate Its Strength

## Objective
Understand what makes a password strong by creating passwords with varying complexity and testing them against password strength tools. Research common attack methods and identify best practices for password security.

---

## 🛠️ Tools Used
- [passwordmeter.com](https://www.passwordmeter.com) — Primary strength checker
- [security.org/how-secure-is-my-password](https://www.security.org/how-secure-is-my-password/) — Time-to-crack estimator
- [haveibeenpwned.com](https://haveibeenpwned.com) — Breach database checker

---

## 🔑 Password Samples Tested

| # | Password | Length | Uppercase | Lowercase | Numbers | Symbols | Score (%) | Rating |
|---|----------|--------|-----------|-----------|---------|---------|-----------|--------|
| 1 | `password` | 8 | ❌ | ✅ | ❌ | ❌ | 4% | Very Weak |
| 2 | `Password@` | 9 | ✅ | ✅ | ✅ | ❌ | 38% | Weak |
| 3 | `P@ssw0rd@@!` | 9 | ✅ | ✅ | ✅ | ✅ | 55% | Fair |
| 4 | `Tr0ub4dor!&3` | 11 | ✅ | ✅ | ✅ | ✅ | 72% | Strong |
| 5 | `G7#kLmQ2!!##$vXp` | 13 | ✅ | ✅ | ✅ | ✅ | 100% | Very Strong |

---

## 📊 Detailed Results & Feedback

### Password 1: `password`
- **Score:** 4% — Very Weak  
- **Time to Crack:** < 1 millisecond  
- **Feedback:**
  - No uppercase letters
  - No numbers or symbols
  - Common dictionary word — instant dictionary attack victim
  - One of the top 10 most commonly used passwords globally

<img src="https://github.com/Endlesscyber/Elevate-Labs/blob/4cc2cbdd5410f4471118feea4529697b958026cf/Task%206/Images/Pass1.png">
---

### Password 2: `Password@`
- **Score:** 38% — Weak  
- **Time to Crack:** ~3 seconds  
- **Feedback:**
  - Has uppercase and number, but at predictable positions
  - Still a common pattern recognized by attack tools
  - Lacks symbols — significant weakness

<img src="https://github.com/Endlesscyber/Elevate-Labs/blob/4cc2cbdd5410f4471118feea4529697b958026cf/Task%206/Images/pass%202.png">
---

### Password 3: `P@ssw0rd@@!`
- **Score:** 55% — Good  
- **Time to Crack:** ~2 hours  
- **Feedback:**
  - Classic leet-speak substitution (a→@, o→0)
  - Leet-speak patterns are well-known to modern crackers
  - Adds symbols, but still based on a dictionary word

<img src="https://github.com/Endlesscyber/Elevate-Labs/blob/4cc2cbdd5410f4471118feea4529697b958026cf/Task%206/Images/pass%203.png">
---

### Password 4: `Tr0ub4dor!&3`
- **Score:** 72% — Strong  
- **Time to Crack:** ~3 days  
- **Feedback:**
  - Not a single dictionary word — harder to crack
  - Good mix of all character types
  - Memorable passphrase-style with substitutions
  - Still relatively short for modern standards

<img src="https://github.com/Endlesscyber/Elevate-Labs/blob/4cc2cbdd5410f4471118feea4529697b958026cf/Task%206/Images/pass%204.png">
---

### Password 5: `G7#kLmQ2!!##$vXp`
- **Score:** 100% — Very Strong  
- **Time to Crack:** ~34 years  
- **Feedback:**
  - Fully random character set
  - High entropy — no recognizable patterns
  - Mix of all four character types
  - Not dictionary-attackable

<img src="https://github.com/Endlesscyber/Elevate-Labs/blob/4cc2cbdd5410f4471118feea4529697b958026cf/Task%206/Images/pass%205.png">
---


## ⚔️ Common Password Attacks

### 1. 🔨 Brute Force Attack
A brute force attack tries **every possible combination** of characters until the correct password is found.

- **How it works:** Automated tools cycle through `a`, `b`, ... `z`, `aa`, `ab`, ... trying every permutation.
- **Speed:** Modern GPUs can test **billions of hashes per second**
- **Effectiveness against short passwords:** An 8-character lowercase password can be cracked in **minutes**
- **Defense:** Use long passwords (14+ characters). Each added character exponentially increases crack time.

**Example crack times (brute force on MD5 hash):**

| Length | Character Set | Combinations | Time to Crack |
|--------|--------------|-------------|---------------|
| 6 | lowercase only | ~300 million | < 1 second |
| 8 | lowercase + numbers | ~2.8 trillion | ~5 seconds |
| 10 | all characters | ~59 quadrillion | ~1 year |
| 14 | all characters | ~5 × 10²⁶ | Centuries |

---

### 2. 📖 Dictionary Attack
A dictionary attack uses a pre-built wordlist of **common passwords, words, and phrases**.

- **How it works:** Tools like Hashcat or John the Ripper load millions of common passwords and test them all
- **Common dictionaries include:** RockYou (14 million passwords), common names, sports teams, keyboard patterns (`qwerty`, `123456`)
- **Leet-speak mutations** are also included (e.g., `p@ssw0rd`, `h3ll0`)
- **Defense:** Avoid real words, names, dates, or any recognizable patterns

---

### 3. 🌈 Rainbow Table Attack
Pre-computed hash lookup tables that match known hashes to passwords.

- **Defense:** Salted hashing (adding random data before hashing) makes rainbow tables useless

---

### 4. 🧠 Credential Stuffing
Using username/password pairs leaked from data breaches to try logging into other sites.

- **Defense:** Never reuse passwords across sites. Use a password manager.

---

## ✅ Best Practices for Strong Passwords

Based on the evaluation and research, here are the key practices:

### 🏆 The Golden Rules

1. **Length is King** — Aim for **at least 14–16 characters**. Length has the biggest impact on security.

2. **Use All Four Character Types**
   - Uppercase: `A-Z`
   - Lowercase: `a-z`
   - Numbers: `0-9`
   - Symbols: `!@#$%^&*()-_=+`

3. **Avoid Dictionary Words** — Even with substitutions (`p@ssw0rd`), these are crackable. Use random strings or passphrases.

4. **Never Reuse Passwords** — One breach exposes all your accounts if you reuse.

5. **Use a Password Manager** — Tools like Bitwarden, 1Password, or KeePass generate and store strong passwords securely.

6. **Avoid Personal Information** — Birthdates, names, phone numbers, and pet names are easily guessable.

7. **Avoid Keyboard Patterns** — `qwerty`, `12345678`, `asdfgh` are in every attacker's dictionary.

8. **Enable Multi-Factor Authentication (MFA)** — Even a strong password can be stolen; MFA adds a critical second layer.

---

## 📈 How Password Complexity Affects Security

Password strength is measured in **entropy** — the number of possible combinations.

**Entropy Formula:**
```
Entropy (bits) = log₂(Character Set Size ^ Password Length)
```

| Character Set | Size | 8-char Entropy | 16-char Entropy |
|--------------|------|----------------|-----------------|
| Lowercase only | 26 | 37.6 bits | 75.2 bits |
| Lower + Upper | 52 | 45.6 bits | 91.2 bits |
| Lower + Upper + Numbers | 62 | 47.6 bits | 95.3 bits |
| All printable characters | 95 | 52.6 bits | 105.2 bits |

> **Rule of thumb:** Security experts recommend **at least 80 bits of entropy** for sensitive accounts. That means 14+ characters using all character types.

### Key Takeaway
> A 17-character random password using all character types would take **longer than the age of the universe** to crack with today's fastest hardware — while an 8-character dictionary-based password can fall in **under a second**.

---

## 💡 Tips Learned from the Evaluation

| # | Tip |
|---|-----|
| 1 | Length matters more than complexity alone — a 16-char lowercase string beats an 8-char complex one |
| 2 | Leet-speak substitutions (`@`, `0`, `3`) offer minimal protection — attackers know all the patterns |
| 3 | Using passphrases (4+ random unrelated words) is both strong and memorable |
| 4 | Password strength checkers give instant feedback — use them during account creation |
| 5 | Check passwords against breach databases (HaveIBeenPwned) before using them |
| 6 | Password managers eliminate the need to memorize complex passwords |
| 7 | MFA is non-negotiable for high-value accounts regardless of password strength |
| 8 | Never share passwords, store them in plaintext, or write them on sticky notes |

---

## 📝 Summary

Password complexity directly determines resistance to both brute-force and dictionary attacks. A weak, short, or predictable password can be cracked in milliseconds, while a long, random, multi-character-type password would take centuries. The most effective strategy combines:

- **Strong, unique passwords** (14+ chars, all character types, random)
- **A password manager** to generate and store them
- **Multi-Factor Authentication** as a safety net
- **Regular breach monitoring** via HaveIBeenPwned

---
