# Method 2: Networkwalks Hash Calculator + Password Cracker

Target file: `My Locked PDF1 (for NW Tools Lab WK3-PM2).pdf` — a separate file from Method 1, not the same PDF run through a different tool. Its extracted hash, permissions metadata, and recovered password all differ from the John the Ripper lab's file.

Tools: Networkwalks Hash Calculator (https://networkwalks.com/hash-calculator/), Networkwalks Password Cracker (https://networkwalks.com/password-cracker/) — both run entirely client-side in the browser; no file is uploaded to a server.

## Steps

1. Opened the Hash Calculator, selected the **PDF** tab, and uploaded the target PDF. The tool parsed it locally and extracted a `$pdf$4*4*128*-1060*1*16*55d1a5c1...` hash — pdf2john-compatible format, same underlying extraction logic as JTR's own `pdf2john`, just running in-browser instead of via Perl.
   (`screenshots/nw-tools/nw-tools-02-hash-calculator.png`)
2. Copied the full hash and pasted it into the Password Cracker's input field.
3. Left the built-in wordlist selected (100 common passwords) and clicked **Start Cracking**.
4. The tool tried candidates in sequence and matched on attempt 91 of 100.
   (`screenshots/nw-tools/nw-tools-01-password-cracked.png`)

**Result: `password1`**

## Verified result

Opened the PDF using the recovered password — confirmed correct.
