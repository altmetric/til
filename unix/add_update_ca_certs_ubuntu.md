# How to add/update CA certificates on Ubuntu Linux

You may occasionally run into a situation where you or a piece of software you are working with is trying 
to access an HTTPS website and coming up with an SSL error. An example of such an error returned by curl is
listed below:
```
user@host$ curl 'https://www.somewebpage.com' 
curl: (60) SSL certificate problem, verify that the CA cert is OK. Details:
error:14090086:SSL routines:SSL3_GET_SERVER_CERTIFICATE:certificate verify failed
More details here: http://curl.haxx.se/docs/sslcerts.html
```

This is very often caused by the fact that the local system does not have a copy of the issuing CA certificate 
for the company that issued the SSL certificate of `www.somewebpage.com`. This situation itself can arise from a 
variety of reasons, but the easiest fix is to just download that CA Certificate to the local system and update 
the CA cert repository using the following steps:

* Run the following command to get the SSL certificate information from `www.somewebpage.com`
```
openssl s_client -showcerts -connect www.somewebpage.com:443
```
* Look for the a line that looks like this near the top of the output:
```
Certificate chain
 0 s:/C=AU/ST=Some State/L=Some City/O=Some Organisation Name/OU=Some Division/CN=www.somewebpage.com
   i:/C=BM/O=QuoVadis Limited/CN=QuoVadis Global SSL ICA G2
```
and copy the value from the `CN=` field on the line that starts with `i:` (for issuer), in this case `QuoVadis Global SSL ICA G2`
* First have a look in `/usr/local/share/ca-certificates` and check that there is no CA certificate for that specific 
CN already
  * If there is, you can  try and re-download/update it following the steps below 
  * If that still does not work it means that you have a different problem and this guide is useless to you
* Take the value above and Google for that value + something like 'CA certificate' or 'download CA certificate'
* When you have located the CA certificate (must be in .pem or .crt format, not .crl) either copy/paste it to a file on 
the server or use `wget` to download it to the server
* Move the file into /usr/local/share/ca-certificates with the .crt extension
```
mv /home/user/CAcertificate.pem /usr/local/share/ca-certificates/CAcertificate.crt
```
* Run the following command to update the CA certificate store
```
sudo update-ca-certificates
```
* Re-run your initial curl/software to confirm if the issue has been fixed

