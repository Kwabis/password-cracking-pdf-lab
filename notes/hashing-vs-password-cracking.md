# Hashing vs. Encryption, and Why Password Cracking Works

## The core distinction
- **Encryption** is reversible with the right key: plaintext becomes ciphertext, and the key can turn it back into plaintext.
- **Hashing** is one-way: plaintext becomes a fixed-length digest, with no direct path back to the original input.

A password-protected PDF uses both. The document content is encrypted (reversible, given the correct derived key). But the value used to *verify* a submitted password is a stored hash-like value — it can't simply be reversed to read off the password.

## Why cracking tools don't "decrypt" the password
Tools like John the Ripper don't attack the PDF file directly. The process is:

1. A tool (`pdf2john`, run locally or by a web service) extracts the PDF's stored verification data into a standard format: `$pdf$<version>*<revision>*<key length>*<permissions>*<metadata flag>*<ID length>*<ID>*<U length>*<U value>*<O length>*<O value>`.
2. A cracking tool takes a candidate password, runs it through the same algorithm the PDF format uses, and compares the result against that stored U/O value.
3. Match → that candidate is the real password. No match → try the next one.

This is why the *strategy* for choosing candidates matters more than the specific tool: a dictionary/wordlist attack tries known real-world passwords first; brute force (incremental mode) tries every possible character combination, which is exhaustive but far slower against anything non-trivial.

## What this project demonstrated
Two independently provided lab passwords, both cracked in under a second against a wordlist of roughly 100–3000 entries:

| Password    | Length | Why it fell |
|-------------|--------|-------------|
| `password1` | 9      | Default/placeholder value, near the top of any common-password list |
| `good-luck` | 9      | Two real words joined by a hyphen — a recognizable pattern, not random |

Neither password was cracked because it was short. Both were cracked because they were **predictable** — either a known default value, or a human-readable phrase. A longer password built the same way (two dictionary words joined by punctuation) fails the same way regardless of length, because wordlist attacks — especially with word-mangling rules enabled — are built to test exactly these patterns.

**What resists this class of attack:** sufficient length *and* genuine randomness — not memorable patterns, not predictable substitutions like `p@ssw0rd`, which are themselves already present in most modern wordlists.

## Note on the two target files
This project used two separately provided encrypted PDFs — one for the John the Ripper lab, one for the Networkwalks tools lab — not the same file cracked twice. They happen to share a similar filename, but their extracted hashes (and therefore their permissions metadata and passwords) are different.
