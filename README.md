# 2420_Assignment_3_Part_1


First issue that I have encouter that my previous apache server that we used in previous classes was running actually I had 5 apache servers runnig, and my port 80 was in use I could not access it
I had first to stop the apache server and the disable it in porder to make space nginx to run on port 80

I also installe dthe lsof package to my drolet in order to see all teh servers runnign, this package was quite usefull as detailed all teh servers running,

My second issue was that my server block was asking me for a certificate, while I was following the arch wiki tutorial for creating a seperate server block file, teh example has ssl addition on the port syntax, after oding some research I have removed this adn the server was running smothley

ANother issues was that my nginx file thorwing the erro and I had to increase my Nginx Setting server_names_hash_max_size and server_names_hash_bucket_size the message in journactl was indicating to increase to 2048 and 128 from 64 nad 1024 respectivly. After handling all of these erros my server was running proprely. 

# Reference
  
  #Use the section 3.2.3.1 to setup your server block
  1. https://wiki.archlinux.org/title/Nginx 3.2.3.1 
