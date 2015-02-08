---
layout: post
title: Enabling HTTPS on Apache server with Namecheap
---

###What's the difference with HTTP ?
HTTPS (*Hypertext Transfer Protocol Secure*) is built on top of HTTP , the protocols SSL/TLS are reponsible for the encryption of the connection. SSL and TLS are two similar protocol for encryption, they provide security over the network. SSL is the predecessor of TLS, recently SSL has found being vulnerable to [POODLE attack](http://googleonlinesecurity.blogspot.fr/2014/10/this-poodle-bites-exploiting-ssl-30.html). 

Having an encrypted connection means that nobody can spy on your connection between the server and the client. An other difference : HTTP uses the port 80 whereas the HTTPS uses the port 443.


###Why should I activate HTTPS ?
For **security matter**, when your website is dealing with sensible data like password or email, it is strongly recommended to activate the HTTPS mode.

*Note : HTTPS doesn't mean your password can not be compromised, it just means nobody can intercept your data on the transport layer.* 

**SEO** : Websites that have enable HTTPS are more likely to have a better rank on search engine. Since Google added a preference in its algorithm for sites running HTTPS.  

---

### Prerequisites for this recipe 

* Apache2 server on Linux ( hosted on DigitalOcean )

---

#### 1) Get a SSL certificate 

In order to get your SSL certificate, you got to pick a Certificate Authority (abbreviated by *CA*). I personnally chose a [NameCheap](https://www.namecheap.com) since a one-year SSL certificate is provided with the [Github Student Pack](https://education.github.com/pack/#namecheap). You'll need to provide Certificate Signing Request (CSR) containing your information and your website adresse, in order to have a CSR this you'll need to connect on your server and generate a private key:

	$ cd /etc/apache2
	$ mkdir ssl
	$ cd ssl
	$ openssl req -new -nodes -keyout myserver.key -out server.csr -newkey rsa:2048

#### 2) Activate the SSL module
	$ sudo a2ensmod ssl
	$ sudo service apache2 restart

#### 3) Install certificates on your server

Once your CA have sent the certificates, you'll have to import them on your server. Normally with Comodo if you picked *mod_ssl*, you will receive two certificates,  : *yourdomain*.crt and *yourdomain*.ca-bundle (this one is the Certificate Authority own certificate). 

If you use openssl and received 4 files : 

+*COMODORSADomainValidationSecureServerCA.crt* 
+*AddTrustExternalCARoot.crt* 
+*COMODORSAAddTrustCA.crt* 
+*yourdomain.crt* 

You'll have to concatenate these into one file CAcertifcate.ca-bundle

	$ cat COMODORSADomainValidationSecureServerCA.crt COMODORSAAddTrustCA.crt AddTrustExternalCARoot > mydomain.ca-bundle

*Note : I had many troubles installing the two certicates, even after creating my ca-bundle because I chose the wrong module. You might want to ask for a new certicate with the correct module ( mod_ssl or openssl ). I re-issue my certificate on my CA's website with mod_ssl and it did the trick for me.*

To install your certificate on your server, once you are connected via SSH on your server :

	$ cd /etc/apache2/ssl
	$ nano mydomain.ca-bundle 
	#Paste the content of yourdomain.ca-bundle sent by your CA
	$ nano mydomain.crt
	#Paste the content of yourdomain.crt sent by your CA

#### 4) Set up the directive 

The next step, you will have to indicate your server where are the certificate :

	$ vi /etc/apache2/sites-available/default-ssl.conf

And then set : 

	SSLEngine On
	SSLCertificateChainFile /etc/apache2/ssl/mydomain.ca-bundle
	SSLCertificateFile /etc/apache2/ssl/mydomain.crt
	SSLCertificateKeyFile /etc/apache2/ssl/myserver.key 


#### 5) Automatically redirect HTTP to HTTPS 

To redirect connection from the port 80 (HTTP) to the port 443 (HTTPS). You can also add at the top of the file 

	<VirtualHost *:80>
        ServerName yourdomain.com
        RewriteEngine On
        RewriteRule ^(.*)$ https://%{HTTP_HOST}$1 [R=301,L]
	</VirtualHost>


	$ sudo service apache2 restart











