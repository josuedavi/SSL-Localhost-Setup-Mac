First of all you need to be able to run MAMP in port 80. This is a "heat check" if you don't have any process jamming http ports. You can check it like this:

sudo lsof | grep LISTEN
If you do happen to have any process with something like this *:http (LISTEN), you are in trouble. Before with adventure check if it isn't MAMP itself (yeah, you should close that beforehand)

ps <pid of that process>
If you don't see MAMP, you are in good hands, I have just the thing for you:

# I've forced the removal of the job
$ launchctl remove org.apache.httpd

# and load it again
$ launchctl load -w /System/Library/LaunchDaemons/org.apache.httpd.plist

# and unload it again
$ launchctl unload -w /System/Library/LaunchDaemons/org.apache.httpd.plist
Now you should be able to use port 80 (and almost any other) in MAMP. Just go to MAMP > Preferences > Ports Tab and click the Set to default Apache and MySQL ports.

Now comes the easy part, you just have to follow what this guy wrote here. Well, that's copy that here just in case ...

Backup your /Applications/MAMP/conf/ dir.

Generate a (dummy) SSL Certificate

$ cd ~

# generate a private key (will request a password twice)
$ openssl genrsa -des3 -out server.key 1024
 
# generate certificate signing request (same password as above)
$ openssl req -new -key server.key -out server.csr
 
# Answer the questions
Country Name (2 letter code) [AU]: CA
State or Province Name (full name) [Some-State]: Quebec
Locality Name (eg, city) []: Montreal
Organization Name (eg, company) [Internet Widgits Pty Ltd]: Your Company
Organizational Unit Name (eg, section) []: Development
Common Name (eg, YOUR name) []: localhost
Email Address []: your_email@domain.com
A challenge password []: # leave this empty
An optional company name []: # leave this empty
 
# generate the certificate
$ openssl x509 -req -days 365 -in server.csr -signkey server.key -out server.crt
 
# remove the password from the server key
$ cp server.key server.tmp
$ openssl rsa -in server.tmp -out server.key
 
# Move the certificate into your MAMP apache configuration folder
$ cp server.crt /Applications/MAMP/conf/apache
$ cp server.key /Applications/MAMP/conf/apache
Open /Applications/MAMP/conf/apache/httpd.conf and uncomment Include /Applications/MAMP/conf/apache/extra/httpd-ssl.conf.

Keep your vhost in /Applications/MAMP/conf/apache/extra/httpd-vhosts.conf just the same.

In /Applications/MAMP/conf/apache/extra/httpd-ssl.conf, find the following block and edit the fields Server Name and Document Root with the values you already have in your vhost.

#   General setup for the virtual host
DocumentRoot "/Applications/MAMP/Library/htdocs"
ServerName www.example.com:443
ServerAdmin you@example.com
ErrorLog "/Applications/MAMP/Library/logs/error_log"
TransferLog "/Applications/MAMP/Library/logs/access_log"
and edit in your DocumentRoot and ServerName settings:
Happy secure MAMPing!
