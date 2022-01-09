# Installing hns-bridge on ubuntu 20.x

If you installed hnsd and want to setup own hns resolver, then this guide is for you, if not the go to [installing hnsd](/hnsd.md)

## Build tools

Since you installed hnsd, needed tools are already installed, my congrats!

## Downloading code and installing dependencies

```sh
git clone https://github.com/angrymouse/hns-bridge ~/hns-bridge
cd ~/hns-bridge
npm install
```

## Buying domain

To setup hns-bridge, you need to buy traditional domain for your bridge somewhere, where doesn't matter.
This is needed to make your bridge accessible by .\<yourdomain\> 
Example: \<handshake-domain\>.hns.is, angrymouse.hns.is, costanzo.rsvr.xyz

When domain is bought, point * and @ by A records to external IP address of your server on which you'll run hns-bridge.

## Configuring hns-bridge

Hns-bridge uses config.json to configure it

Setup it like this:

```json5
{
    "nameservers": [
        "127.0.0.1:5341"
        "103.196.38.38"
    ],
    "port": 80, // Note: this value must be 80 to work
    "domainMap": {
    "<your-domain-name>":"" // E.g "hns.is":"" or "rsvr.xyz":""
    },
    "rootRedirect": "https://angrymouse.hns.is/" // Where your domain's root will redirect when calling bare domain (rsvr.xyz but not portfolio.costanso.rsvr.xyz) 
}

```

Don't forget to remove // with liknes after it - that's comments, if you don't remove it then config will be invalid and bridge won't work!


## Opening ports

Usually there's UFW firewall on ubuntu. If you installed another just open 80th port. Here's command for ufw:

```sh
sudo ufw allow http
```

## Running hns-bridge

To run hns-bridge in daemon mode use pm2:

```sh
pm2 start . --name="hns-bridge"
pm2 save
```

## Congrats!

You made it! Don't forget to open 80th port on your router if you use home servers, and enjoy your bridge!