# Method 1: John the Ripper (Johnny GUI + Command Line)

Target file: `My Locked PDF1.pdf`
Tools: John the Ripper (jumbo build, Windows), Johnny GUI, Strawberry Perl

## Hash extraction — two ways, same result

**Option A — web-based extractor**
Uploaded the PDF to an online pdf2john extractor, which runs `pdf2john` server-side and returns the hash in `$pdf$...` format.

**Option B — local extraction, no third party**
Installed Strawberry Perl (https://strawberryperl.com/) so `pdf2john.pl`, which ships inside JTR's `run` folder, could be run locally instead of trusting a website with the file:

```
cd C:\Users\user\Downloads\john-1.9.0-jumbo-1-win64\run
perl pdf2john.pl "C:\Users\user\Downloads\My Locked PDF1.pdf" > hash2.txt
```

Compared the two outputs — identical `$pdf$4*4*128*-1028*1*16*...` string from both methods, confirming the web tool and the local script do the same extraction.

## Cracking via Johnny (GUI)

1. Opened Johnny, went to **Settings** → **Browse** → selected `john.exe` in the JTR `run` folder.
2. **Open password file** → "Open password file (PASSWD format)" → selected the saved hash file. Confirmed the **Formats** column read `PDF`.
3. Went to **Options** in the sidebar, selected **Wordlist** as the attack mode (not the default Single crack / Incremental), browsed to `password.lst` in the same `run` folder as the wordlist source.
4. Back on the **Passwords** screen, clicked **Start new attack**.
5. Cracked almost instantly.

**Result: `good-luck`**

See: `screenshots/jtr/jtr-flag-captured.png` — the lab's flag-capture confirmation after opening the decrypted PDF.

## Cracking via command line

Ran directly against `john.exe` rather than through the GUI, since this is the more common way JTR is actually used in practice.

Troubleshooting encountered along the way:

- `perl pdf2john.pl "pdf2john.pl"` → `Can't open perl script "pdf2john.pl": No such file or directory` — this happened before `cd`-ing into the `run` folder; the script only exists there, not in the parent JTR folder or the Downloads root.
  (`screenshots/jtr/jtr-cmd-01-hash-extraction-path-errors.png`)
- Path errors like `File not found: C:/Downloads/ My Locked PDF1.pdf` — caused by typing the PDF's path by hand and getting the folder wrong (`C:\Downloads\` instead of `C:\Users\user\Downloads\`) plus a stray space before the filename. Fixed by using File Explorer's **Copy as path** on the actual file instead of typing it manually.
  (`screenshots/jtr/jtr-cmd-02-hash2-output.png`)
- `john --show hash1.txt` → `stat: hash1.txt: No such file or directory` — the file existed, but in a different folder than the one the command was run from (`hash1.txt` was saved via Notepad to Downloads root, not into the `run` folder).
  (`screenshots/jtr/jtr-cmd-03-show-attempt-fail.png`)
- `john --wordlist=password.lst hash2.txt` → `No password hashes loaded (see FAQ)` — `pdf2john.pl`'s raw output is in `filename:hash` format. Because the Windows path begins with a drive letter (`C:`), John split on the *first* colon it found — right after the `C` — rather than the colon separating the filename from the hash. This mangled the rest of the line into something John couldn't parse.
  (`screenshots/jtr/jtr-cmd-04-cli-no-hashes-loaded.png`)
- **Fix:** stripped the `filename:` prefix, keeping only the bare `$pdf$...` string, saved as `hash3.txt`, and re-ran:
  ```
  john --wordlist=password.lst hash3.txt
  john --show hash3.txt
  ```
  Cracked in under a second: `good-luck`
  (`screenshots/jtr/jtr-cmd-05-cracked-good-luck.png`)

## Verified result

`good-luck` — confirmed both via Johnny's GUI result and independently via the command line, and confirmed by successfully opening the PDF with the recovered password.
