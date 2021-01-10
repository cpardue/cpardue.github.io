---
layout:     post
title:      TCM PEH Linux Notes
date:       2021-01-10
summary:    TheCyberMentor's Practical Ethical Hacking Course Linux Notes
categories: Certs
thumbnail: jekyll
tags:
 - Linux
 - Certs
---

These are the notes I've taken for TheCyberMentor Academy's Practical Ethical Hacking - Intro to Linux section

new kali and sudo
    new kali uses non-privileged user
    use sudo to run as root now, duh
    su - to switch to root with kali's pw, duh
    
navigating filesystem
    pwd
        print present working directory
    cd ..
        change dir up one dir
    ls -lha
        list contents (long, human readable, all)
    ~
        shorthand for home dir
    mkdir newFolder
        make directory newFolder here
    echo “this text” > newfile
        output “this text” overwritten into newfile
    cp /location/A/file /location/B/file
        copy file from A to B
    mv /path/A/file /path/B/FILE
        move and rename file from A to B
    updatebd
        update cache of locations and paths
            do this semi-frequently
    passwd
        change current user password
            enter twice to over-ride complexity rules
    man command
        manpage for command
    command --help
        abbreviated manpage (sometimes)

Users and Privileges
    rwx/rwx/rwx owner group
        owner/group/all owner group
    rwx = 421 bits
        777 = rwxrwxrwx
        111 = r--r--r--
        123 = r---w-rw-
    chmod -switch file
        switch can be bits or switch
        switch can set sticky bit too
            set sticky bit = u+s
            remove sticky bit = u-s
    adduser
        check the man for the cool switches
    addgroup
        again check the man for the cool options
    deluser
        delete user
    usermod
        modify user
    passwd
        passwd -switch username
    su username
        switch to user username

Common Network Commands
    ifconfig
        legacy ip addr
    iwconfig
        wireless    
    ip addr
        replaced ifconfig
        ip addr | grep ‘inet ’
    ping -c n ip
        ping n times this ip
    arp -a
        check ip and mac directly associated
    netstat
        view connections
        netstat -ant 
            tcp
        netstat -anu
            udp
    route
        prints routing table

Installing and Updating Tools
    sudo apt-get update -y && sudo apt-get upgrade -y
    sudo apt-get dist-upgrade -y
    sudo apt-get install pip3
    pimpmykali.sh
        rm -rf pimpmykali/
        git clone https://github.com/Dewalt-arch/pimpmykali
        cd pimpmykali
        sudo ./pimpmykali.sh
            select 0
    sudo apt install gedit -y
    sudo apt install flameshot -y

Viewing, Creating, and Editing Files
    echo ‘string’ > textfile
    echo ‘string’ >> textfile
        appends the data
    cat filename
    more filename
    less filename
        less is more than more
    head -n filename
        list first n lines of filename
    tail -n filename
        list last n lines of filename
    tail -f filename
        cats tail of filename perpetually, good for logs
    touch filename
        changes timestamp to time touched
        creates file that did not previously exist
        
