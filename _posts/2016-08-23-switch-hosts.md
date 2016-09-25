---
layout: post
title: "switch hosts"
date: 2016-08-23
---
When I at home, and want to pull the code from company's gitlab, I have to change the ip address every time, that wastes time and I do not want repeat the action again and again. So I write this python script to do it quickly.

[python file](../../../files/switch_hosts.py)

```
import sys,os

#parameter c represents "company", h represents "home"
env = sys.argv[1]

inner_ip = "*.*.*.* gitlab.***.com"
external_ip = "*.*.*.* gitlab.***.com"

domain = "gitlab.***.com/"
domain_with_port = "gitlab.***.com:9080/"

#edit hosts file
hostfile = open("/etc/hosts","r")
lines = hostfile.readlines()
hostfile.close()

hostfile = open("/etc/hosts","w+")
for line in lines:
    if "gitlab.***.com" not in line:
        hostfile.write(line)

if env == "c":
    hostfile.write(inner_ip)

if env == "h":
    hostfile.write(external_ip)

hostfile.seek(0,os.SEEK_SET)
for line in hostfile.readlines():
    print line.replace("\n","")

hostfile.close()

#edit git config file
files = ["file1","file2"]

for file_path in files:
    config_file = open(file_path,"r+")
    content = config_file.read()

    if env == "h":
        content = content.replace(domain,domain_with_port)

    if env == "c":
        content = content.replace(domain_with_port,domain)

    config_file.seek(0,os.SEEK_SET)
    config_file.write(content)
    config_file.truncate(len(content))
    config_file.flush()
    config_file.close()

```
