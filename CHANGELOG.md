0.10

* Add missing port forwards, update nginx config

---

0.9

* Update README
* Formatting, allowing long lines

---

0.8

* Increment added port forward by one to not clash with minio2

---

0.7

* Update README for number of cores and memory set for virtual machines
* Update nginx.conf to do reverse proxy to different services (testing required)
* Update vagrant port forwards

---

0.6

* Cleanup networking for .2 and .3 IP addresses

---

0.5

* Fix misnamed variable

---

0.4

* vtnet1 is actually untrusted interface
* formatting and indentation fixes

---

0.3

* Fix paths for consul configuration
* Adjust ERRATA file for /etc/vbox warning
* Adjust ansible for "scp_if_ssh = True"
* vtnet1 not vtnet0 for host interface pot jails associate with

---

0.2

* Trim to 2 hosts
* Add consul, nomad, traefik
* Add beast-of-argh
* Add nomad job for nextcloud

---

0.1

* First bash at minio-sampler
