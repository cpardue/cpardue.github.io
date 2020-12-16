---
layout:     post
title:      Installing Suricata via Smoothsec
date:       2019-12-05
summary:    Installing Suricata via the very cool Suricata/Snort Smoothsec project
categories: Homelab
thumbnail: cogs
tags:
 - Homelab
---
{% highlight ruby %}
# download smoothsec install iso from https://sourceforge.net/projects/smoothsec/
# burn onto a thumb drive
# boot machine into usb
# follow the install script
## installing from iso... ###############################################################################################
#setup new root passwd
#setup new user
#setup new user passwd
#setup snorby account info
#setup snorby account passwd
#reboot
# we have to update this before we move on, for reasons
npm update pigsty -g && npm update pigsty-mysql -g
###############################################
# include arpalert and fail2ban for securing the appliance
apt-get install arpalert openjdk-7-jre fail2ban
# update the arpalert oui list
cd /etc/arpalert/
mv oui.txt oui.txt.old
apt-get -y rsyslog
wget http://http://standards-oui.ieee.org/oui.txt
############################################################
# upgrade dist to jessie, since wheezy is EOL && dangerous #
############################################################
# first update all archived
sudo apt-get update
sudo apt-get upgrade
sudo apt-get dist-upgrade
# Next replace repositories "wheezy" with "jessie"
sudo sed -i /deb/s/wheezy/jessie/g /etc/apt/sources.list
sudo sed -i /deb/s/wheez/jessie/g /etc/apt/sources.list.d/*.list
# next update and import the new package lists
sudo apt-get update
# next upgrade new packages and upgrade wheezy to jessie
sudo apt-get upgrade -y
sudo apt-get dist-upgrade -y # follow the prompts
# finally clean up old packages and leftovers
sudo apt-get autoremove -y && sudo apt-get autoclean -y
###############################################
# back to initial configuration
# 1. visit http://<newIp>:443 and configure snorby
# 2. set everything else under Administration>
# 3. create a csv of your assets by ip,name and upload it to Snorby
# 4. set time zone under settings> 
# NOTE: you can also configure timezone as shown here (recommended): 
sudo nano /var/www/snorby/config/config/snorby_config.yml
# reflect these changes, mind the tabs
production:
  domain: demo.snorby.org
  wkhtmltopdf: /usr/local/bin/wkhtmltopdf
  ssl: false
  mailer_sender: snorby@snorby.org
  geoip_uri: http://geolite.maxmind.com/download/geoip/database/GeoLiteCountry/GeoIP.dat.gz
  authentication_mode: database
  time_zone: 'America/Chicago'
development:
  domain: localhost:3000
  wkhtmltopdf: /Users/mephux/.rvm/gems/ruby-1.9.2-p0/bin/wkhtmltopdf
  ssl: false
  mailer_sender: snorby@snorby.org
  geoip_uri: http://geolite.maxmind.com/download/geoip/database/GeoLiteCountry/GeoIP.dat.gz
  time_zone: 'Africa/Freetown'
###############################################
# add pulledpork to crontab
04 05 * * * /usr/local/bin/pulledpork.pl -c /etc/pulledpork/snort/pulledpork.conf -l
# install other useful stuff
apt-get install -y htop
apt-get install -y iftop
###############################################
# go into the following and configure according to your use case
/etc/smoothsec.cfg # check all options, ensure paths are correct, correct monitor interface and domain
/etc/arpalert.conf # check file paths
/etc/snort/snort.conf # pay attention to paths, disable/enable options, and rule path in section 7
/etc/suricata/suricata.yaml # pay attention to paths and disable/enable options
/etc/pulledpork/snort/* # enable rulesets one at a time and verify resource usage with htop
/etc/pulledpork/suricata/* # enable rulesets one at a time and verify resource usage with htop
###############################################
### Install is complete.  
###############################################	
### at this point, running htop while monitoring via Snorby shows that 
### snort deployment pulls about 0.8GB ram usage and 4-10% CPU.  
### suricata deployment pulls about 0.9GB ram usage and 5-12% CPU.  
### dual deployment pulls just over 2GB ram usage and 10-20% CPU.
### NOTE: looks like more can be done with suricata as far as tuning and alerting
########################################################################
################ moving to suricata performance tuning #################
### initial false positives and performance tuning
### always investigate and verify beyond shadow of doubt that it is a false positive
### always verify that performance tuning has not negatively impacted performance
############################## rules ###################################
Top False Positive generatoring rules: 
1:200920[5-7]
1:2027768
############################ performance ###############################
# 1. https://suricata.readthedocs.io/en/suricata-4.1.2/performance/tuning-considerations.html
# always allow some time for the machine to settle into it's new baseline
########################################################################
#### baseline before: 6.6%_CPU 1479MB_Mem 0MB_Swp ######################
########################################################################
sudo nano /etc/suricata/suricata.yaml
# uncomment max-pending-packets, set to 2048
# install tcmalloc, an optimized thread-caching malloc for C++ binaries
apt-get install libtcmalloc-minimal4
# load tcmalloc before and instead of malloc
sudo nano /etc/init.d/suricata
# under start and restart, change this
/usr/local/bin/suricata --user suricata -c /etc/suricata/suricata.yaml -i $INTERFACE_DEF -D
# to this
LD_PRELOAD=”/usr/lib/libtcmalloc_minimal.so.0" /usr/local/bin/suricata --user suricata -c /etc/suricata/suricata.yaml -i $INTERFACE_DEF -D
# now run the restart script and verify that suricata is still working
ps ax | grep suricata  # should show the original path under instance
htop  # should be less Mem usage
########################################################################
#### baseline changed: 7.1%_CPU 1159MB_Mem 0MB_Swp #####################
########################################################################


# 2. https://suricon.net/wp-content/uploads/2016/11/SuriCon2016_MichalPurzynski_PeterManev.pdf
{% endhighlight %}
