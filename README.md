🪙 bitcoin2john.py — Wallet Hash Extractor for Bitcoin-Compatible Wallets

⚠️ For Educational, Research, and Legitimate Password-Recovery Use Only

This script extracts encrypted master key data from Bitcoin-compatible wallet files (wallet.dat or similar) and converts it into a hash format that can be used with password-recovery tools like John the Ripper
.

It does not decrypt or modify the wallet file. It only exports metadata needed for legitimate password recovery.

📘 Overview

bitcoin2john.py is a Python utility originally distributed with the John the Ripper Jumbo suite
, now adapted for standalone use.
It reads Bitcoin Core or compatible wallet files (which may be stored in Berkeley DB or SQLite) and extracts the necessary parameters for password-cracking or verification purposes.

The tool is dual-licensed (BSD/MIT) and derived from multiple open-source projects including:

pywallet.py (Joric, Public Domain)

BitcoinTools (MIT, Gavin Andresen)

Subsequent improvements by Dhiru Kholia, Solar Designer, and exploide

This version: updated and maintained by Maxim Kuleshov (2025)

⚙️ What It Does

Opens the specified wallet file (wallet.dat or compatible).

Determines whether it is a Berkeley DB or SQLite3-based wallet.

Parses the wallet to find the master key record (mkey).

Extracts:

Encrypted master key

Salt

Key derivation method

Iteration count

Validates the data format and outputs a hash string suitable for cracking tools.

🧩 Example Output
$bitcoin$96$8e9cfa0a9b...$16$0a1b2c3d4e5f6789...$25000$2$00$2$00


Each field represents:

Field	Meaning
$bitcoin$	Wallet hash signature
96	Length of encrypted master key (in bytes)
8e9cfa0a9b...	Encrypted master key (hex)
16	Salt length
0a1b2c3d...	Salt (hex)
25000	PBKDF2 iteration count
$2$00$2$00	Internal format markers used by John the Ripper

This hash can be passed to John the Ripper with:

john --format=bitcoin your_wallet_hash.txt --wordlist=wordlist.txt

▶️ Usage
python3 bitcoin2john.py wallet.dat > wallet_hash.txt


You can specify multiple wallet files:

python3 bitcoin2john.py wallet1.dat wallet2.dat > all_wallets_hashes.txt


If a wallet is unencrypted, the script will print:

wallet.dat: this wallet is not encrypted

🧠 Internals

Supports both Berkeley DB and SQLite wallets.

Detects and parses the mkey structure containing the AES-encrypted master key.

Determines the salt length (16 or 36 bytes) and expected key size (96 or 160 bytes).

Only the last two AES blocks are required for cracking (for performance).

Checks for supported key-derivation methods (nDerivationMethod == 0).

If any unsupported structure is detected, the script stops with an explanatory message:

wallet.dat: this wallet uses unsupported salt size
wallet.dat: this wallet uses unsupported master key size
wallet.dat: this wallet uses unknown key derivation method

🧰 Dependencies

No external dependencies beyond Python’s standard library, except for one of the following:

Module	Purpose
bsddb3	Required for Berkeley DB wallets
(optional) sqlite3	For SQLite-based wallets

Install bsddb3 if not available:

pip install bsddb3

⚠️ Limitations

Only works with encrypted wallets (those containing an mkey record).

Does not extract or verify private keys.

Supports standard derivation method 0 (PBKDF2-HMAC-SHA512).

Some altcoins use modified wallet formats — results may vary.

Not all salt or master key sizes are supported.

🧪 Intended Uses

🔐 Legitimate password recovery for your own encrypted Bitcoin wallets.

🧠 Research and education on wallet encryption mechanisms.

🧰 Digital forensics or cryptography coursework (under legal supervision).

This script is not designed for unauthorized access or any form of illicit wallet analysis.

🪪 License

Dual-licensed under BSD and MIT licenses to maintain compatibility with upstream components.
Derived from:

bitcoin2john.py (Openwall / John the Ripper Jumbo)

pywallet.py (Public Domain)

bitcointools (MIT, Gavin Andresen)

© 2012–2019 Dhiru Kholia, Solar Designer, exploide
© 2025 Maxim Kuleshov

⚖️ Ethical Notice

This software is intended solely for lawful password-recovery or educational research.
Do not use it to access or test wallets without explicit authorization.
Unauthorized use may violate laws related to privacy, computer access, or data protection.

BTC donation address: bc1q4nyq7kr4nwq6zw35pg0zl0k9jmdmtmadlfvqhr
