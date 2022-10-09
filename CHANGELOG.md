0.31

* Cosmetic code fixes

---

0.30

* Additional hotfixes

---

0.29

* Bug fixes, prepare db and nextcloud not copied over

---

0.28

* Configure for nextcloud

---

0.27

* Fix nginx reverse proxy setup

---

0.26

* Patch pot common.sh
* because https://github.com/bsdpot/pot/commit/f033266a26ecd144dc047bb7f35929112407dfc0

---

0.25

* Version bump on consul image

---

0.24

* Split detailed install steps to own document

---

0.23

* Fix pot.conf
* Disable coredumps
* enable sysctl values for jails to allow ping

---

0.22

* Add strategic continues

---

0.21

* Update README some more

---

0.20

* Update README

---

0.19

* Update README with increased server specs
* Update consul pot image version with optional PEERS

---

0.18

* Fixed JSON formatting for consul agent.json

---

0.17

* Working cli minio bucket setup

---

0.16

* Trying different setup with minio commands for insecure flag

---

0.15

* Minio alias set, bucket creation, remove config host add

---

0.14

* Fixing minio for cli config

---

0.13

* Updating README
* Changing IP addresses, leaving vtnet1 unnamed, removing untrusted name, bugfixing IP issues

---

0.12

* Adjust IP addresses for testing purposes
* Update ports, update README

---

0.11

* Add more nginx workers
* Fix jail IP address subnet typo

---

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
