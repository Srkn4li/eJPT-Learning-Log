# Leaked Password Databases & Have I Been Pwned

## Overview
One of the biggest risks for any company is **password reuse**. Employees often use the same password for their corporate email as they do for LinkedIn, Adobe, or MyFitnessPal.
If one of those external services gets hacked, the attackers have a valid password that might work on the company's VPN or Outlook Web Access (OWA).

## The Gold Standard: Have I Been Pwned (HIBP)
This is the most famous and ethical resource to check for compromised accounts.
* **Website:** `haveibeenpwned.com`
* **How it works:** I enter an email address (e.g., `target@company.com`), and it checks a massive database of billions of leaked records.
* **The Result:** It tells me **IF** the account was compromised and **WHERE** (e.g., "Verification: Found in the LinkedIn 2016 Breach").

### Why HIBP is safe
* It does **not** show the password itself.
* It only confirms that the email was part of a breach.
* This makes it perfect for reporting to clients without handling illegal data myself.

## The Attacker's Perspective (Credential Stuffing)
While HIBP tells me *if* there is a leak, malicious attackers go a step further. They use "Breach Data Search Engines" (like DeHashed, Intelx, or Snusbase) to find the **actual** password or hash associated with the breach.

**The Attack Chain:**
1. **Discovery:** I see on HIBP that `admin@target.com` was in the "Adobe" breach.
2. **Retrieval:** An attacker looks up the Adobe dump to find the password, e.g., `Summer2014!`.
3. **Password Spraying / Stuffing:** The attacker tries `Summer2014!` (and variations like `Summer2023!`) against the company's login portal.

## My Takeaway
Checking for leaked credentials is crucial OSINT.
* **If I find a leak:** It doesn't mean I have access, but it gives me a massive hint about the target's password habits.
* **Pattern Recognition:** Even if the password is old, seeing `Fido123` in a leak tells me the user likes their dog's name. I can use that for creating a custom wordlist.
