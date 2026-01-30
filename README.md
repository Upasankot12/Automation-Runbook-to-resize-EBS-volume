#!/bin/bash
set -e

# Update these if your device name is different
DISK="/dev/xvda"
PARTITION="/dev/xvda1"
MOUNT_POINT="/"

echo "Checking current disk usage..."
df -h

echo "Growing partition..."
sudo growpart $DISK 1

echo "Detecting filesystem type..."
FS_TYPE=$(df -T $MOUNT_POINT | tail -1 | awk '{print $2}')

if [ "$FS_TYPE" = "xfs" ]; then
    echo "Extending XFS filesystem..."
    sudo xfs_growfs $MOUNT_POINT
elif [ "$FS_TYPE" = "ext4" ]; then
    echo "Extending EXT4 filesystem..."
    sudo resize2fs $PARTITION
else
    echo "Unsupported filesystem: $FS_TYPE"
    exit 1
fi

echo "Resize completed successfully."
df -h
