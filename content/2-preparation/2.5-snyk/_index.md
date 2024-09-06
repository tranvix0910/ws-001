---
title : "Source Code Security - Snyk"
date :  "`r Sys.Date()`" 
weight : 5 
chapter : false
pre : " <b> 2.5 </b> "
---
#### Installing NodeJS and npm
```shell
sudo apt install npm -y
```
Check the version:
```shell
tranvi0910@deployment-server:~$ node -v
v18.20.4
```
```shell
tranvi0910@deployment-server:~$ npm -v
v10.7.0
```

#### Installing Snyk with npm
Install Snyk on the **Build Server**:
```sh
npm install snyk -g

# Format HTML

npm install snyk-to-html -g
```
```shell script
root@build-server:~# npm install snyk -g

added 36 packages in 56s

12 packages are looking for funding
  run `npm fund` for details
root@development-server:~# npm install snyk-to-html -g

added 23 packages in 5s

1 package is looking for funding
  run `npm fund` for details
```

Log in to the official Snyk website and proceed with authentication.

- [Link Snyk](https://app.snyk.io/)

![alt text](/images/2-preparation/2.5-snyk/2-5-1.png)
![alt text](/images/2-preparation/2.5-snyk/2-5-2.png)

Get your **API Token** from your account:

![alt text](/images/2-preparation/2.5-snyk/2-5-3.png)

Run the following command on the **Development Server** to authenticate Snyk:

```sh
snyk auth ab089484-2b45-4f10-b991-xxxxxxxxxxx
```
```sh
root@build-server:~# snyk auth ab089484-2b45-4f10-b991-d101f237fecb
Executable doesn't exist, trying to download.
Downloading from 'https://static.snyk.io/cli/v1.1292.1/snyk-linux' to '/usr/lib/node_modules/snyk/wrapper_dist/snyk-linux'
Shasums:
- actual:   xxxxxxxxxxxxxxx
- expected: xxxxxxxxxxxxxxx
Downloaded successfull!

Your account has been authenticated. Snyk is now ready to be used.
```

Navigate to the project directory and proceed with testing:

```
cd /projects/wineapp/wineapp-frontend

snyk test
```

![alt text](/images/2-preparation/2.5-snyk/2-5-4.png)

You can view detailed issues using the following command:

```
snyk monit
```
![alt text](/images/2-preparation/2.5-snyk/2-5-5.png)

![alt text](/images/2-preparation/2.5-snyk/2-5-6.png)

You can test and save the results to an HTML file:

```sh
snyk test --json | snyk-to-html -o <name_file>.html
```
![alt text](/images/2-preparation/2.5-snyk/2-5-7.png)

Thus, Snyk has been installed and configured successfully.

