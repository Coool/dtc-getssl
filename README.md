<h1>Let's Encrypt DTC</h1>                                                            
I've been hacking away at getting DTC set up to use Googles' Let's Encrypt.</br>
Here's my work so far…</br>
https://www.yournet.co.nz/getssl/</br>
There are two files:
<ul>
<li>getssl – the getssl script that you get from the getssl site.</li>
<li>dtc-getssl</li>
</ul>
<h2>About dtc-getssl</h2>
This is a wrapper around getssl to do the stuff that DTC needs done to make it work.</br>
To execute you:</br>
<ul>
<li>Need to have the files in /home/dtc</li>
<li>Run ./dtc-getssl -a &lt;ADMIN NAME&gt; -d &lt;DOMAIN NAME&gt; -s &lt;SUB DOMAIN&gt; -c</br>
Where</br>
* ADMIN NAME is the DTC Admin name of the account that the domain is located in.</br>
* DOMAIN NAME is the domain name you want the cert for.</br>
* SUB DOMAIN is the subdomain of the domain you want the cert for.</br>
What we're doing is just creating the right stuff with the right permissions so it will all work in DTC.</br>
eg:&nbsp; ./dtc-getssl -a deafblindassociation -d deafblindassociation.nz -s www -c</br>
getssl will create you a folder for the sub/domain combination in the .getssl folder.</br>
dtc-getssl wil then display a bunch of information that you need to copy into the getssl.cfg file.</br></li>
<li>Then edit the getssl.cfg file for the domaineg:&nbsp; /home/dtc/.getssl/www.deafblindassociation.nz/getssl.cfg</br>
In the case of our example:</br>
#This tells getssl where to find the file it makes so that it can verify we actually own the domain.</br>
ACL=('/var/www/sites/deafblindassociation/deafblindassociation.nz/subdomains/www/html/.well-known/acme-challenge')</br>
#This tells getsll to use the ACL above for all and any verification's even if we're getting a cert for more than one subdomain (which I don't think we should be).</br>
USE_SINGLE_ACL="true"</br>
#These lines just tell getssl where to put the files once it's made them.</br>
DOMAIN_CERT_LOCATION="/var/www/sites/deafblindassociation/deafblindassociation.nz/subdomains/www/ssl/www.deafblindassociation.nz.cert.cert"</br>
DOMAIN_KEY_LOCATION="/var/www/sites/deafblindassociation/deafblindassociation.nz/subdomains/www/ssl/www.deafblindassociation.nz.cert.key"</br>
CA_CERT_LOCATION="/var/www/sites/deafblindassociation/deafblindassociation.nz/subdomains/www/ssl/www.deafblindassociation.nz.cert.ca"</br>
You also need to make sure the production ssl server isn’t commented out and that the test one is.</br>
# The staging server is best for testing</br>
#CA="https://acme-staging.api.letsencrypt.org"</br>
# This server issues full certificates, however has rate limits</br>
CA="https://acme-v01.api.letsencrypt.org"</br>
Finally, comment out the SANS option unless you have reason for it.&nbsp; You'll see in our example the getssl script seemed to think we want a subdomain included that we don't.</br>
#SANS="dtc.yournet.co.nz"</br></li>
<li>Now Run ./dtc-getssl -a &lt;ADMIN NAME&gt; -d &lt;DOMAIN NAME&gt; -s &lt;SUB DOMAIN&gt; without the -c option</br>
You should see getssl generate the keys for you.</br>
We need this wrapper because we're running the script with the correct user (dtc) so that we get the correct permissions on the file.</br></li>
<li>Restart apache2</br>
getssl does have the ability to restart the web server and we will need to do this in future, but this script is way to green to be letting it restart your production system without doing a bit of checking first!</br></li>
</ul>
