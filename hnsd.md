# Compiling HNSD light node on Ubuntu 20.x

## Firstly you need to install NodeJS 14 and some build tools on your ubuntu server:

```sh
sudo apt update
sudo apt upgrade
curl -sL https://deb.nodesource.com/setup_14.x | sudo bash -
sudo apt -y install git nodejs gcc g++ make npm automake autotools-dev libtool bind9-utils dnsutils net-tools unbound
sudo npm install -g pm2 node-gyp
```

## Then clone hnsd repo to your home dir and build it from source:

```sh
git clone git://github.com/handshake-org/hnsd.git ~/hnsd
cd ~/hnsd
./autogen.sh && ./configure && make
```

## Now setup pm2 to run it in background:

```sh
sudo pm2 autostart
```

If it suggests some commands to run, then run it

## Now start your hnsd resolver in background with pm2:

```sh
pm2 start ./hnsd --name="hnsd" -- -p 4 -r 127.0.0.1:5341 -n 127.0.0.1:5369 -i 0.0.0.0
pm2 save
```

## You're done with installing!

Check your hnsd node by digging it:

```sh
dig @127.0.0.1 -p 5341 namebase +dnssec
```

If it shows something like this:

```

; <<>> DiG 9.16.1-Ubuntu <<>> @127.0.0.1 -p 5341 namebase +dnssec
; (1 server found)
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 3527
;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags: do; udp: 4096
;; QUESTION SECTION:
;namebase.      IN  A

;; ANSWER SECTION:
namebase.    139  IN  A  52.43.158.89

;; Query time: 940 msec
;; SERVER: 127.0.0.1#5341(127.0.0.1)
;; WHEN: Sun Jan 02 19:43:36 EST 2022
;; MSG SIZE  rcvd: 53
```

Then my congratulations! You installed hnsd light node! Now move to [Installing HNS-bridge](/hns-bridge.md) if ou want to install it!
If not, then you did something wrong, try asking people on [this discord server](https://discord.gg/zRFXXuNJRZ)