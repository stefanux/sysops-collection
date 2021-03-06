baserole feature-list
=====================

- grub configuration
- **packages**
  - default packages
  - upgrade
  - unattended upgrades ( -> jnv.unattended-upgrades )
- **DNS**
  - resolv.conf
  - FQDN (+reverse) 2DO (check if hostname --fqdn is not just hostname)
- **SSHD**
  - config of sshd
  - **fail2ban** (-> oefenweb.fail2ban ; optional helpers https://github.com/stefanux/fail2ban-remove-ban)
- **Usermanagement** (incl. authorized keys und sudo) 2DO generic with dictionary
- **configs** (of systemapps)
  - bashrc
  - htop
  - ...
- **NTP**
  - ntpd -> geerlingguy.ntp
  - chrony (when maintainers are found)

Recommended
===========

Virtualization
  - proxmox
  - libvirt/KVM ( tcharl.ansible_role_libvirt_host )
  - LXC on proxmox (maintainer -> sysops.tv?  https://github.com/bashclub/zamba-lxc-toolbox)
  - possibly other plattforms (like ovirt, openNebula or cloudstack - when maintainers are found)

VM creation
  - libvirt/KVM/cloud-init ( tcharl.ansible_role_libvirt_host oder community.libvirt?, cloud-init-example on https://github.com/stefanux/cloud-init-example)
  - proxmox/cloud-init (in progress)

mailrelay: postfix (see E-Mail) for system-mails (like cron etc.) or apps: https://github.com/stefanux/ansible-postfix-mailrelay


Additional roles
==============

**Backup**
  - bacula https://github.com/stefanux/ansible-role-bacula
  - bareos (2DO Coding)
  - Borg+Borgmatic (2DO Port Code)
  - restic (2DO Coding, maintainer needed)
  - mysql/mariadb (-> geerlingguy.mysql )
    - fulldump (SQL commands):
      - mysqlbackup https://github.com/stefanux/ansible-mysqlbackup
    - other methods:
      - FIXME
  - pfsense config (2DO https://github.com/pfsensible/core ?)
  - opensense config (2DO https://github.com/naturalis/ansible-opnsense ?)
  - etckeeper (2DO)

**Git**
  - git (client) -> geerlingguy.git
  - gitea https://github.com/stefanux/ansible-role-gitea ( -> maintainer needed)
  - gitlab -> geerlingguy.gitlab


**Filesystems**
  - ZFS
    - vanilla install (2DO)
    - Pool management (create, by-id-devices via vars)
    - special-usecases:
      - Proxmox https://github.com/bashclub/proxmox-zfs-postinstall
      - Samba shadow-copies "Zamba" https://github.com/bashclub/zamba-lxc-toolbox/
    - snapshot house-keeping: zfs-keep-and-clean https://github.com/bashclub/zfs-housekeeping
    - ZFS autosnapshot
  - ceph (?)
  - glusterfs (?)

**Docker**
  - installation https://github.com/stefanux/ansible-role-docker -> substitute with upstream: https://github.com/geerlingguy/ansible-role-docker Vergleich: https://github.com/stefanux/ansible-role-docker/compare/master...geerlingguy:master
  - registry
    - ...?
  - optional management tools:
    - portainer
    - traefik

**Instant messenger**
  - mattermost (Code ready)
  - matrix-synapse / element-web
  - ...?

**Filesharing**
  - samba
    - standalone (2DO: shadowcopy + fruit von bashclub erg??nzen + ZFS) geerlingguy.samba / https://github.com/stefanux/ansible-role-samba.git
    - AD-member "zmb-member" https://github.com/bashclub/zamba-lxc-toolbox
  - nextcloud (-> https://github.com/JGoutin/ansible_home/tree/master/roles/nextcloud ?)
  - S3
    - minio
    - ceph RGW
  - SSH sftpgo
  - SeaweedFS (?)
  - OpenMediaVault / TrueNAS Core?

**Webserver**
  - nginx
    - reverse-proxy
  - Apache ( -> geerlingguy.apache )
    - apache only (simple static sites)
    - redirector
    - LAMP  (-> geerlingguy.php geerlingguy.php-versions )
      - mod_php
      - php-fpm
  - All-in-one-packages
    - froxlor  (Code ready)
    - ispconfig -> sysops.tv?

**TLS-cert + CA-management**
  - letsencrypt
    - certbot https://github.com/stefanux/ansible-role-certbot
    - helper-scripte -> deploy_hook
  - certificate distribution
    - own certs (individual, wildcards) -> Code available -> role
    - vaulted files via sops https://github.com/mozilla/sops ? 2DO)
  - internal CA (creates certs for hosts) -> 2DO

**E-Mail**
  - mailserver (dovecot + postfix)
   - stand-alone
   - backends like LDAP
  - groupware
    - kopano -> sysops.tv?
    - ...?
  - local mailrelay ("satellite")-setup for cron etc.
    - postfix https://github.com/stefanux/ansible-postfix-mailrelay -> can use any SMTP-accounts (2DO include examples for microsoft365, google, a few common providers)
  - archiving
    - (mail-)piler -> sysops.tv
  - spamfiltering
    - rspamd (need redis)
    - spamassassin/policy-weightd/postgrey
  - newsletter: mailman, listmonk, mautic (more a CRM), ...?

**VPN**
  - openvpn
  - wireguard
  - ipsec strongswan (2DO, but low prio because usually this is done on firewalls and wireguard is simpler)
  - (stunnel -> needed?)

**DNS**
  - **self-hosted:*
    - recursive
      - dnsdist (-> powerdns.dnsdist ) + powerDNS-recursor (-> powerdns.pdns_recursor) (clustering: keepalived, csync2-sync von Zertifikaten wenn letsencrypt, nginx-reverse-proxy f??r Statusseite)
      - bind (2DO -> maintainer needed)
      - unbound (2DO -> maintainer needed)
    - autoritative
      - PowerDNS Authoritative (-> in progress)
      - bind -> 2DO: maintainer needed
    - DoT (DNS over TLS)
      - powerdns
      - bind? -> 2DO: maintainer needed
    - DoH / dnscrypt (not supported atm, only if maintainer is found)
    - adfiltering
      - powerdns with filtering (lua-based) - unreleased solution available
      - pihole ?
      - adguard home?
  - **DNS (external service)**:
    - hetzner oder hosttech via https://github.com/ansible-collections/community.dns
    - inwx.de (because they offer official ansible-support, dnssec, anycast and API)
      - example
      - zone-management on inwx (request creation of a API-account via support-ticket)
    - netcup https://docs.ansible.com/ansible/latest/collections/community/general/netcup_dns_module.html
    - cloudflare https://docs.ansible.com/ansible/latest/collections/community/general/cloudflare_dns_module.html
    - AWS/Route53?
    - ...?

**Database**
  - mysql (limited distribution-support or use packages from oracle?; community.mysql )
  - mariadb
    - standalone
    - galera (2DO)
  - PostgreSQL ( -> geerlingguy.postgresql )
  - management-tools:
    - phpmyadmin
    - phpPgAdmin

**Monitoring**
  - check_mk -> sysops.tv
    - including checks/templates (2DO extend List)
  - icinga(2) -> need maintainers
  - zabbix ( community.zabbix )
    - including checks/templates:
      - bacula
      - bareos (2DO)
      - iostat
      - glusterfs
      - mysql
      - postfix
      - pfsense (2DO)
        - wireguard/openvpn/ipsec
      - tcpstats
      - opnsense (2DO)
        - wireguard/openvpn/ipsec
      - strongswan (ipsec)
      - wireguard
      - ZFS https://github.com/stefanux/zabbix_zfs-on-linux
      - ... (2DO extend List)
  - Uptime Kuma (for SoHo or extra monitoring - include simple statuspage)
  - statuspages: cachet, cstate, ... -> need maintainers

**User directory**
  - LDAP?
    - Samba
    - 389dir
    - UCS univention
    - SSSD integratin on system
  - keycloak?
  - ...?

**Firewall**
  - opensense
  - pfsense
  - hostfirewall
    - iptables/nftables geerlingguy.firewall (maybe iptables-persistent ?)
    - ufw

**Clustering**
  - keepalived
  - Filesync:
    - csync2
    - zookeeper

**Reverse-Proxy/Loadbalancer**
  self-hosted:
    - haproxy
    - nginx proxy manager GUI (needs docker)
    - nginx reverse proxy (vanilla)
    - apache mod_proxy (maintainer needed)
  managed (via API):
    - hetzner LB
    - ...?

**package management**
  - build: fpm (effing package manager)
  - host repository: 
    - deb: FIXME
    - rpm: FIXME

**Log-aggregation**
  - grafana loki
  - graylog

**Python**
  - PIP -> geerlingguy.pip

**Apps**
  - Videoconference
    - bbb ( https://github.com/juanluisbaptiste/ansible-bigbluebutton )
    - jitsi via docker (but usage discouraged due to limitations, over-complex architecture and bad documentation)
  - Wiki
    - dokuwiki https://github.com/stefanux/ansible-role-dokuwiki
    - wiki.js? (2Do: maintainer needed)
    - bookstack (2Do: maintainer needed)
    - mediawiki (2Do: maintainer needed)
  - Netbox (IPAM/IT Asset Management)
  - Piwik (2DO)
  - passwortmanager
    - vaultwarden (done)
    - hashicorp vault (2DO maintainer needed)?
    - privacyIDEA (2DO maintainer needed)
  - roundcube webmail
  - Ticketsystems
    - Zammad
  - kimai2 (timetracking)
  - joplin (note taking application)

**candidates**
  - Guacamole (remote desktop gateway)
  - doodle-clones (dudle, framadate, ...)
  - etherpad
  - jellyfin / emby?
  - limesurvey
  - mastodon (twitter-alternative)
  - nodebb
  - peertube
  - teakspeak / mumble
  - wekan? / focalboard?
  - whiteboard https://github.com/cracker0dks/whiteboard (could be replaced by videoconferencing-tool like bbb)
  - wordpress



Requirements
------------

minimum ansible version: 2.10

Role requirements
-----------------

good documentation (preferably a link to a basic user documentation too)

example playbook

proper code formatting (ansible-lint)

classify variables per role 
- required (execution fails if not defined)
- recommended (common customization)
- optional (less commonly changed)

dependencies
- other roles
- neede pip-modules on target

definition of variables and dependencies (see above) need to be machine-readable to enable automation/custom GUIs) -> 2DO define fileformat!

python3 is required

active maintainers!


Dependencies
------------

FIXME Link requirements-file
FIXME list all include_role in requirements-file


License
-------

open discussion: should we enables commercial forking (BSD? MIT?)?
... or do we force GPLv3?

FAQ
---

Q: Why not plain shellscripts?
A: Shellscript have full flexibility ... but you`ll need to implement everything yourself:
- templating with condition and variable expansion
- handlers (run action when certain condition are met, i.e. restart service only when config is changed via template)
- are not idempotent (it does not have the same result when you run it again)
- re-implement code stuff that is already available today (ansible galaxy has tons of code)
- validate config for services that offer it (i.e. prevent broken sudo configs ...)
- automated/unattended run (installations are not always done interactivly done by a humans!)
- error-handling: try to trap errors with pipefail ... that blows up code massively. Example for error-handling in bash (how many of your scripts does implement something similar?):
~~~
set -eo pipefail

cleanup() {
# remove temp files
}
trap cleanup 0

error() {
  local parent_lineno="$1"
  local message="$2"
  local code="${3:-1}"
  if [[ -n "$message" ]] ; then
    error_message="Error on or near line ${parent_lineno}: ${message}; exiting with status ${code}"
  else
    error_message="Error on or near line ${parent_lineno}; exiting with status ${code}"
  fi
  echo "$error_message" # for cron
  exit "${code}"
}
trap 'error ${LINENO}' ERR
~~~


similiar projects
-----------------

- https://github.com/tteck/Proxmox
- debops https://docs.debops.org/en/stable-3.0/
- https://github.com/JGoutin/ansible_home
- https://github.com/davestephens/ansible-nas

appstores
- cloudron: https://www.cloudron.io/store/index.html
- yunohost: https://yunohost.org/en/apps
- 
