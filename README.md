CTF Walkthrough  - MingMecca
==============================
https://www.mingmecca.digital

Instructions
------------
The /instructions.html page can be found by clicking around on the Othello
board in the image or by viewing page source.
![alt text](/images/image1.png "image1.png")

Flag 1
------------
The first flag is a bit cheap. Attempting to navigate to a page that doesn't
exist results in a 404 error page. /404.html includes the first flag.
![alt text](/images/image4.png "image4.png")

Flag 2
----------------
Looking at the page source for /instructions.html, there is a hint on line #667
![alt text](/images/image2.png "image2.png")

Googling and reading about web crawlers leads to a file called robots.txt

/robots.txt includes a flag and something about /delphi.html.
![alt text](/images/image3.png "image3.png")

Flag 3
-----------------
On the page /delphi.html there is a login form. This is a rabbit hole that leads
nowhere. Looking at the source confirms that the 'Submit' button doesn't do
anything.

In the page source, there is a large text blob and a hint
'Encoding ≠ Encryption'. Encoding is not the same as encryption.

The text blob ends with '=='. These are Base64 padding characters.

The data can be decoded using `base64 -d` or using something like CyberChef.
![alt text](/images/image5.png "image5.png")

The result is a JPEG image with a flag and hint about /etc/vsftpd.conf
![alt text](/images/image6.png "image6.png")


Flag 4
-------
Vsftpd is an FTP server for Unix like systems. The hint `listen_port=2121`
suggests Vsftpd might be running somewhere and listening on port 2121.

Anonymous login is enabled and there are 3 files available for download

* Encrypted ZIP,
* Wordlist,
* Packet Capture (PCAP)

![alt text](/images/image7.png "image7.png")

Opening the PCAP in Wireshark and analyzing HTTP request statistics, there is
a PNG image downloaded from http://mingmecca.host:8181/
![alt text](/images/image8.png "image8.png")

This image can be exported to reveal a flag and a hint.
![alt text](/images/image9.png "image9.png")


Flag 5
------------------
In order to decrypt the ZIP file, it's safe to assume the provided wordlist can
be used to perform a bruteforce attack and find the password.
![alt text](/images/image10.png "image10.png")

An alternative to bruteforcing, is comparing the provided Rockyou wordlist with
the one included in Kali. Using diff on these two files will also reveal the
password.
![alt text](/images/image12.png "image12.png")

Inside the ZIP is an image with a flag, a perl script and a script extract from
the 1998 movie Pi.
![alt text](/images/image11.png "image11.png")


Flag 6
-------
Navigating to sundance.html there is a a block of text that appears to be a
jumbled conversation.

Googling the asemica perl script results in a GitHub project that implements
something called asemic markov-chained ciphers.
https://github.com/linenoise/asemica

From the project page 'Asemica is a symmetric cipher whose key is a document and
whose ciphertext looks like plaintext.'

So using the script extract from earlier as the corpus file, the jumbled
conversation can be decrypted to reveal a flag and hint.
![alt text](/images/image13.png "image13.png")

Flag 7
------------------
Navigate to /tartarus836564633329666788444.html. Here there's two QR codes and
a link to a project called C3 codes - http://c3codes.ourproject.org/

The first QR code, hidden in the Davinci man image, can be adjusted in an image
editor like Gimp and decoded using zbar-tools to reveal a flag. Might need to
spend some time with the curves and contrast to get it working.
![alt text](/images/image14.png "image14.png")


Flag 8
-------
The second, more colorful QR code has triple the capacity of a regular QR code.
The steps to decode it involve decomposing the combined images into three
channels. Then each channel can be decoded using zbar-tools. This will reveal
a .onion address and a flag.
![alt text](/images/image16.png "image16.png")

![alt text](/images/image15.png "image15.png")


Flag 9
------------------
The .onion address is only accessible via the Tor network.

The Tor Browser can be installed in Kali with `sudo apt -y install
torbrowser-launcher`
![alt text](/images/image17.png "image17.png")

After watching the Youtube clip from Mr Robot and downloading the ISO file,
it's clear that something has been hidden in a music file using the program
'Deep Sound'.

Deep Sound can be downloaded from http://www.jpinsoft.net/DeepSound/ however it
is only available for Windows.

Mounting or extracting the ISO, one of the WAVs is much larger in file size than
the others and does not have a thumbnail image.

Hidden in the file '04 Doh T.wav' is an image with a password for something, a
flag and a hint about /AndroidManifest.xml.
![alt text](/images/image21.png "image21.png")

Flag 10
-------
/AndroidManifest.xml is a page hosted on the Tor site. This page then leads to
another file 'secure_volume.hc'
http://cybersrqcgmhikkfy4e7ihrmvuf5436w6mrie2ljrrorrcmpiohdlead.onion:9090/AndroidManifest.xml
![alt text](/images/image22.png "image22.png")

Searching for information about .hc files tells us this file is probably a
Veracrypt volume.

Unlocking and mounting the volume with the password 'brimstone' reveals a PDF.
![alt text](/images/image23.png "image23.png")

Using strings, exiftool and binwalk, it appears this PDF is a polyglot. It is
both a PDF and a ZIP. Created using a tool called TruePolyglot.

Change the file extension to .zip and extract the GlowHockey APK inside.
![alt text](/images/image24.png "image24.png")

APKs are encoded and compressed Android application packages. Decoding and
extracting the APK with apktool, then analyzing the AndroidManifest.xml reveals
the final flag.
![alt text](/images/image25.png "image25.png")

Flag 11 - Bonus Meme
------------------
Back on /sundance.html and looking at the page source, there is a comment with
a Bitcoin transaction ID and a hint about OP_RETURN scripts.
![alt text](/images/image18.png "image18.png")

Searching for this Bitcoin TX on a blockchain explorer reveals a hex string.
![alt text](/images/image19.png "image19.png")

This can be decoded with CyberChef magic. Turns out to be a line of bash that
results in a flag and provides a hint for how all the other flags were
generated.
![alt text](/images/image20.png "image20.png")
