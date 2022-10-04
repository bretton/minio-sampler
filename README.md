# Introduction
`minio-sampler` (aka `minsampler`) borrows heavily from [potman](https://github.com/bsdpot/potman), [minio-incinerator](https://github.com/bretton/minio-incinerator/), `clusterfurnace` and `cephsmelter` to build a virtualbox minio cluster .

Do not run in production! This is a testing environment to show minio running on FreeBSD, with Consul, and the Beast-of-Argh monitoring solution alongside.

# Outline
This will bring up 2 minio servers, configured for erasure coding, with a web interface:
* minio1 (4CPU, 4GB, 4 attached disks, nginx reverse proxy host, consul jail, beast-of-argh jail)
* minio2 (4CPU, 4GB, 4 attached disks)

# Requirements
The host computer running `minio-sampler` needs at least 8 CPU threads, 8GB memory, plus 50GB disk space, preferably high speed SSD.

# Overview

## Quickstart
To create your own sampler, init the VMs:

    git clone https://github.com/bretton/minio-sampler.git
    cd minio-sampler

      (edit) config.ini and set ACCESSIP to a free IP on LAN, and set DISKSIZE

    export PATH=$(pwd)/bin:$PATH
    (optional: sudo chmod 775 /tmp)
    minsampler init mysample
    cd mysample
    minsampler packbox
    minsampler startvms
    ...
    minsampler status
    ...

## Stopping

    minsampler stopvms

## Destroying

    minsampler destroyvms

## Dependencies

`minio-sampler` requires
- ansible
- bash
- git
- packer
- vagrant
- virtualbox

# Installation and Operation

## FreeBSD

(todo: add steps from https://docs.freebsd.org/en/books/handbook/virtualization/#virtualization-host-virtualbox )

### Hosts which underwent an upgrade from 13.0 to 13.1
Please see the [ERRATA](ERRATA.md) file for info on fixing a broken upgrade.

### Install for FreeBSD

Install packages, start services and configure permissions and networks
```
pkg install bash git packer py39-ansible py39-packaging vagrant virtualbox-ose
service vboxnet enable
    
(sudo) pw groupmod vboxusers -m <username>

mkdir -p /usr/local/etc/vbox
vi /usr/local/etc/vbox/networks.conf
```

(add, with asterisk; this is extremely broad)
```
* 0.0.0.0/0
```

Symlink to expected file
```
mkdir -p /etc/vbox
ln -s /usr/local/etc/vbox/networks.conf /etc/vbox/networks.conf
```

Add to /etc/rc.conf because the host needs to be a router for private networks in Virtualbox to get internet access
```
gateway_enable="YES"
```

Restart networking (may pause ssh session for a bit)
```
sudo service netif restart && sudo service routing restart
```

Start virtualbox networking
```
service vboxnet start
```

edit .profile, add the following, adjusting for username
```
PATH=/home/<username>/minio-sampler/bin:$PATH; export PATH
```

Download and configure minio-sampler:
```
git clone https://github.com/bretton/minio-sampler.git
cd minio-sampler

  (edit) config.ini and set ACCESSIP to a free IP on LAN, and set DISKSIZE

export PATH=$(pwd)/bin:$PATH
(optional: sudo chmod 775 /tmp)
minsampler init -v mysample
cd mysample
minsampler packbox
minsampler startvms

  vagrant ssh minio1   
  vagrant ssh minio2
```

## Ubuntu 20.04 with Virtualbox
Install necessary packages
```
sudo apt-get install curl wget -y
sudo apt-get install ruby-full -y   # Ubuntu 20.04 is Ruby 2.7

sudo add-apt-repository ppa:git-core/ppa -y
sudo apt-get update
sudo apt-get install git -y

sudo apt-get install ansible virtualbox -y

curl -fsSL https://apt.releases.hashicorp.com/gpg | sudo apt-key add -
sudo apt-add-repository "deb [arch=amd64] https://apt.releases.hashicorp.com $(lsb_release -cs) main"
sudo apt-get update && sudo apt-get install packer vagrant
```

Download and configure minio-sampler:
```
git clone https://github.com/bretton/minio-sampler.git
cd minio-sampler

  (edit) config.ini and set ACCESSIP to a free IP on LAN, and set DISKSIZE

export PATH=$(pwd)/bin:$PATH
minsampler init -v mysample
cd mysample
minsampler packbox
minsampler startvms

  vagrant ssh minio1   
  vagrant ssh minio2
```

# Usage

    Usage: minsampler command [options]

    Commands:
        destroyvms  -- Destroy VMs
        help        -- Show usage
        init        -- Initialize new minio-sampler
        packbox     -- Create vm box image
        startvms    -- Start (and provision) VMs
        status      -- Show status
        stopvms     -- Stop VMs

## Minio Dashboard

The Minio web interface is available via nginx reverse proxy at https://ACCESSIP:9000 for the IP address set in ACCESSIP variable of `config.ini`.

Accept the self-signed certificate TWICE. First is for nginx, second minio. Remember, this is not a production-ready environment. 

Login with user `sampler` and `samplerpasswordislong` to access the dashboard.

You can now create a bucket and explore settings.

## Grafana Dashboard

The Grafana web interface is available via nginx reverse proxy at https://ACCESSIP:3000 for the IP address set in ACCESSIP variable of `config.ini`.

## config.ini

### Access IP

A virtual interface is created with a free IP address from the LAN. You must provide this free IP address in `config.ini` in the `ACCESSIP` section.

### Disk Sizes

Virtual disks are saved in the `minio-sampler` directory for the duration of running the program. 

Set the size of the virtual disks in `config.ini` in the `DISKSIZE` section. 

Input disk size in thousands of MB, where 10000 (MB) is the same as 10GB. 

To set a 2GB disk, use 2000. This is the default.

Do not add the metric M, or MB or GB! It won't work.

Eight (8) virtual drives will be created of this size, so ensure you have sufficient disk space!

A basic check is done to see if the input figures can be catered to, on init.

