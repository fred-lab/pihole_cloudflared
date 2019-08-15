# Pihole with Cloudflared  
Pihole with docker for DNS over HTTPS via Cloudflared.  

## How to use  
- Update **.env** with your informations. Choose the right architectrue (amd64, 386 or arm) For a Raspberry py, choose **arm**.  
- Start the container with ```docker-compose up --build -d```.  
- Connect into the container with ```docker-compose exec cloudflared sh ``` or ```docker-compose exec pihole sh```.  
- Connect to the Pihole interface : **172.23.0.2/admin** (172.23.0.2 is the @ip of the pihole container).  

### Update pihole admin password  
If you have trouble with the pihole password, connect to the container ```docker-compose exec cloudflared sh ``` and run ```pihole -a -p``` then enter a new password (leave it blank if you don't want to use a password).  

### To test if DNS over HTTPS is working
- Inside the cloudflared container: ```dig @172.0.0.1 -p 5053 google.com```  
- Outside the cloudflared container : ```dig @172.28.1.1 -p 5053 google.com```  

You should have something like this in result :
```
; <<>> DiG 9.14.4 <<>> @172.28.1.1 -p 5053 google.com
; (1 server found)
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: SERVFAIL, id: 41025
;; flags: qr rd; QUERY: 1, ANSWER: 0, AUTHORITY: 0, ADDITIONAL: 1
;; WARNING: recursion requested but not available

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 4096
; COOKIE: e6c016ce407a1fdd (echoed)
;; QUESTION SECTION:
;google.com.			IN	A

;; Query time: 64 msec
;; SERVER: 172.28.1.1#5053(172.28.1.1)
;; WHEN: jeu. août 15 18:03:27 CEST 2019
;; MSG SIZE  rcvd: 51
```

## Relative links  
https://docs.pi-hole.net/guides/dns-over-https/

https://github.com/pi-hole/docker-pi-hole