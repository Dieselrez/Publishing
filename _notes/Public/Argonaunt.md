---
title: Argonaunt
feed: hide
date: 29-06-2025
permalink: /Argonaunt
---
Set up and installation of Argonaunt server

Necessary Preparations:
- Have subscription to newshosting site/one that supports this type of server... 	
- Have Subscription to NZBFinder, like newshosting but different.
- Have server with sufficent storage/NAS available. 
- Install Docker & Docker Compose onto the server per Docker's wiki--check by running hello world
- Install Tailscale per their wiki--make sure it works by checking web client

How to set it up, use this folder thing \\
:"Docker for Conner"\\
What is inside of it\\

	mnt(folder)
		local
			data
				media
					anime
					movies
					tv-shows
				usenet
					incomplete
					complete
					
	home/conner/(all contain a docker compose file)
		argonaunt
		jellyfin
		prowlarr
		radarr
		sab
		sonarr

paste that folder over and replace conner with something else and change the mnt folder chain to something else if needed. Use chmod to change all folder under data to be able to have full use to execute, read, and write. which the command is "sudo chmod 777 data/(astericks)" this should do all the folders under data to have all of those rights.

So long as everything is set up, go into argonaunt folder and run command 

"sudo docker compose up -d"

This runs the docker compose file in said folder which is going to automagically run the other docker compose files in the other folder in that directory. With the -d it runs the command in the background that way you can do other things in the server while it is running and close the cli without turning docker off. 

After running that check the ip of the machine, I use ubuntu so the command to see the ip of the server is 

"hostname -I"

which should be anything ie. 192.168.1.123    
if you wanted to see the webinterfaces of the different apps is use the ip with the port
ie.--

Jellyfin---192.168.1.123:8096---how to watch things

Prowlarr---192.168.1.123:9696---add the indexers for sonarr and radarr(NZBFinder)

Radarr---192.168.1.123:7878---an search engine for mov

Sonarr---192.168.1.123:8989---an search engine for tv

Sabnaz---192.168.1.123:8080---downloader app

What you should do instead of using the local ip is using the tailscale ips and use that to manage and use jellyfin aswell.

you might be wondering what Tailscale is and how does it give you a new ip	

Tailscale is a VPN that I see more as a Vlan/tunnel application. what it does is that it routes the ip through their network. This is useful because if you needed to access something across the internet you need to port forward the server ip+port, this can both cause there to be vulnerabilities on your network and you'd need to go through some hoops to get your isp to let that through. 

#### Now you are on to the easy fun part! 

first you should set up your download app sabnaz,
no need to set up a login if you need to make it easy, and allow access from exterior addresses to make sure you don't shoot yourself in the foot. 

##### Do this for all the apps coming up as well if the options are there. 

go to Sabnaz the ip:8080 and then in the settings go to servers tab, add server, and put in the details for the host, username, and password for the newshosting subscription. 
Then for the next step go to general and find the api key which you will need later. 

#### Go to Prowlarr
Prowlarr is a way you connect the downloader to the indexers. 
first you go to the indexer tab and add the indexer NZBFinder and then use the api key from the indexer and make sure to test the server before saving!

#### Go to Sonarr/Radarr(same set up)
go to settings, indexers, select nzbfinder(prowlarr) use the api key from prowlarr. go to download clients and add sabnzbd and add the one from sabnaz from before. then go to media management and add a new path to be /data/media/movies orrrrr tv.

#### Finally Jellyfin!

just set up like you would for anything, good user/pass, then when you get to the server put in the tailscale ip into the thing with port :8096 at the end and it should work perfect. 

to do a test go to either Radarr or Sonarr to search, download, and see if it does! it should have an error either in one of those or in Sabnaz when downloading, if it does not show in jellyfin after a few mins it might not be in the right folder/there was an error. Beyond that we covered everything and happy watching! 
