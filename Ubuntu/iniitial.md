## Ubuntu

To see information about unused and used memory and swap space on your custom
'''
free
'''

To see details about the RAM installed on your VM
```
sudo dmidecode -t 17
```

To verify the number of processors
```
nproc
```

To see details about the CPUs installed on your VM
```
lscpu
```
---

To create a directory that serves as the mount point for the data disk

```
sudo mkdir -p /home/minecraft
```

To format the disk

```
sudo mkfs.ext4 -F -E lazy_itable_init=0,\
lazy_journal_init=0,discard \
/dev/disk/by-id/google-minecraft-disk
```

To mount the disk

```
sudo mount -o discard,defaults /dev/disk/by-id/google-minecraft-disk /home/minecraft
```

To install screen

```
sudo apt-get install -y screen
```

To create backup script

```
sudo nano /home/minecraft/backup.sh
```

```
#!/bin/bash
screen -r mcs -X stuff '/save-all\n/save-off\n'
/usr/bin/gsutil cp -R ${BASH_SOURCE%/*}/world gs://${YOUR_BUCKET_NAME}-minecraft-backup/$(date "+%Y%m%d-%H%M%S")-world
screen -r mcs -X stuff '/save-on\n'
```

To make the script executable
```
sudo chmod 755 /home/minecraft/backup.sh
```

In the mc-server SSH terminal, open the cron table for editing:

```
sudo crontab -e
```

When you are prompted to select an editor, type the number corresponding to nano, and press ENTER.

At the bottom of the cron table, paste the following line:

```
0 */4 * * * /home/minecraft/backup.sh
```

In the mc-server SSH terminal, run the following command:

sudo screen -r -X stuff '/stop\n'