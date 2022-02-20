# Read Write NTFS macOS

## Software

### macFUSE 
https://osxfuse.github.io

### ntfs-3g

```bash
brew tap gromgit/homebrew-fuse
brew install ntfs-3g-mac
````

### Disk Mount Script

Open terminal

```nano ntfs```

```bash
#!/bin/bash

#Check sudo
if [[ $(/usr/bin/id -u) -ne 0 ]]; then
    echo "This script should be run as ROOT. Try sudo"
    exit
fi
clear
echo ""
ls -1 /Volumes | cat
echo ""
read -p "Disk Label: " disk_label

LABEL=$(diskutil list | grep "$disk_label" | awk '{print $5}')
DNODE=$(diskutil list | grep "$LABEL" | awk '{print $8}')

echo ""
if [[ "$LABEL" != "" ]]; then
    echo "Disk found!"
    echo "Label: " $LABEL
    echo "Node: "$DNODE
    echo ""
    diskutil unmount /dev/$DNODE
    sleep 2
    sudo /usr/local/sbin/mount_ntfs /dev/$DNODE /Volumes/$LABEL
    echo "Volume $LABEL on $DNODE mounted"
else
    echo "Disk not found!"
fi
echo ""
```

Save file Pres CTRL + X  and than Y, Enter

Make Excutable
```sudo chmod +x ntfs```

Run
```sudo ./ntfs```
