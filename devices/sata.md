# SATA

This page describes SATA storage related commands and settings for different hardware platforms


```
# remove partirion table, if any
/bin/dd if=/dev/zero of=/dev/sda bs=1000000 count=1

# create a new partition table
/usr/sbin/parted /dev/sda mktable gpt

# create an ext4 partition in entire drive
/usr/sbin/parted /dev/sda mkpart ext4 1MB 480GB

# format the partition as ext4
/sbin/mkfs.ext4 -F /dev/sda1

# create a mounting point
mkdir /tmp/ssd

# mount the new partition
/bin/mount /dev/sda1 /tmp/ssd/

# Write some text in a file in the new partition
echo "Hello World from SATA SSD!" > /tmp/ssd/test.txt

# Read back the text
cat /tmp/ssd/test.txt
```
