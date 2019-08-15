# Cloudflared  
A Cloudflared docker's image made with Alpine, for DNS over HTTPS.  
Can be use with Pihole.  

## How to use  
- Update **.env** with your informations. Choose the right architectrue (amd64, 386 or arm) For a Raspberry py, choose **arm**.  
- Start the container with ```docker-compose up --build -d```.  
- Connect into the container with ```docker-compose exec cloudflared sh ```.  

In the **docker-compose.yml**, the cloudflared container has a static ip. You have to use this @ip to specify a custom DNS in your configuration.  

### To test if everything is working
- Inside the container: ```dig @172.0.0.1 -p 5053 google.com```  
- Outside the container : ```dig @172.28.1.1 -p 5053 google.com```  

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