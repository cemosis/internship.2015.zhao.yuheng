#Websites Organization
The following schemas explain how all the websites are going to be deployed and hosted.

## Before migrations
### www.cemosis.fr on Google Sites
![cemosis](cemosis.png)
### www.feelpp.org and doc.feelpp.org hosted their on their Github repositories' `gh-pages` branch
![feelpp](feelpp.png)
## After migrations
A virtual machine on Amazon Web Services was provided by IRMA, a domain name belongs to the university was also allocated to that machine.
For each website hosted on that machine that has a different domain name, its domain name will point to the machine's IP address. After that Nginx web server installed on that machine will handle the redirections between several different websites.
### www.cemosis.fr on the machine `csmi.math.unistra.fr` 
![cemosis-new](cemosis-new.png)
The website of Master CSMI will be deployed on that machine and an extra copy(alias csmi.cemosis.fr) will also be deployed on its Github repository's `gh-pages` branch.
### csmi.math.unistra.fr on the machine `csmi.math.unistra.fr` and csmi.cemosis.fr on Github `gh-pages` branch
![](csmi.png)
### www.feelpp.org on the machine `csmi.math.unistra.fr`
![](feelpp-org.png)
### doc.feelpp.org on the machine `csmi.math.unistra.fr`
![](feelpp-doc.png)
##Final schema on the machine `csmi.math.unistra.fr`
![](final.png)
