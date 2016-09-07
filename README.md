# Kubernetes definitions and configuration
- Mostly site specific, use those as examples
- Secrets and private configuration is referenced but (obviously) not included
  - `default` secrets
  - `traefik-config` configMap

# Manual steps (aka: FIXME)
## Create database users: 5pi/infra#12
- `kubectl create -f secrets.yml`
- Find postgres pod
- `kubectl exec -ti postgres-2416409090-jf8uh -- psql -U postgres`
- `create user ghost_ada with password 'pwd-from-secrets.yml';`
- `create database ghost_ada;`
- `grant all privileges on database ghost_ada to ghost_ada;`

- `create user ghost_fish with password 'pwd-from-secrets.yml';`
- `create database ghost_fish;`
- `grant all privileges on database ghost_fish to ghost_fish;`

## Create volumes 5pi/infra#13
For each volume:

```
do-volume create ghost-fish 1
do-volume attach '{ "volume": "ghost-fish" }'
mkfs.ext4 /dev/disk/by-id/scsi-0DO_Volume_ghost-fish

# For ghost data volumes:
mount /dev/disk/by-id/scsi-0DO_Volume_ghost-fish /mnt
mkdir -m 777 /mnt/{apps,data,images} # See 5pi/infra#11
umount /mnt

do-volume detach /dev/disk/by-id/scsi-0DO_Volume_ghost-fish
```

