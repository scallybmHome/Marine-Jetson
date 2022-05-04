# Getting Started

We are assuming that you are installing SignalK => 1.40.0

And that your Jetson is running and up to date.

Present installationhas been tested on:

- Xavier NX running Ubuntu 18.04

# Installing Signal K

The installation of Signal K can be broken down in to three distinct steps;

1. Install the tools and libraries required to run the Signal K server (the dependencies)
1. Install a Signal K Server to process the Signal K data; we will install the Node-Server
1. Install the Web Apps (consumers) that can read Signal K data

## Step 1 - Install the Dependencies

Ubuntu, which is the default Linux distribution for Jetson modules, is based on Debian, which has a powerful installation tool called "apt", which we will use to install some of the dependencies and tools required.
The default Jetson install has been stripped of any 'unnecessary' packages, so several of these will need to be installed prior to the final packages.
Before you use "apt" it is always recommended to update its index of repositories (websites that store Linux software). To do that use the following command in a terminal session...

    $ sudo apt update && sudo  apt install -y curl

Now, install the LTS version of node and npm

    $ curl -fsSL https://deb.nodesource.com/setup_lts.x | sudo -E bash -
    $ sudo apt-get install -y nodejs

We want to make sure that we're using the latest version of npm:

    $ sudo npm install -g npm@latest
    
Use the following command to check the install

    node -v && npm -v

which will report, something like, `v16.15.0, 8.8.10 which are the versions of "node" and "npm"

Finally we need to install a Bonjour (mDNS) service for Linux called Avahi, which allows Apps and other network devices to Discover the Signal K server. To do this we will use "apt" again ...

     $ sudo apt install -y libnss-mdns avahi-utils libavahi-compat-libdnssd-dev


## Step 2 - Install Signal K Server

A Signal K Server is the central hub of a Signal K system; reading and converting the boat's NMEA data (or Signal K data from a gateway), which it then stores and logs, before outputting the Signal K data to web apps (consumers) on the boat or sending it off the boat to other vessels or Cloud services.

Install the Node Server using npm

    $ sudo npm install -g signalk-server

## Step 3 - your own setup and running automatically as daemon

To generate your own vessel settings file and configure your Pi to start the server automatically , type...

    $ sudo signalk-server-setup

and follow the prompts. If you are following the defaults and are logged on as ”pi” the boat info will be stored in `/home/pi/.signalk/defaults.json` and the settings in `/home/pi/.signalk/settings.json`

You can re-run this command at any time in the future to change the settings.

These files can be edited via the admin UI or directly looking at the example settings.

**This script will also set up Node server to run automatically in the background as a daemon, [systemd](https://wiki.debian.org/systemd/), when the system boots.** You will no longer be able to launch it manually, because the automatically started instance will occupy the ports where the services are available. You should do this once you are happy with the way the server works.

Stop the daemon temporarily with;

    $ sudo systemctl stop signalk.service
    $ sudo systemctl stop signalk.socket

Start the deamon with;

    $ sudo systemctl start signalk.socket
    $ sudo systemctl start signalk.service

Disable the automatic start with;

    $ sudo systemctl disable signalk.service
    $ sudo systemctl disable signalk.socket

Check status with;

    $ sudo systemctl status signalk.service
    $ sudo systemctl status signalk.socket

**In addition the setup script will enable security by default.** At the admin UI you have to use ”Login” in the upper right corner and create a account, for example 'admin' and 'password', and then logon. Security information is stored in `/home/pi/.signalk/security.json`.

![enable_security](https://user-images.githubusercontent.com/16189982/43796658-279e7c40-9a85-11e8-98d4-a90f1e9904d1.jpeg)

## Everything after this is the same..

Check out the SignalK notes on installing on a Raspberry Pi
