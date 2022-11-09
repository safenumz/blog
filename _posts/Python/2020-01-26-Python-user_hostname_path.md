---
layout: post
title: '[Python] username, home directory, hostname, ip address'
category: Python
tags: [python]
comments: true
---


# Get username

~~~python
import getpass

username = getpass.getuser()
print(username)
~~~

# Get Home directory

~~~python
import os

homedir = os.path.expanduser("~")
print(homedir)

homedir2 = os.environ["HOME"]
print(homedir2)
~~~

# Get hostname and IP

~~~python
# Importing socket library 
import socket 
  
# Function to display hostname and IP address 
def get_hostname_ip(): 
    try: 
        host_name = socket.gethostname() 
        host_ip = socket.gethostbyname(host_name) 
        print("Hostname :  ",host_name) 
        print("IP : ",host_ip) 
    except: 
        print("Unable to get Hostname and IP") 

get_hostname_ip()
~~~