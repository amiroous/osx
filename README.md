## OS X's BSD/unix Commands 
[Ref.](http://www.westwind.com/reference/os-x/commandline/)

| Command | Definition |
| ------- | ---------- |
| `pwd` | print working directory |
| `cd` | moves into the directory |
| `mkdir` `rmdir` | makes/remove a directory |
| `cp` | copy |
| `mv` | move / rename |
| `rm` `rm -rf` | remove |
| `ls` `ls -a` `ls -a` | list files in current directory |
| `who` `whoami` | who is logged in |
| `grep` `grep -i` `grep -r` | search a file with pattern |
| `find` | find files using file-name |
| `ssh` `ssh -l` | login to remote host |
| `chmod` `chmod -R` | change the permissions for a file or directory |
| `chown` `chown -R` | change the owner and group of a file |
| `ifconfig` | view or configure a network interface |
| `uname -a` | displays important information about the system |
| `whereis` | find out where a specific Unix command exists |
| `whatis` | displays a single line description about a command |
| `locate` | search for the location of a specific file(s) |
| `man` | display the manual page of a specific command |
| `hostname` | prints the system's host name |
| `which <executable>` | identify the location of executable |
| `where <executable>` | identify the location of executable |


## [Brew](https://docs.brew.sh/FAQ.html)
> mainstream package manager for macOS
> Sudo is dangerous, don't use it for install packages
- Install: `/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"`
- Uninstall: `ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/uninstall)`
- Install xCode: `xcode-select --install`

> Homebrew installs packages to their own directory and then symlinks their files into `/usr/local/Cellar`.
```
brew install wget

cd /usr/local
find Cellar
 Cellar/wget/1.16.1
 Cellar/wget/1.16.1/bin/wget
 Cellar/wget/1.16.1/share/man/man1/wget.1

ls -l /usr/local/bin
> bin/wget -> ../Cellar/wget/1.16.1/bin/wget
```

> Where does stuff get downloaded?
`brew --cache` => `~/Library/Caches/Homebrew`

| Command | Definition |
| ------- | ---------- |
| `brew update` | update the local base of available packages and versions |
| `brew outdated` | find out what is outdated |
| `brew upgrade` | actually installs new version of outdated packages |
| `brew cleanup -n` | see what would be cleaned up |
| `brew cleanup` | clean up everything |
| `brew cleanup -s` | clean up everything but keep only linked versions |
| `brew cask cleanup` | cleanup for unfinished download |
| `brew doctor` | show you any problem with your brew installation |
| `brew missing` | show you any problem with your brew installation |
| `brew list -1` | list of all installed packages |
| `brew list -1 `|` xargs brew rm` | remove all packages together but keep homebrew |
| `brew search ...` | search for a package |
| `brew tap` | list tapped repositories |
| `brew tap <tapname>` | add tap |
| `brew untap <tapname>` | remove a tap |
| `brew install <packagename>` | install a package |
| `brew uninstall <packagename>` | uninstall a package |
| `brew rm <packagename>` | uninstall a package |
| `brew prune` | remove broken symlinks |


## LAMP stack
> Linux, Apache, MySQL, and PHP/Python/Perl
(LAMP on MAC OS)[https://websitebeaver.com/set-up-localhost-on-macos-high-sierra-apache-mysql-and-php-7-with-sslhttps]

### Apache
> Mac OS default path to apache is `/etc/apache2/`

Important Files:
- `/etc/hosts`
- `/etc/apache2/httpd.conf`
- `/etc/apache2/extra/httpd-vhosts.conf`
- `/etc/apache2/extra/httpd-ssl.conf`
- `/etc/ssl/openssl.cnf`


| Command | Definition |
| ------- | ---------- |
| `ifconfig` | Network interfaces configurations |
| `sudo lsof -i` | List all open network connections on the entire system |
| `sudo apachectl stop` | Stop apache server |
| `sudo apachectl start` | Start apache server |
| `sudo apachectl restart` | Restart apache server |
| `apachectl -V` or `httpd -V` | apache version |
| `apachectl configtest` |  |



### PHP
> Mac OS default path to PHP is `/usr/bin/php`

| Command | Definition |
| ------- | ---------- |
| `php -i` | current PHP info |
| `php --version` | current PHP version |

**Install PHP Through Homebrew**
```text
brew tap homebrew/php
brew update
brew install php71 --with-httpd
brew search xdebug
brew install php71-xdebug
brew search mcrypt
brew install php71-mcrypt
```
Path to PHP config `/usr/local/etc/php/7.1/php.ini`


### MySQL

**Remove MySQL Instance**
- Back Up all databases (if needed): `mysqldump --all-databases > all_databases_export.sql`
- Back databases individually (if needed): `mysqldump database_name > database_exportname.sql`
- Stop the database server `sudo launchctl unload -F /Library/LaunchDaemons/com.oracle.oss.mysql.mysqld.plist`
- Remove MySQL
```apacheconfig
sudo rm -rf /usr/local/mysq*
sudo rm  /Library/LaunchDaemons/com.oracle.oss.mysql.mysqld.plist
sudo rm -rf /Library/StartupItems/MySQLCOM
sudo rm -rf /Library/PreferencePanes/My*
```

### SSL/HTTPS

1. `/etc/apache2/httpd.conf`
```text
LoadModule socache_shmcb_module libexec/apache2/mod_socache_shmcb.so
LoadModule ssl_module libexec/apache2/mod_ssl.so
Include /private/etc/apache2/extra/httpd-ssl.conf
```

2. `/etc/apache2/extra/httpd-ssl.conf`

Replace `www.example.com:443` with `localhost`
Replace `/Library/WebServer/Documents` with `/Users/<whoami>/Sites`
Add in under `<VirtualHost_default_:443>`:
```text
<Directory "/Users/<whoami>/Sites">
  Options All
  MultiviewsMatch Any
  AllowOverride All
  Require all granted
</Directory>
```
_Certificate files will be saved in `/private/etc/apache2/`_

3. `/etc/ssl/openssl.cnf`
```text
[ req ]
#default_bits			= 2048
#default_md				= sha256
#default_keyfile 		= privkey.pem
distinguished_name		= req_distinguished_name
attributes				= req_attributes
req_extensions 			= v3_req

[ req_distinguished_name ]
countryName				= Country Name (2 letter code)
countryName_min			= 2
countryName_max			= 2
stateOrProvinceName		= State or Province Name (full name)
localityName			= Locality Name (eg, city)
0.organizationName		= Organization Name (eg, company)
organizationalUnitName		= Organizational Unit Name (eg, section)
commonName			= Common Name (eg, fully qualified host name)
commonName_max			= 64
emailAddress			= Email Address
emailAddress_max		= 64

[ req_attributes ]
challengePassword		= A challenge password
challengePassword_min	= 4
challengePassword_max	= 20

[ san ]
subjectAltName          = @alt_names

[ v3_req ]
# Extensions to add to a certificate request
basicConstraints = CA:FALSE
keyUsage = nonRepudiation, digitalSignature, keyEncipherment
subjectAltName = @alt_names

[alt_names]
DNS.1 = localhost
DNS.2 = *.localhost.com
```
4. Run following commands in order:
`sudo openssl req -extensions san -config /etc/ssl/openssl.cnf -x509 -nodes -newkey rsa:4096 -keyout /private/etc/apache2/server.key -out /private/etc/apache2/server.crt -days 365 -subj "/C=US/ST=California/L=San Diego/O=AMIR/CN=localhost"`
`sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain /private/etc/apache2/server.crt`
`sudo apachectl restart`


### Virtual Host

1. Activate V-Host: `/etc/apache2/httpd.conf`
```text
Include /private/etc/apache2/extra/httpd-vhosts.conf
```

2. Define V-Hosts: `/etc/apache2/extra/httpd-vhosts.conf`
```text
<VirtualHost *:80>
    ServerAdmin webmaster@sandbox.app
    DocumentRoot "/Users/<whoami>/Sites/sandbox"
    ServerName sandbox.localhost.com
    ServerAlias www.sandbox.localhost.com
</VirtualHost>

<VirtualHost *:443>
    ServerName sandbox.localhost.com
    DocumentRoot "/Users/<whoami>/Sites/sandbox"

    SSLEngine on
    SSLCipherSuite ALL:!ADH:!EXPORT56:RC4+RSA:+HIGH:+MEDIUM:+LOW:+SSLv2:+EXP:+eNULL
    SSLCertificateFile /private/etc/apache2/server.crt
    SSLCertificateKeyFile /private/etc/apache2/server.key

    <Directory "/Users/<whoami>/Sites/sandbox">
        Options Indexes FollowSymLinks
        AllowOverride All
        Order allow,deny
        Allow from all
        Require all granted
    </Directory>
</VirtualHost>
```

3. Map in `/etc/hosts` file
```text
127.0.0.1   sandbox.localhost.com
```

## Node & [NPM](https://docs.npmjs.com/getting-started/publishing-npm-packages)

| Command | Definition |
| ------- | ---------- |
| `brew install node` | install Node.js and npm |
| `node --version` & `npm --version` | check versions |
| `brew update && brew upgrade node` | update node and npm |
| `npm init` | create a `package.json` file |
| `npm install -g <package-name>` | install a package globally |
| `npm install -g –save-dev <package-name>` | install a package globally, save into `package.json` as devDependencies |
| `npm list -g` | all packages that are installed globally |
| `npm list -g <package-name>` | check if a package is installed globally |
| `npm install –save-dev <package-name>` | install a package locally |
| `npm update` | update packages locally |
| `npm uninstall <package-name>` | remove a package |
| `npm config set registry https://registry.npmjs.org/` | set registry path |

- "dependencies": These packages are required by your application in production.
- "devDependencies": These packages are only needed for development and testing.



## Git & GitHub

[Atlassian Site](https://www.atlassian.com/git/tutorials/setting-up-a-repository/git-config)

| Command | Definition |
| ------- | ---------- |
| `` |  |



## Useful Software:
* Adobe Photoshop
* Docker & Kitematic
* iTerm & OhMyZsh
* Alfred 3
* Blue Jeans
* Caffeine
* Duet
* FileZilla
* Firefox
* Flux
* Google Chrome
* Google Chrome Canary
* ImageOptim
* IntelliJ IDEA
* Microsoft Office Package
* Postman
* Sequel Pro
* Slack
* Sketch
* Snagit
* Sourcetree
* Spectacle
* Spotify
* Steam
* Sublime Text 3
* Visual Studio Code
* VLC Player
* XCode
* Zeplin
