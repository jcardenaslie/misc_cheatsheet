"sudo" standa for "Super Do" - prefix to any other command to run at Super User Admin Level.


sudo nano /etc/ssh/sshd_config              = edit the sshd_config file 
sudo nano /home/bank2/.ssh/authorized_keys  = edit the author 
sudo /etc/init.d/ssh restart                = restart ssh server 
sudo systemctl status ssh                   = show status of ssh server 
sudo service ssh status                     = show status of ssh server 
service --status-all                        = gives a nice listing of all services and a plus if started, minus if not 
lsof -i:22                                  = show all programs listening on Port 22 
ip addr show                                = show the servers IP Address 
sudo apt-get install coreutils              = so you can use the "cat" command 
cat anyfilename.txt                         = list the file on the console 
tail anyfilename.txt                        = list last few lines of file on the console 
tail -20 /var/log/auth.log                  = show last 20 lins of a file (change to ## desired) 
sudo ufw disable                            = disable firewall 
chmod 600 ~/.ssh/id_rsa                     = to fix warning below on a key file  WARNING: UNPROTECTED PRIVATE KEY FILE! Permissions 0664 for "xxxxxx.ppk" are to open. 
sftp -i SFTPUser1_Private.pem SFTPUser3@192.168.1.179 = SFTP using .pem keyfile and use SFTP on some server 
chmod +x filename.sh                        = Make a file executable (for Bash Scripts) 
./filename.sh                               = Execute a Bash Script  
sudo nano /etc/crontab                      = Edit the global cron jobs 
sudo systemctl restart cron                 = Restart the cron service/daemon 
grep CRON /var/log/syslog                   = Look for the text "CRON" in a given filename (the syslog) 
ssh-keygen -t rsa -b 4096                   = Create a new private/public key pair using RSA algo at 4096 bits 
ssh-copy-id -i /.ssh/keyfile user@host      = copy a key file to another server's authorized_keys file 
scp file user@HostServer:path               = run the "secure copy" command to copy file to some path on another computer running SSH


-----------------------------------------------------------------------------------------------------------------------------------------------

SOURCE: https://gist.github.com/lymanlai/3008244

1 - Create SSHD_CONFIG Rule

Match group sftpgroup
 	ChrootDirectory %h
  X11Forwarding no
  AllowTcpForwarding no
 	ForceCommand internal-sftp


2 - Create Group

sudo addgroup sftpgroup

3 - Create User

sudo adduser bech-docs

4 - Add user to new group and set permissions

sudo usermod -G sftpgroup bech-docs
sudo chown root:root /home/bech-docs
sudo chmod 755 /home/bech-docs

5 - Create directories for user and set final permissions:

cd /home/bech-docs
sudo mkdir bech-to-guru guru-to-bech
sudo chown bech-docs:bech-docs *



sudo chown root:root /home/bech-docs-cert
sudo chmod 755 /home/bech-docs-cert

sudo mkdir bechcert-to-guru guru-to-bechcert
sudo chown bech-docs-cert:bech-docs-cert *


sudo addgroup sftpcertpassgroup
sudo adduser bechcertpass
sudo usermod -G sftpgroup bechcertpass
sudo chown root:root /home/bechcertpass
sudo chmod 755 /home/bechcertpass
cd /home/bechcertpass
sudo mkdir bechcertpass-to-guru guru-to-bechcetrpass
sudo chown bechcertpass:bechcertpass *