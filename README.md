# samba-ad-dc

[![gh-actions](https://github.com/diegogslomp/samba-ad-dc/actions/workflows/almalinux-image.yml/badge.svg)](https://github.com/diegogslomp/samba-ad-dc/actions/workflows/almalinux-image.yml)
[![gh-actions](https://github.com/diegogslomp/samba-ad-dc/actions/workflows/rockylinux-image.yml/badge.svg)](https://github.com/diegogslomp/samba-ad-dc/actions/workflows/rockylinux-image.yml)
[![gh-actions](https://github.com/diegogslomp/samba-ad-dc/actions/workflows/debian-image.yml/badge.svg)](https://github.com/diegogslomp/samba-ad-dc/actions/workflows/debian-image.yml)
[![gh-actions](https://github.com/diegogslomp/samba-ad-dc/actions/workflows/ubuntu-image.yml/badge.svg)](https://github.com/diegogslomp/samba-ad-dc/actions/workflows/ubuntu-image.yml)

Samba Active Directory Domain Controller Docker Image

Provision a new domain:
```
docker run -d --privileged \
  --restart=unless-stopped --network=host \
  -e REALM='SAMDOM.EXAMPLE.COM' \
  -e DOMAIN='SAMDOM' \
  -e ADMIN_PASS='Passw0rd' \
  -e DNS_FORWARDER='8.8.8.8' \
  -v dc1-samba:/usr/local/samba \
  --name dc1 --hostname DC1 diegogslomp/samba-ad-dc
```

Show logs (Ctrl+c to exit) and run tests:
```
docker logs dc1 -f
docker exec dc1 samba-tests
```

For multiple DCs testing:
```
git clone --single-branch https://github.com/diegogslomp/samba-ad-dc
cd samba-ad-dc
docker compose build
docker compose up -d
docker compose logs -f
for dc in dc{1,2,3,4}; do docker compose exec $dc samba-tests; done
```

TODO:

  - [ ] [Sysvol replication workaround](https://wiki.samba.org/index.php/Rsync_based_SysVol_replication_workaround)

Official site: https://wiki.samba.org/index.php/Setting_up_Samba_as_an_Active_Directory_Domain_Controller

Dependencies: https://wiki.samba.org/index.php/Package_Dependencies_Required_to_Build_Samba

Exposed ports: https://wiki.samba.org/index.php/Samba_AD_DC_Port_Usage
