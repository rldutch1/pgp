GPG/PGP Commands

Create / generate GPG/PGP keys:
	gpg2 --full-gen-key
	gpg --gen-key

Send key to default key server:
	gpg --send-key KEYNAME

Send key to specific key server:
	gpg --keyserver hkp://pgp.mit.edu --send-key KEYNAME

Export ascii armored key:
	gpg --export --armor jqdoe@example.com > jqdoe-pubkey.asc
	gpg --export-secret-keys --armor jqdoe@example.com > jqdoe-privkey.asc

Encrypt ascii file: 
	gpg -a -e filename

Encrypt binary file:
	gpg --encrypt filename
	
UUEncode a file before ascii encrypting:
	uuencode filename filename > filename.uue
	gpg -a -e filename.uue

UUEncode a file before normal encrypting:
	uuencode filename filename > filename.uue
	gpg --encrypt filename.uue

Decrypt UUEncoded file with normal encryption then UUDecode it:
	gpg --decrypt filename > filename.uue
	uudecode filename.uue

Decrypt to standard out:
	gpg --decrypt filename
	
Password encrypt a file:
	gpg -c -s filename
	
Clearsign a file:
	gpg --clearsign filename

Import GPG/PGP key:
	gpg --import filename.asc
	
Show fingerprint from key:
	gpg --fingerprint keyid

Show fingerpring from file:
	gpg --with-fingerprint filename

List keys:
	gpg --list-keys
	
Delete Keys:
	gpg --delete-keys keyid

