# Password Cracking Two Protected PDFs

By Abigail Kwablah

Network Walks Week 3 project: two password-cracking exercises against two separately provided encrypted PDFs, using two different toolchains.

- [Method 1: John the Ripper (Johnny GUI + CLI)](docs/01-jtr-johnny-cli.md) — recovered password: `good-luck`
- [Method 2: Networkwalks Hash Calculator + Password Cracker](docs/02-networkwalks-tools.md) — recovered password: `password1`
- [Concept notes: hashing vs. encryption, why dictionary attacks work](notes/hashing-vs-password-cracking.md)

## What this demonstrates

Neither method "decrypts" a password directly. Both extract a stored verification hash from the PDF, then test candidate passwords by hashing each one with the same algorithm and comparing the result. A match means that candidate is the real password.

Both lab passwords here were cracked in under a second, not because they were short, but because they were predictable: one a known default value, the other a readable two-word phrase. See the notes doc for the full reasoning on why length alone doesn't protect against this class of attack.

## Acknowledgment

Labs provided by [Network Walks](https://networkwalks.com) cybersecurity training program.
