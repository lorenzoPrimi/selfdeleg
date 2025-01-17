# Self Delegator Bot

A simple self delegator bot for any Blockchain on the Cosmos ecosystem <br>
Forked from the [Desmos bot](https://github.com/g-luca/selfdeleg)

## Requirements

* A cosmos blockchain CLI
* Python >= 3

## 1\. Install

``` bash
git clone https://github.com/lorenzoPrimi/selfdeleg.git && cd selfdeleg && mkdir logs
```

## 2\. Configuration

``` bash
mv template.ini config.ini && nano config.ini
```

And edit:

1. `CLIENT` with your client command name, and `UNIT` with the smallest unit of the token <br>
2. `KEY_NAME` with your validator key name, and `KEY_BACKEND` if you use a different [keyring backend](https://docs.cosmos.network/v0.42/run-node/keyring.html). <br>
If you use the **test** keyring, in the future steps you can replace the password inputs with spaces/random characters
3. `VALIDATOR_ADDRESS`, `USER_ADDRESS`, `DELEGATE_ADDRESS`, with your addresses.
If you want to self delegate, `USER_ADDRESS` and `DELEGATE_ADDRESS` should match.
4. If you are installing the bot in a machine that is not running a node/validator configure `DEFAULT_NODE_ADDRESS` and `DEFAULT_NODE_PORT` with remote nodes addresses
5. `MINIMUM_BALANCE` amount of JUNO that the bot will always keep (minimum 1 JUNO)
6. The other configuration values are optional

## 3\. Run

To run the bot, use:

``` bash
python3 bot.py
```

This will ask for the keyring password at first, if you don't want to you can use

``` bash
python3 bot.py password
```

## 3\. Run as a Service \(Ubuntu/Linux\)
<br>

``` bash
sudo nano ./start.sh
```

and change the password:

````
#!/bin/bash

source ~/.profile
/usr/bin/python3 $HOME/selfdeleg/bot.py password
````
<br>
Then,

``` bash
sudo nano /etc/systemd/system/selfdeleg.service
```

and paste changing accordingly the User path and username:

````
[Unit]
Description=Juno Self Delegator Bot
After=network-online.target


[Service]
WorkingDirectory=/home/ubuntu/selfdeleg/
User=ubuntu
Type=simple
Restart=always

ExecStart=/bin/bash /home/ubuntu/selfdeleg/start.sh


[Install]
WantedBy=multi-user.target
````
<br>
Now you can reload and start the service

``` bash
sudo systemctl daemon-reload
```

``` bash
sudo systemctl start selfdeleg
```
<br>
To follow the daily output:
<br>

``` bash
tail -f ./logs/debug.log
```
