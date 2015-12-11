# Tor onion website

## What does it do:
* Create a Tor .onion website with a couple of static HTML pages

## Project goals:
* Make it really easy to setup and maintain a Tor .onion website
* Include HTTPS with strong crypto and forward secrecy
* Make it decently secure out of the box (caveat emptor)
* Provide a basic level of anonymity for the visitors and webmaster
* Make it plug-and-play

## What can I do with it:
* It ships with a couple of static HTML pages which you can use to provide some basic information about yourself, like your: email address, PGP key, XMPP address, OTR fingerprint, bitcoin address and services you provide.
* You can of course expand it to be a regular website or blog.
* You can run the .onion website from your own home without changing the firewall or NAT configuration of the router, but you can also run it on a remote server.

## Software:
* Tor is used for setting up a Tor .onion website
* Nginx is used for serving webpages
* Ansible is used for automatic system configuration

## HTTPS/SSL setup:
* Automatic configuration of unique dhparam file
* Automatic configuration of self-signed certificates for .onion domains
* Automatic configuration of HSTS/STS (currently limited browser support for self-signed certificates)
* Automatic configuration of HPKP fingerprints (currently limited browser support for self-signed certificates)
* Automatic configuration of .onion subdomains with SSL fingerprints as the subdomains

## Additional features:
* Automatic system hardening
* Automatic cleanup of unnecessary software
* Automatic installation of system updates
* Automatic cleanup of system logs
* Automatic system configuration checks with Ansible
* The firewall only allows Tor traffic in and out, with exception of DHCP traffic.
* Network card MAC address randomization

## OS support:
* Raspbian Jessie (lite)
* Debian Jessie

## Hardware support:
* The system runs on a Raspberry Pi, Beaglebone and other cheap hardware
* Any system with 512 MB RAM or more will probably do fine

## For who is this project intended:
* Anyone who wants to run their own .onion website
* Anyone who doesn't want to be dependent on (proprietary) third party services for a website
* Anyone who wants to take privacy and security into their own hands
* Anyone who is an crypto enthusiast

## For who is this project not intended:
* Anyone who does not want to be listed as a Tor user.
* Anyone who's personal freedom depends on keeping their anonymity secure on the net. This product is too limited for that purpose.

## License:
* Consider the code to be public domain. If you or your jurisdiction do not accept that then consider the code to be released under Creative Commons 0 (CC0). If you or your jurisdiction do not accept that... well then settle for the MIT license. What we mean to say is that you are free to copy, modify and relicense the code by all means. But don't hold us liable for any damages incurred by using or abusing the software.
* Code which is copied from other projects remains under the original license.

## General security notes:
* For maximum security of the encryption keys it is best to have complete control over the hardware. This probably means a compromise on anonymity for the webmaster.
* For maximum anonymity for the webmaster it is best to run the .onion website at a remote location. This probably means a compromise on system security. This is especially true when the system is run on a VPS. When the system in run on a VPS you have to assume that the encryption keys are compromised, because you have no control over the hardware.

## Important installation notes:
* By default SSH is removed from the system for security reasons. To change this behavior you have to comment out the 'openssh-server-cleanup' role in /etc/ansible/localhost.yml.
* By default incoming SSH traffic is blocked by the firewall for security reasons. To change this behavior you have to change the default firewall rules to permit incoming SSH traffic. Edit /etc/ansible/roles/iptables-pre-tor/templates/iptables.conf.j2 and /etc/ansible/roles/iptables/templates/iptables-{distro}.conf.j2. Uncomment the 'Permit incoming SSH traffic' rule in both files.
* The system assumes only one network card is available.


## How to install the .onion website on Debian Jessie and Raspbian Jessie (lite)
1. Install the OS

2. Install Ansible and Git
    ```
    sudo apt-get update && sudo apt-get install ansible git -y
    ```

3. Clone the git repository
    ```
    sudo git clone https://github.com/garlicgambit/onion-website.git /etc/ansible
    ```

4. Set the correct time on the system for UTC 0
    ```
    sudo date -s “dd mmm yyyy hh:mm:ss”
    ```

    Example:
    ```
    sudo date -s “01 Jan 2014 14:30:00”
    ```

5. Go to the Ansible configuration directory
    ```
    cd /etc/ansible
    ```

6. Run Ansible as a user with administrative privileges
    ```
    sudo ansible-playbook localhost.yml
    ```

7. On modern hardware the installation will take less then 5 minutes

8. After Ansible is done the system will automatically reboot

9. When the system is freshly booted you will have a .onion website running


## Configure basic contact information for the .onion website:
1. Go to the Ansible configuration directory
    ```
    cd /etc/ansible
    ```

2. Open the file group_vars/localhost with your favorite editor
    ```
    sudo nano group_vars/localhost
    ```

3. To add an email address, look for contact_email: '' and add the email address between the single quotes.

    Example:
    ```
    contact_email: 'name@example.com'
    ```

4. Repeat this process for other contact information you want to add.

5. Save the file

6. Run Ansible to immediately deploy the change
    ```
    sudo ansible-playbook localhost.yml
    ```

7. Or wait for 30 minutes for automatic deployment

8. Make sure you verify the changes are correctly displayed on the .onion website


## Configure GPG key to be available for download:
1. Put the GPG key on the .onion device

2. Move the key to /etc/ansible/roles/nginx/files/gpg.pub (or change the location in /etc/ansible/group_vars/localhost)

3. Run Ansible to immediately deploy the GPG key
    ```
    sudo ansible-playbook localhost.yml
    ```

4. Or wait for 30 minutes for automatic deployment

5. It will now be available to download on the .onion website

6. Make sure you verify that the GPG key is available to download from the .onion website

## Contribute
Like this project? Help us out! Lots of work still needs to be done and any sort of help is appreciated:
* We really need more testers. Just grab the code, install it, run it, break it and send us feedback
* Contributions in all forms and sizes are greatly appreciated: ideas, comments, code, suggestions, donations, infrastructure, etc

If you have any questions, comments or suggestions you can contact us at:
jmercier@openmailbox (dot) org

GPG key:
0xF7698FEE3295ABB5

Support this project by donating to:
Bitcoin: 17P396Xc3cwMtBaa8bvznEHrLx42Y5NFEp
Monero: 463DQj1ebHSWrsyuFTfHSTDaACx3WZtmMFMwb6QEX7asGyUBaRe2fHbhMchpZnaQ6XKXcHZLq8Vt1BRSLpbqdr283QinCRK
