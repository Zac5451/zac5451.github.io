https://www.youtube.com/watch?v=3K1hUwxxYek
================
Commands:
================
sudo apt update
sudo apt install lightdm
sudo reboot
sudo apt install x11vnc

sudo nano /lib/systemd/system/x11vnc.service

!Copy and paste these commands - change the password
[Unit]
Description=x11vnc service
After=display-manager.service network.target syslog.target

[Service]
Type=simple
ExecStart=/usr/bin/x11vnc -forever -display :0 -auth guess -passwd password
ExecStop=/usr/bin/killall x11vnc
Restart=on-failure

[Install]
WantedBy=multi-user.target

!Save file and run these commands:

systemctl daemon-reload
systemctl enable x11vnc.service
systemctl start x11vnc.service
systemctl status x11vnc.service

● x11vnc.service - x11vnc service
     Loaded: loaded (/usr/lib/systemd/system/x11vnc.service; enabled; preset: enabled)
     Active: active (running) since Fri 2024-07-05 18:11:59 CST; 8s ago
   Main PID: 116165 (x11vnc)
      Tasks: 1 (limit: 38021)
     Memory: 11.0M (peak: 19.0M)
        CPU: 351ms
     CGroup: /system.slice/x11vnc.service
             └─116165 /usr/bin/x11vnc -forever -display :0 -auth guess

Jul 05 18:12:00 zac-HP x11vnc[116165]: 05/07/2024 18:12:00
Jul 05 18:12:00 zac-HP x11vnc[116165]: The VNC desktop is:      zac-HP:0
Jul 05 18:12:00 zac-HP x11vnc[116165]: PORT=5900
Jul 05 18:12:00 zac-HP x11vnc[116165]: *>
Jul 05 18:12:00 zac-HP x11vnc[116165]: Have you tried the x11vnc '-ncache' VNC client-side pixel caching feature y>
Jul 05 18:12:00 zac-HP x11vnc[116165]: The scheme stores pixel data offscreen on the VNC viewer side for faster
Jul 05 18:12:00 zac-HP x11vnc[116165]: retrieval.  It should work with any VNC viewer.  Try it by running:
Jul 05 18:12:00 zac-HP x11vnc[116165]:     x11vnc -ncache 10 ...
Jul 05 18:12:00 zac-HP x11vnc[116165]: One can also add -ncache_cr for smooth 'copyrect' window motion.
Jul 05 18:12:00 zac-HP x11vnc[116165]: More info: http://www.karlrunge.com/x11vnc/faq.html#faq-client-caching

zac@zac-HP:/$ ufw
ERROR: not enough args

Usage: ufw COMMAND

Commands:
 enable                          enables the firewall
 disable                         disables the firewall
 default ARG                     set default policy
 logging LEVEL                   set logging to LEVEL
 allow ARGS                      add allow rule
 deny ARGS                       add deny rule
 reject ARGS                     add reject rule
 limit ARGS                      add limit rule
 delete RULE|NUM                 delete RULE
 insert NUM RULE                 insert RULE at NUM
 prepend RULE                    prepend RULE
 route RULE                      add route RULE
 route delete RULE|NUM           delete route RULE
 route insert NUM RULE           insert route RULE at NUM
 reload                          reload firewall
 reset                           reset firewall
 status                          show firewall status
 status numbered                 show firewall status as numbered list of RULES
 status verbose                  show verbose firewall status
 show ARG                        show firewall report
 version                         display version information

Application profile commands:
 app list                        list application profiles
 app info PROFILE                show information on PROFILE
 app update PROFILE              update PROFILE
 app default ARG                 set default application policy

zac@zac-HP:/$ ufw status
ERROR: You need to be root to run this script
zac@zac-HP:/$ sudo ufw status
Status: active

To                         Action      From
--                         ------      ----
20/tcp                     ALLOW       Anywhere                  
21/tcp                     ALLOW       Anywhere                  
22/tcp                     ALLOW       Anywhere                  
80/tcp                     ALLOW       Anywhere                  
443/tcp                    ALLOW       Anywhere                  
888/tcp                    ALLOW       Anywhere                  
41283/tcp                  ALLOW       Anywhere                  
39000:40000/tcp            ALLOW       Anywhere                  
5244                       ALLOW       Anywhere                  
6800                       ALLOW       Anywhere                  
Samba                      ALLOW       Anywhere                  
19496/tcp                  ALLOW       Anywhere                  
20/tcp (v6)                ALLOW       Anywhere (v6)             
21/tcp (v6)                ALLOW       Anywhere (v6)             
22/tcp (v6)                ALLOW       Anywhere (v6)             
80/tcp (v6)                ALLOW       Anywhere (v6)             
443/tcp (v6)               ALLOW       Anywhere (v6)             
888/tcp (v6)               ALLOW       Anywhere (v6)             
41283/tcp (v6)             ALLOW       Anywhere (v6)             
39000:40000/tcp (v6)       ALLOW       Anywhere (v6)             
5244 (v6)                  ALLOW       Anywhere (v6)             
6800 (v6)                  ALLOW       Anywhere (v6)             
Samba (v6)                 ALLOW       Anywhere (v6)             
19496/tcp (v6)             ALLOW       Anywhere (v6)             

zac@zac-HP:/$ sudo ufw allow x11vnc
ERROR: Could not find a profile matching 'x11vnc'
zac@zac-HP:/$ sudo ufw allow 5900
Rule added
Rule added (v6)
zac@zac-HP:/$ sudo ufw allow 5901
Rule added
Rule added (v6)

如果还是登不上，就检查一下ufw防火墙设置，是否没有开放端口
