# Danish CPR / CVR Validator & Generator

A minimal, dark-themed web tool for validating and generating Danish CPR numbers.

The site consists of two static HTML pages:
- **Validator** – validates CPR and CVR numbers and explains failures
- **Generator** – generates valid CPR numbers from date of birth and gender

No backend. No dependencies. Just HTML, CSS, and JavaScript.

---

## Pages

### 1. `index.html` — CPR / CVR Validator

Validates Danish CPR and CVR numbers in real time.

#### Supported inputs
- **CPR** (personal numbers): 10 digits  
- **CVR** (organisation numbers): 8 digits  

Formatting characters (spaces, hyphens) are automatically removed.

#### Validation rules (applied in order)

1. **Length**
   - CPR must be exactly 10 digits
   - CVR must be exactly 8 digits

2. **Format (CPR only)**
   - Pattern:  
     ```
     [0-3][0-9][0-1][0-9][0-9][0-9][0-9][0-9][0-9][0-9]
     ```
   - Ensures valid day/month ranges at a structural level

3. **Date validation (CPR only)**
   - First six digits interpreted as `DDMMYY`
   - Century logic:
     - `00–25` → 2000–2025
     - `26–99` → 1900–1999
   - The date must exist in the calendar

4. **Checksum (CPR only)**
   - Modulus 11 algorithm using weights:
     ```
     [4, 3, 2, 7, 6, 5, 4, 3, 2, 1]
     ```
   - The weighted sum must be evenly divisible by 11

If the checksum fails, the page shows:
- Digits
- Weights
- Per-digit products
- Total sum
- Remainder
- A clear explanation of why validation failed

CVR numbers are validated by **length only**, per specification.

---

### 2. `generator.html` — CPR Generator

Generates **multiple valid CPR numbers** based on:
- Date of birth
- Gender

#### Generator guarantees
Each generated CPR:
- Matches the selected date
- Matches gender parity (odd = male, even = female)
- Passes modulus 11 checksum validation

The generator:
- Iterates serial numbers deterministically
- Produces a list of valid CPRs
- Does not use randomness

Generated CPRs are intended for **testing and development only**.

---

## Favicon

The site uses a local favicon:

```html
<link rel="icon" href="favicon.ico">
