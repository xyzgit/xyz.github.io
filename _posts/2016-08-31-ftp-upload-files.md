---
layout: post
title: "upload files to ftp server"
date: 2016-08-31
---
If you do the same thing again and again, then you will feel that is too boring.

Here is a script I wrote to upload apk and ipa files to ftp server.

```
# -*- coding: utf-8 -*-
import paramiko
import sys,os

host = "*.*.*.*"
transport = paramiko.transport.Transport((host))
username = "***"
password = "***"    
transport.connect(username = username, password = password)
sftp = paramiko.SFTPClient.from_transport(transport)

local_path = sys.argv[1]

remote_root_path = "your ftp server path"

remote_folder = "{0}/{1}".format(remote_root_path,sys.argv[2])

# if $remote_folder is not exist, then make a folder with name '$remote_folder'
try:
   sftp.lstat(remote_folder)
except IOError:
   sftp.mkdir(remote_folder)

# define a callback of uploading progress
def progress(cur, total):
    percent = float(cur) / total
    sys.stdout.write('\r[{:.2%}]'.format(percent))
    sys.stdout.flush()

#upload files in local_path
files = os.listdir(local_path)
for file in files:
    print "upload ",file
    local_file = os.path.join(local_path,file)
    remote_file = os.path.join(remote_folder,file)
    sftp.put(local_file,remote_file,callback=progress)

sftp.close()
transport.close()

print 'All Files Upload Done !'
```
