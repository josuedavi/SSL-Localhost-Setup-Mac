# SSL-Localhost-Setup-Mac
Run SSL certificate (https://) on local server 

Steps:
#1: Download MAMP 
Link: https://www.mamp.info/en/downloads/

#2: Find your MAMP folder (should be in finder under applications)

#3 Once in MAMP folder, go to:
/conf/apache/httpd.cong
now open this file (I Used Atom to open it, you can use any text editor, we just need to make a few changes to the file)
/conf/apache/extra/httpd-ssl.cong
now open this file, also

#4 Now you want to create an ssl folder under your user
#find the root of yours users folder (From the command line)
~

#create ssl folder
mkdir ssl

#create 'v3.ext' file
touch v3.ext

#create 'v3.ext' file
touch server.csr.cnf
