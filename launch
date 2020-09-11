#DNS name server:
echo nameserver 8.8.8.8 > /etc/resolv.conf

#server ntp
apt install -y ntp ntpdate
systemctl enable ntp.service
systemctl restart ntp.service
systemctl restart ntpdate

#Configuration NTP
sed -i '10c \NTPSERVER="10.2.0.1"\' /etc/default/ntpdate
sed -i '23,26c \server "10.2.0.1"\' /etc/ntp.conf
/etc/init.d/ntp stop
ntpd -q -g
/etc/init.d/ntp start

#Modification du fuseau horaire
rm /etc/localtime  
ln -s /usr/share/zoneinfo/Europe/Paris /etc/localtime

#Upgrade
apt update
apt -y full-upgrade

#install sudo:
apt install -y sudo mc wget ansible ssh git docker bc build-essential dkms rsync raspberrypi-kernel-headers

#Configuration SSH:
echo PermitRootLogin=yes >> /etc/ssh/sshd_config

#Restart SSH:
systemctl enable ssh
systemctl restart sshd.service

#Configuration rsyslog.conf:
echo *.* @@10.1.6.13:514 >> /etc/rsyslog.conf
systemctl restart rsyslog.service

#Creation dossier partage
mkdir /mnt/servrpi
echo 10.1.6.13:/ /mnt/servrpi nfs4 rw,hard,intr,_netdev 0 0 >> /etc/fstab
mount -a

#envoi de l'adresse IP sur fichier ipraspberry
IPRASP=$(ip a | grep '10.1.6' | cut -d ' ' -f6 | cut -d '/' -f1)
echo $IPRASP >> /mnt/servrpi/export/exportrpi/hosts

#Change password
echo "root:123" | chpasswd

#change language of the keyboard
echo XKBMODEL="pc105" > /etc/default/keyboard
echo XKBLAYOUT="fr" >> /etc/default/keyboard
echo XKBVARIANT="latin9" >> /etc/default/keyboard
echo BACKSPACE="guess" >> /etc/default/keyboard

#Kill Cron:
sed -i '/launch/d' /etc/crontab

#Remove the script "launch.sh" with a cron:
echo @reboot root rm -f /etc/launch.sh >> /etc/crontab

reboot
