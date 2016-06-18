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
	Use a double greater than symbol to export both public and private to the same file ">>".
	
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

Sign a filename:
	gpg --sign filename

Verify a signature:
	gpg --verify file.txt.gpg

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

Generate a revocation certificate:
	gpg --output revoke.asc --gen-revoke KEYNAME

Revoke the key by importing the revocation certificate:
	gpg --import revoke.asc
	
Send the revoked key to a keyserver:
	gpg --keyserver subkeys.pgp.net --send KEYNAME

Bash Function:
pgp(){
case $1 in
        ascii)
				echo "ASCII Encrypting... " $2
        gpg -a -e $2
				echo "..." >> $2;rm -f $2
				echo $2 " has been deleted!"
        ;;
        uue)
  			echo "UUEncoding..." $2
  			uuencode $2 $2 > $2.uue
 				echo "ASCII Encrypting..." $2.uue
 				gpg -a -e $2.uue
 				echo "..." > $2.uue;rm -f $2 $2.uue
 				echo $2 " has been deleted!"
  			;;
        uuegpg)
  			echo "UUEncoding..." $2
  			uuencode $2 $2 > $2.uue
 				echo "Encrypting..." $2.uue
 				gpg --encrypt $2.uue
 				echo "..." > $2.uue;rm -f $2 $2.uue
 				echo $2 " has been deleted!"
  			;;
        uuedecrypt)
 				#echo "Decrypting..." $2
 				gpg --decrypt $2 > $2.uue
  			echo "Decoding..." $2
  			uudecode $2.uue;
  			echo "..." > $2.uue;rm -f $2.uue
 				echo $2 " has been decoded!"
  			;;
        passencrypt)
        echo "Encrypting..." $2
        gpg -c -s $2
				echo $2 " has been password encrypted!"
        ;;
        clearsign)
        echo "Signing..." $2
        gpg --clearsign $2
				echo $2 " has an associated signature file!"
        ;;
        sign)
        echo "Signing..." $2
        gpg --sign $2
				echo $2 " has been signed!"
        ;;
        verify)
        echo "Signing..." $2
        gpg --verify $2
				echo $2 " verify status!"
        ;;
        decrypt)
        #echo "Decrypting..." $2
        gpg --decrypt $2
        ;;
        encrypt)
        echo "Encrypting..." $2
        gpg --encrypt $2
        echo "..." > $2;rm -f $2
        echo $2 " has been deleted!"
        ;;
        import)
        echo "Importing..." $2
        gpg --import $2
        echo $2 " has been imported!"
        ;;
        fingerprint)
				gpg --fingerprint $2
				;;
				fingerprint_from_file)
				gpg --with-fingerprint $2
				;;
        delete-keys)
        echo "Deleting..." $2
        gpg --delete-keys $2
        gpg --list-keys
        ;;
		list)
			gpg --list-keys
		;;
		message)
	cd ~/Documents/PGP/messages
	xyzrh1=message.`date +"%Y%m%d%H%M%S%Z"`
	vi $xyzrh1.txt; pgp ascii $xyzrh1.txt
				;;
        *)
    message000="pgp ascii textfile"
    message001="pgp uue filename"
    message002="pgp uuegpg filename"
    message003="pgp uuedecrypt filename"
		message004="pgp passencrypt textfile"
		message005="pgp clearsign textfile"
		message006="pgp sign textfile"
		message007="pgp decrypt filename"
		message008="pgp encrypt filename"
		message009="pgp import filename.asc"
		message010="pgp delete-keys 5DD98B3E"
		message011="pgp list"
		message012="gpg --edit-key 5DD98B3E"
		message013="pgp fingerprint"
		message014="pgp fingerprint_from_file"
		message015="pgp verify filename"
		message999="message"
                echo $message000
                echo $message001
                echo $message002
                echo $message003
                echo $message004
                echo $message005
                echo $message006
                echo $message007
                echo $message008
                echo $message009
                echo $message010
                echo $message011
                echo $message012
                echo $message013
                echo $message014
                echo $message015
        ;;
esac
}


On Fedora Linux, the passphrase cache timeout was 300 seconds (5 minutes). I couldn't find where I could change that setting. I ended up installing dconf-editor and I was able to change the passphrase cache. I added screenshots (dconf-editor_settings##.png.

