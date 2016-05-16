ssh debbie.vlan77.be -p 22222 -l r……


1. Create a oneliner which shows the amount of currently revoked certificates in the Terena SSL CA

curl -s http://crl.terena.org/TERENASSLCA.crl | openssl crl -inform DER -text | grep 'Revocation' | grep '2014' | wc -l

2. Your very curious it-enabled grandmother likes to see how you solved

cat id_rsa.pub >> ~/.ssh/authorized_keys

3. Create a onliner which sends one icmp echo packet to 127.0.0.1 for every pdf downloaded from the

ping -c $(cat /home/logs/apache_google.log | grep "/documenten/echo/08-09/" | grep ".pdf" | wc -l) localhost

4. Create a oneliner which lists the top 3 most used passwords in the ftp brute force attack captured in
ftp_bruteforce.pcap

tshark -r ftp_bruteforce.pcap -Y "ftp.request.command==PASS" -T fields -e ftp.request.arg | sort | uniq -c | sort -nr | head -3 | awk '{print$2}'

5. (op debbie zelf) Perform a network capture while surfing to h ttp://debbie.vlan77.be/nw2/test.html . Write a wireshark
filter that will show only HTTP POST requests

nc -vz debbie.vlan77.be 1-10100 2>&1 | grep 'succeeded'

6. On server debbie, use the list of logged in users to print only the username that has been logged in to
the server for the longest time. (Hint: use “perl -ne”)

who -u | awk '{if($5 == "."){print $1,$4}}' | sort -k2 | head -1 | awk '{print $1}'

7. It is always a good idea to make (secure) backups of your data and store them on a different
location.

op debbie
tar -cvf backup.tar.gz ./ | openssl enc -des3 -in backup.tar.gz -out backup.tar.gz.enc | nc -l 15999 < backup.tar.gz.enc
op linux
nc -l debbie.vlan77.be 10010 | openssl aes-256-cbc -d -a -out something.tar.gz.new

8. On server debbie, list all TCP ports on which a daemon is currently listening for connections. Only show
the ports you can connect to using IPv4.

nc -zv4 debbie.vlan77.be 1-20000 2>&1 | grep 'succeeded'| cut -d' ' -f4 | grep -v '\(.\).*\1'

10. An ftp brute force attack was captured using wireshark in the following file: ftp_bruteforce.pcap. You
want to know if the attack was successful, i.e. if the

stap 1
tshark -r ftp_bruteforce.pcap -Y "ftp.response.code==230" -T fields -e tcp.dstport

stap 2
var=$(tshark -r ftp_bruteforce.pcap -Y "ftp.response.code==230" -T fields -e tcp.dstport) && tshark -r ftp_bruteforce.pcap -Y "tcp.srcport==$var" -T fields -e ftp.request.arg

11. Companies like Google and Microsoft make heavily use of the X.509 subjectAltName extension.
     KHLeuven also uses this extension to add an alternative name *.khleuven.be to the

echo "" | openssl s_client -connect gmail.com:443 2> /dev/null | perl -nle 'print if /BEGIN/../END/' | openssl x509 -text | grep DNS | sed 's/,/\n/g' | cut -d ':' -f2 | wc -l

12. Create a oneliner which lists all palindromes with exactly 4 letters in a dictionary.

4-5-6 LETTERS

grep -we '\(.\)\(.\)\2\1' /usr/share/dict/dutch
grep -w '^\(.\)\(.\).\2\1' /usr/share/dict/dutch
grep -we '^\(.\)\(.\)\(.\)\3\2\1$' /usr/share/dict/dutch

13. Show the permissions of only your homedirectory, without using any pipes. The solution must be
    generic: It should work from any location and without specifying your exact username

ls -ld ~

14.

15. Some subdirectory of /tmp contains a bunch of movies. However their extension is wrong. The extension
should be .avi in stead of .jpg. Copy =…

