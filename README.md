# Introduction
`minio-sampler` (aka `minsampler`) borrows heavily from [potman](https://github.com/bsdpot/potman), [minio-incinerator](https://github.com/bretton/minio-incinerator/), `clusterfurnace` and `cephsmelter` to build a virtualbox minio cluster .

Do not run in production! 

This is a testing environment to show `minio` running on FreeBSD, with `consul`, `nomad`, and the Beast-of-Argh one-pot monitoring solution providing `prometheus`, `grafana`, `loki`, `alertmanager`, plus a `nextcloud` nomad image, configured to use `minio` S3 as file storage, and a `mariadb` pot image for the `nextcloud` database.

# Outline
This will bring up 2 minio servers:
* minio1 (8CPU, 8GB, 4 attached disks)
* minio2 (4CPU, 4GB, 4 attached disks)

# Requirements
The host computer running `minio-sampler` needs at least 16 CPU threads, 16GB memory, plus 50GB disk space, preferably high speed SSD. The setup takes an hour or so with packbox step included.

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
      vagrant ssh minio1
      OR
      open http://ACCESSIP
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

Please see [Detailed Install FreeBSD & Linux](DETAILED-INSTALL.md)

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

A basic check is done to see if the input figures can be catered to, on running `minsampler init <name>`.

## Dashboards

The default dashboard with links to all the included tools is `http://ACCESSIP`. There is a default index.html with links.

Where `https` is applicable, please accept the self-signed certificate. You might need to do this twice, even refresh, to get the minio dashboard.

### Minio Dashboard

The Minio web interface is available via nginx reverse proxy at `https://ACCESSIP:19000` for the IP address set in ACCESSIP variable of `config.ini`.

Accept the self-signed certificate TWICE. First is for nginx, second minio. Remember, this is not a production-ready environment. 

Login with user `sampler` and `samplerpasswordislong` to access the dashboard.

You can now create a bucket and explore settings.

### Grafana Dashboard

The Grafana web interface is available via nginx reverse proxy at `http://ACCESSIP:3000` for the IP address set in ACCESSIP variable of `config.ini`.



