# RBL Checker

This PHP script is an internal project to monitor a few of the popular RBL's (aka DNSBL's or DNS black lists) for our IP blocks. We have a /22 and a /21 that we process mail out of for our customers. Sometimes, this mail manages to land one of our IP's on a blacklist. I couldn't really justify paying for this service considering I have some conflicted feelings towards email spam blacklists, but that is outside the scope of this project. 

This script is quick and dirty and is not very elegant, but it does get the job done. The "$ip" array can handle a standard IPv4 address as well as a block in CIDR notation. If you don't know what either of these things are, you may want to reada few Wikipedia articles before attemtping to put this into use. 

The "$rbl" array is pre-filled with a handful of blacklists. (65 to be exact - way too many for the average mail sender) I suggest trimming this down to a few more targeted lists, such as the lists you find your IP's listed on most often. 

As it stands now, running ~3000 IP's through ~65 blacklists takes upwards of 6 hours. You could probably fork this into multiple processes. Again, that's outside the scope of this project - my suggestion is to trim the RBL list down and pay attention to the size of the netmask you use. (IE, a /16 has 65534 possible hosts. You will almost assuredly completely kill PHP with a /16.)


# To Use:

Adjust the $ip array to the IP's or IP range (in CIDR notation!) you'd like to check. Check and adjust the $rbl array if you'd like, or cull it down after your initial run. Navigate to the directory where you store the php file. Check perms. 

1. sudo chmod +x bl.php

And then, run the file:

2. ./bl.php 

What you do from here is up to you. I have it scheduled to run with a cron and capture the results and email them to me. 

# Goals


1. Eventually I'd like to replace "getHostByName" with dig and also introduce some timers and exit conditions. If this gets hung up on a DNS call, it can hang for quite awhile.

2. I'd like to limit the blocks that can used with this script. Obviously it's limited by your available memory, but someone running ten /20's through this script is going to have a problem. 

3. I'd like to tune the responses from the blacklists. I know some of these don't respond with a standard reverse A record of the offending IP - some return different forms of a 127 address. Identifying and checking this response would help with false positives. 