for f in tmp/**.jpg; do n=$(basename $f .jpg);cp $f tmp/$n.avi;done

16. Create a onliner which relies on the command ping to do a fully automatic icmp traceroute. Limit
the amount of hops to 10.

for i in {1..10}; do ping -c 1 -t $i facebook.com | grep "From" | cut -d ' ' -f2; done

17. The VoIP lab, one of the cnw2 labs amazed you so much you immediately bought some cheap VoIP
phones off Ebay for further experimenting

for i in {8000..9000}; do wget --user=admin --password=$i debbie.vlan77.be/nw2/phonie 2>&1 | if grep -q "200"; then echo $i; fi ; done

18. You just received your pem encoded certificate from you CA. Now you have 2 files: cert.pem en cert.key.
Use the command chmod to set the appropriate permissions.

chmod 600 cert.key
chmod 755 cert.pem

19. Create a linux CLI oneliner to extract the DNS servers and their destination ports of the DNS replies in
the sip_dump.pcap file

tshark -r sip_dump.pcap -Y dns.flags.response==0 -T fields -e dns.qry.name -e udp.dstport | sort | uniq

20. C reate a linux CLI oneliner to decode the following string “TGludXggUlVMRVM=“. (the

echo 'TGludXggUlVMRVM='|base64 --decode

21. What linux command with options should be used to perform a scan to the server debbie.vlan77.be …

nmap debbie.vlan77.be -r -sT

22. reate a CLI oneliner using openssl to retrieve the certificate of the server facebook.com and to
display only it’s fingerprint and public key.

openssl s_client -connect www.facebook.com:443 -showcerts 2>/dev/null | openssl x509 -pubkey -noout

20. EX! As a web server administrator you have been asked to give your manager a linux CLI oneliner to extract
the 5 IP adresses that contacted the web server the most. The apache log is located in /home/log.
Create a correct oneliner. The output should look something like this: (count IPs)

cat /home/logs/apache_google.log | awk '{print $1}' | sort | uniq -c | sort -nr | head -5

21. EX! Create a linux CLI oneliner to extract the source ports of the DNS requests in the sip_dump.pcap file.

tshark -r sip_dump.pcap -Y dns.flags.response==0 -T fields -e udp.srcport

22. Create a linux CLI oneliner to download and decode the encoded file “encoded.text” on
http://debbie.vlan77.be/nw2/encoded .

wget http://debbie.vlan77.be/nw2/encoded |base64 --decode

23. What linux command with options should be used to perform a scan to the server debbie.vlan77.be with
the following requirements: …

nmap -v -sV debbie.vlan77.be

24. Create a linux CLI oneliner to extract an overview of the different FTP usernames in the file
ftp_bruteforce.pcap.

tshark -r ftp_bruteforce.pcap -Y "ftp.request.command == USER" -T fields -e ftp.request.arg | sort | uniq -c | sort -rnk1 | head -3

25. Create a oneliner to show ‘Time = 15:44:25 (11/10/1901)’ with the current time and date

date +"Time = %X (%d/%m/%Y)"

26. Create a CLI oneliner to match all words with 1 4, 15 and 16 unique letters. The output shoud look like:
Words with 14 letters:

for i in {14..16}; do echo Woorden met $i letters: ; grep -E '^.{'$i'}$' /usr/share/dict/dutch | wc -l; done

27. Create a CLI oneliner using openssl to retrieve the certificate of the server www.facebook.be and to
encrypt the text “CNW2 RULES” with it’s public key.

cho "CN2 RULEZ" > secret.txt | openssl s_client -connect www.facebook.com:443 -showcerts 2>&1 | openssl x509 -pubkey -noout > pubkey.pem | openssl rsautl -encrypt -inkey pubkey.pem -pubin -in secret.txt | cat

28. Create a CLI oneliner to find a match between different rsa private key files and their companion crt files.
The output should look something like:
alfa.key matches to beta.crt

for key in $(ls | grep key); do for crt in $(ls | grep crt); do hashkey=$(openssl rsa -in $key -noout -modulus | md5sum ); hashcrt=$(openssl x509 -in $crt -noout -modulus | md5sum ); if [ "$hashkey" == "$hashcrt" ]; then echo $key matches $crt; else echo $key does not match $crt; fi; done ; done

34. Create a linux CLI oneliner to extract the top 3 SIP methods in sip_dump.pcap.

tshark -r sip_dump.pcap -Y sip.CSeq -T fields -e sip.CSeq.method | sort | uniq -c | sort -rn | head -3

35. Which linux command with options should be used to perform a scan with the following requirements:
Scan the first 20 systems in 193.191.187.129/25. Use the
system debbie.vlan77.be as ftp proxy server

nmap 193.191.187.129/25 -b debbie.vlan77.be --max-hostgroup 20

36. Create a CLI oneliner to retrieve the file public.gpg from the site encoded.rudi.vlan77.be and encode the
(own created) file clear.txt into secret.txt.

curl --silent http://encoded.rudi.vlan77.be/public.gpg | gpg --import public.gpg | gpg -e -u "Sender User Name" -r "Receiver User Name" clear.txt | mv clear.txt.gpg secret.txt

37. What Linux ssh command do you use to bind your local port 3000 to a web server on port 4444 on the
network of the ssh server.

ssh -L 3000 : randomserver.com : 4444 r0430817@debbie.vlan77.be

38. Create a regular expression to match all words in a dictionary with 5 unique letters

cat /home/logs/greek_alphabet.txt | grep -Eow '\w{5}' | grep -v '\(.\).*\1'

39. Use tshark to create an overview and an amount of the different response codes of FTP in the file
ftp_bruteforece.pcap.

tshark -r ftp_bruteforce.pcap -Y "ftp.response.code" -T fields -e ftp.response.code | sort | uniq -c |sort -nr



cat /dev/null > ~/.bash_history && history -c && exit

curl -is http://www.appminded.net/iamgroot.txt > temp.txt

