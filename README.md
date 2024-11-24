# 2420 Assignment 3 - Part 1




Bash scripting: Automating tasks and generating dynamic content.
System administration: Managing users, permissions, and services.
Systemd services and timers: Scheduling tasks using systemd.
Nginx configuration: Hosting web content and managing server blocks.
Firewall setup: Securing your server using UFW.
This project builds on concepts discussed in class and requires applying your understanding of system administration, systemd, scripting, and web server configuration.



## Step 1

Run the user script
The user sctipr will create all the nessecsary base directories and files for the new `webgen` user. 

**Please note that you have to have root privilages in order to tun the user script**

Run the following command:

    `sudo ./user `

## Step 2

### Generate Index Service and Timer

In Step 2 you have to create a generate-index.service and generate-index.timer 

1. `generate-index.service` will run your generate-index file.
 
`generate-index.service` needs to be created at the location below

    /etc/systemd/system/


You need to configure your `generate-index.service`

Copy the code below into your `generate-index.service` file.
      

      [Unit]
      Description=generate-index html
      Wants=network-online.target
      After=network-online.target

      [Service]
      User=webgen
      Group=webgen
      ExecStart=/var/lib/webgen/bin/generate-index
      RemainAfterExit=yes
      Restart=on-failure

      [Install]
      WantedBy=multi-user.target 


Run the follwoing commands to start and enable the services:

      sudo systemctl start generate-index.service
      sudo systemctl enable generate-index.service

      sudo systemctl start generate-index.timer
      sudo systemctl enable generate-index.timer

Check the status of your services by running the commands below:

    sudo systemctl status generate-index.timer
    sudo systemctl status generate-index.service

      
Your output when running the status for `generate-index.service` should look similar to the image below

![Service Status](2420_Assignment_3_Part_1/assets/services image.png "Status running")


This file needs to be located in the location below
`generate-index.timer` will start your service every day at 05:00
    





#### 2. `generate-index.timer`
The timer file schedules the service to execute every day at 05:00.





Reload systemd and enable the timer: Run the following commands to reload systemd, enable, and start the timer:

bash

sudo systemctl daemon-reload




chmod +x /path/to/generate_index

# Step 3


## Configure Nginx

### Steps:
1. **Modify the Main `nginx.conf` File**:
   - `nginx.conf` needs to be updated file in order the Nginx server runs as the `webgen` user.
   - Find the `user` directive in `/etc/nginx/nginx.conf` and and at the top of the file change it to:
     ```nginx
     user webgen;
     ```

2. **Create a Separate Server Block File**:
   - Avoid modifying the main `nginx.conf` file directly for server-specific configurations.
   - Create a new server block file in `/etc/nginx/sites-available/` (or the appropriate directory based on your setup, e.g., `/etc/nginx/conf.d/` on Arch Linux). Example:
     ```nginx
     server {
         listen 80;
         server_name localhost;

         location / {
             root /path/to/your/html/files;
             index index.html;
         }
     }
     ```
   - Symlink the server block file to `/etc/nginx/sites-enabled/` if needed:
     ```bash
     sudo ln -s /etc/nginx/sites-available/your-server-block.conf /etc/nginx/sites-enabled/
     ```

3. **Why Use Separate Server Block Files?**
   - Easier management: Each domain or configuration is isolated.
   - Prevents accidental overwrites of the main configuration file.
   - Simplifies debugging and future scalability.

4. **Verify Nginx Configuration and Status**:
   - Check the Nginx configuration syntax:
     ```bash
     sudo nginx -t
     ```
   - Reload or restart the Nginx service:
     ```bash
     sudo systemctl reload nginx
     sudo systemctl restart nginx
     ```
   - Check the service status:
     ```bash
     sudo systemctl status nginx
     ```

---

## Task 4: Firewall Configuration with `ufw`

### Steps:
1. **Install and Configure UFW**:
   - Install `ufw` (Uncomplicated Firewall):
     ```bash
     sudo pacman -S ufw
     ```
   - Allow SSH and HTTP traffic:
     ```bash
     sudo ufw allow



useradd -m /var/lib/webgen/bin/HTML login_shell webgen# 2420_Assignment_3_Part_1


First issue that I have encouter that my previous apache server that we used in previous classes was running actually I had 5 apache servers runnig, and my port 80 was in use I could not access it
I had first to stop the apache server and the disable it in porder to make space nginx to run on port 80

I also installe dthe lsof package to my drolet in order to see all teh servers runnign, this package was quite usefull as detailed all teh servers running, and to check which ports are open as my port 80 was conflicting with nginx and that was teh reason why the nginx did not run

My second issue was that my server block was asking me for a certificate, while I was following the arch wiki tutorial for creating a seperate server block file, teh example has ssl addition on the port syntax, after oding some research I have removed this adn the server was running smothley

ANother issues was that my nginx file thorwing the erro and I had to increase my Nginx Setting server_names_hash_max_size and server_names_hash_bucket_size the message in journactl was indicating to increase to 2048 and 128 from 64 nad 1024 respectivly. After handling all of these erros my server was running proprely. 

MY issue was that my creatin generate-index.service was failing, I had to debug this by adding a ReaminAfterExit=yes directive to let the system to keep the service active nad restart on  failure is that the serice will restart if it fails


#Ip tables filter issue
#refernecne https://bbs.archlinux.org/viewtopic.php?pid=1333991#p1333991
# Reference
  
  #Use the section 3.2.3.1 to setup your server block
  1. https://wiki.archlinux.org/title/Nginx 3.2.3.1 

  #lsof
  https://www.liquidweb.com/blog/how-to-locate-open-ports-in-linux/#:~:text=The%20six%20primary%20methods%20for,re%20correctly%20configured%20and%20secure.
