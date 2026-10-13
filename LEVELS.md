# LOCKE PROTOCOL - The 5 Levels (what you do in each)

Working title: LOCKE PROTOCOL. Mr. Robot realism + the 1990s hacker movie look.
Setting: Parker Datalink basement lab, Oct 14, 1997 (the computers are modern Kali Linux, everything else is 90s).
Everything is a SIMULATION. No real networks, no real hacking. The commands are real so players learn real Linux.
Run it: open `index.html` in a browser (or `python -m http.server` in this folder, then http://localhost:8000).

## How the player plays
- A real terminal. Type real commands. Tab completes, Up arrow repeats history.
- `help` lists commands. `hint` gives a nudge. `objectives` shows the checklist. The side panel tracks objectives.
- sudo password is in README.txt (it is `gold`).
- Each level ends with a short cutscene, then the next level starts.

## LEVEL 1 - COLD BOOT  (learn the basics, patch the system)
Story: Locke can't remember what Steele took. Start by getting the machine ready.
You do:
1. `sudo apt update` - refresh package lists (asks for the sudo password).
2. `sudo apt upgrade -y` - patch the system.
3. `cat README.txt` - read Locke's note (finds the password and the plan).
4. `cat notes/steele.txt` - the clues on Steele (his server is on 10.13.37.0/24, uses a lowercase bird for passwords).
Real skills: apt, sudo, ls, cd, cat.

## LEVEL 2 - RECON  (find the target)
Story: Find the machine on Steele's network that matters.
You do:
1. `sudo apt install nmap -y` - install the scanner (nmap is NOT preinstalled, like a real fresh box).
2. `nmap -sn 10.13.37.0/24` - ping sweep. Finds 3 hosts; vault.local (10.13.37.42) is Steele's.
3. `nmap -sV 10.13.37.42` - service scan. Finds SSH (22) and Apache (80).
4. `curl http://10.13.37.42` - read the web page. An HTML comment gives away an exposed /backup/ folder.
Real skills: nmap host discovery, version detection, curl.

## LEVEL 3 - ENTRY  (get a password, get in)
Story: Steele left a backup of his password file on the public web server.
You do:
1. `wget http://10.13.37.42/backup/shadow.bak` - download the exposed file.
2. `sudo apt install john -y` - install John the Ripper.
3. `john --wordlist=/usr/share/wordlists/rockyou.txt shadow.bak` - crack the hash. Answer: `kestrel` (the bird).
4. `ssh msteele@10.13.37.42` - log in with the cracked password.
Real skills: wget, password cracking with a wordlist, ssh.

## LEVEL 4 - INSIDE THE VAULT  (follow the trail)
Story: You are on Steele's machine (the prompt changes to msteele@vault). Find what he hid.
You do:
1. `cat .bash_history` - his history shows he base64'd a key and encrypted a memory file.
2. `grep -ri locke /var/log/vault` - the logs show the "locke_memory" archive and that LOCKE_PROTOCOL is staged.
3. `base64 -d key.b64` - decodes to the passphrase: `the-world-is-round`.
4. `gpg --decrypt locke_memory.gpg` - enter the passphrase. Reveals Steele's note and extracts the key.
5. `exit` - leave before he notices.
Real skills: reading shell history, grep -r, base64 decode, gpg decrypt, exit.

## LEVEL 5 - LOCKE_PROTOCOL  (the finale)
Back on your own machine the screen goes black: `LOCKE_PROTOCOL: UNLOCK?`
You do:
1. Type `unlock`.
Then the final scene plays (your script, word for word): the golden light, the Digital Memory Field, the golden circle that shatters into a halo around a rising digital sun, and "Then let's walk it." FADE OUT. END.

## Notes / decisions I made (since you were going to bed)
- Gave it a working title, LOCKE PROTOCOL. Easy to change.
- Passwords/answers: sudo = gold, Steele = kestrel, gpg passphrase = the-world-is-round (the "shape of the world" clue from your script).
- Characters are simple 90s-style placeholders (flannel + beanie, bucket hat + round shades, overalls + butterfly clips, power suit). Per your note: characters later.
- Ideas for tomorrow: more levels / a 90s desktop screen between levels / a Discman + pager HUD / sound design / a "hacker handbook" in-game with the command list / mobile on-screen keyboard shortcuts.
- The "Hack'd" folder in App Store was EMPTY when I looked. If there are files for this game, put them there and tell me.
