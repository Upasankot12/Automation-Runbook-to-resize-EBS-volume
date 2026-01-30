# EBS Volume Resize Runbook

## Purpose
This repository documents the steps to resize an Amazon EBS volume and extend the filesystem on a Linux EC2 instance.

## Commands
```bash
df -h
lsblk
sudo growpart /dev/xvda 1
sudo resize2fs /dev/xvda1
df -h

Note: For XFS filesystems, use:
sudo xfs_growfs /
