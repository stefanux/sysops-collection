Purpose and principles
======================

This collection should enable self-hosters full control of their infrastructure and data (GDPR-focus when external services are used).
It will be OSS forever and everyone should be able to integrate it into their infrastructure (or product).
We do not re-invent the wheel, if there is a good role or collection somewhere else: we won`t duplicate it.

supported distributions:
  - Debian (all supported versions)
  - Ubuntu (supported LTS-Versions)
  - Redhat-based distribution (if maintainer is found)

2DO in general:
  - CI (travis, gitlab runner, ...?)


baserole feature-list
=====================

- packages
  - default packages
  - upgrade
  - unattended upgrades ( -> jnv.unattended-upgrades )
- DNS
  - resolv.conf
  - FQDN (+reverse) 2DO (check if hostname --fqdn is not just hostname)
- SSHD-config
- Usermanagement (incl. authorized keys und sudo) 2DO generic with dictionary
- config systemapps
  - htop
  - bashrc
- fail2ban (-> oefenweb.fail2ban ; optional helpers https://github.com/stefanux/fail2ban-remove-ban)
- NTP -> geerlingguy.ntp

Recommended
===========

VM creation
  - libvirt/KVM/cloud-init ( tcharl.ansible_role_libvirt_host oder community.libvirt?, cloud-init-example on https://github.com/stefanux/cloud-init-example)
  - proxmox/cloud-init (in progress)

LXC on proxmox (maintainer -> sysops.tv?)

mailrelay: postfix (see E-Mail) for system-mails (like cron etc.) or apps: https://github.com/stefanux/ansible-postfix-mailrelay


Optional roles
==============

Backup
  - bacula https://github.com/stefanux/ansible-role-bacula
  - bareos (2DO)
  - restic -> Rainer?
  - mysql/mariadb (-> geerlingguy.mysql )
    - fulldump (SQL commands):
      - mysqlbackup https://github.com/stefanux/ansible-mysqlbackup
      - automysqlbackup (maintained?) https://github.com/stefanux/automysqlbackup
    - other methods:
      - FIXME
  - pfsense config
  - opensense config
  - etckepper (2DO)

Git
  - git (client) -> geerlingguy.git
  - gitea https://github.com/stefanux/ansible-role-gitea ( -> maintainer needed)
  - gitlab -> geerlingguy.gitlab

ZFS
  - vanilla install (2DO)
  - special-usecases:
    - Proxmox https://github.com/bashclub/proxmox-zfs-postinstall
    - Samba shadow-copies "Zamba" https://github.com/bashclub/zamba-lxc-toolbox/
  - snapshot house-keeping: zfs-keep-and-clean https://github.com/bashclub/zfs-housekeeping

docker
  - installation https://github.com/stefanux/ansible-role-docker -> substitute with upstream: https://github.com/geerlingguy/ansible-role-docker Vergleich: https://github.com/stefanux/ansible-role-docker/compare/master...geerlingguy:master
  - registry
    - ...?

Instant messenger
  - mattermost
  - matrix-synapse / element-web

Filesharing
  - samba
    - standalone (2DO: shadowcopy + fruit von bashclub ergänzen + ZFS)
    - AD-member "zmb-member" https://github.com/bashclub/zamba-lxc-toolbox
  - nextcloud

webserver
  - nginx
    - reverse-proxy
  - Apache ( -> geerlingguy.apache )
    - simples LAMP
    - redirector
    - mehr LAMP  (-> geerlingguy.php geerlingguy.php-versions )
  - All-in-one-Paket
    - froxlor
    - ispconfig -> sysops.tv

TLS-cert + CA-management
  - letsencrypt
    - certbot https://github.com/stefanux/ansible-role-certbot
    - helper-scripte -> deploy_hook
  - TLS distribution
    - own certs (individual, wildcards)
    - vaulted files via sops https://github.com/mozilla/sops ?)
  - internal CA (creates certs for hosts)

E-Mail
  - mailserver dovecot incl. postfix
  - archiving
    - mailpiler -> sysops.tv
  - spamfiltering
    - rspamd (need redis)
    - spamassassin/policy-weightd/postgrey
  - postfix mailrelay (see E-Mail) for cron etc. https://github.com/stefanux/ansible-postfix-mailrelay
    -> can use any SMTP-Relay (2DO include examples for microsoft365, google, a few common providers)

VPN
  - openvpn
  - wireguard
  - ipsec strongswan (2DO, but low prio because usually this is done on firewalls and wireguard is simpler)
  - (stunnel -> needed?)

DNS (server)
  - recursive
    - dnsdist (-> powerdns.dnsdist ) + powerDNS-recursor (-> powerdns.pdns_recursor) (clustering: keepalived, csync2-sync von Zertifikaten wenn letsencrypt, nginx-reverse-proxy für Statusseite)
      - with filtering (lua-based) or without
    - bind (2DO) -> example on mx1.stefanux.net
    - pihole ?
  - autoritative
    - PowerDNS Authoritative (-> in progress)
    - bind -> 2DO: maintainer needed
  - DoT
    - powerdns
    - bind? -> 2DO: maintainer needed
  - DoH / dnscrypt (not supported atm, only if maintainer is found)


DNS (external):
  - inwx (because they offer official ansible-support, dnssec, anycast and API)
    - example
    - zone-management on inwx (request creation of a API-account via support-ticket)
  - AWS/Route53?
  - ...?

Database
  - mysql (limited distribution-support or use packages from oracle?; community.mysql )
  - mariadb
    - standalone
    - galera (2DO)
  - PostgreSQL ( -> geerlingguy.postgresql )

Monitoring
  - zabbix ( community.zabbix )
    - including checks/templates:
      - https://github.com/stefanux/zabbix_zfs-on-linux
      - pfsense (2DO)
      - opnsense (2DO)
      - ... (2DO extend List)
  - check_mk -> sysops.tv
    - including checks/templates
  - icinga(2) -> need maintainers
  - Uptime Kuma (for SoHo or extra monitoring - include simple statuspage)
  - statuspages (specialists): cachet, cstate

User directory
  - LDAP*?
    - Samba
    - 389dir
    - UCS univention
    - SSSD integratin on system
  - keycloak?
  - ...?

Firewall
  - opensense
  - pfsense

Clustering
  - keepalived

Reverse-Proxy/Loadbalancer
  - haproxy
  - nginx proxy manager GUI (needs docker)
  - nginx reverse proxy (vanilla)
  - apache mod_proxy (maintainer needed)


Python
  - PIP -> geerlingguy.pip

Apps
  - Videoconference
    - bbb ( https://github.com/juanluisbaptiste/ansible-bigbluebutton )
    - jitsi via docker (but usage discouraged due to limitations, over-complex architecture and bad documentation)
  - Wiki
    - dokuwiki https://github.com/stefanux/ansible-role-dokuwiki
    - wiki.js? (2Do: maintainer needed)
  - Piwik (2DO)
  - passwortmanager
    - vaultwarden
    - hashicorp vault (maintainer needed)?
    - privacyIDEA (-> mlands ?)
  - roundcube webmail (-> inwx)

Requirements
------------

minimum ansible version: 2.10

Role requirements
-----------------

good documentation!

example playbook

define variables:
 (sollte pro Rolle (auch automatisiert einlesbar!) definiert werden):
- required (execution fails if not defined):
- recommended
- optional

Dependencies
------------

FIXME Link requirements-file
FIXME list all include_role in requirements-file


License
-------

should we enables commercial forking (BSD? MIT?)?
... or do we force GPLv3?

FAQ
---

Q: Why no shellscripts?
A: Shellscript have limited capabilities and you would to re-implements lots of modules or language features (templating, handlers, are not idempotent, error-handlung etc.)
