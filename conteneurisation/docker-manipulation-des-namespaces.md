# Docker manipulation des namespaces

Création d'un conteneur basé sur une image ubuntu

```
docker run --rm -it ubuntu
Unable to find image 'ubuntu:latest' locally
latest: Pulling from library/ubuntu
9502885e1cbc: Pull complete 
Digest: sha256:3f85b7caad41a95462cf5b787d8a04604c8262cdcdf9a472b8c52ef83375fe15
Status: Downloaded newer image for ubuntu:latest
root@9a282bb5c76c:/# whoami
root

```

Exécution de la commande `mount` pour vérifier les points de montage avec le système hôte

```
root@129ea75cea26:/# mount
overlay on / type overlay (rw,relatime,lowerdir=/var/lib/docker/overlay2/l/EQMVOVWQG5JYZEUPDODL3MDAYE:/var/lib/docker/overlay2/l/RG2ETQZ57DM43VP3F3S5FOV76D,upperdir=/var/lib/docker/overlay2/d89546b5da0e225146cfb0220f988cc1aa6da2a1dbf32cf4e277f7e2342cc675/diff,workdir=/var/lib/docker/overlay2/d89546b5da0e225146cfb0220f988cc1aa6da2a1dbf32cf4e277f7e2342cc675/work)
proc on /proc type proc (rw,nosuid,nodev,noexec,relatime)
tmpfs on /dev type tmpfs (rw,nosuid,size=65536k,mode=755)
devpts on /dev/pts type devpts (rw,nosuid,noexec,relatime,gid=5,mode=620,ptmxmode=666)
sysfs on /sys type sysfs (ro,nosuid,nodev,noexec,relatime)
cgroup on /sys/fs/cgroup type cgroup2 (ro,nosuid,nodev,noexec,relatime)
mqueue on /dev/mqueue type mqueue (rw,nosuid,nodev,noexec,relatime)
shm on /dev/shm type tmpfs (rw,nosuid,nodev,noexec,relatime,size=65536k)
/dev/vda1 on /etc/resolv.conf type ext4 (rw,relatime,discard)
/dev/vda1 on /etc/hostname type ext4 (rw,relatime,discard)
/dev/vda1 on /etc/hosts type ext4 (rw,relatime,discard)
devpts on /dev/console type devpts (rw,nosuid,noexec,relatime,gid=5,mode=620,ptmxmode=666)
proc on /proc/bus type proc (ro,nosuid,nodev,noexec,relatime)
proc on /proc/fs type proc (ro,nosuid,nodev,noexec,relatime)
proc on /proc/irq type proc (ro,nosuid,nodev,noexec,relatime)
proc on /proc/sys type proc (ro,nosuid,nodev,noexec,relatime)
proc on /proc/sysrq-trigger type proc (ro,nosuid,nodev,noexec,relatime)
tmpfs on /proc/kcore type tmpfs (rw,nosuid,size=65536k,mode=755)
tmpfs on /proc/keys type tmpfs (rw,nosuid,size=65536k,mode=755)
tmpfs on /proc/timer_list type tmpfs (rw,nosuid,size=65536k,mode=755)
tmpfs on /sys/firmware type tmpfs (ro,relatime)
```

La variable `upperdir` correspond à :\
`upperdir=/var/lib/docker/overlay2/d89546b5da0e225146cfb0220f988cc1aa6da2a1dbf32cf4e277f7e2342cc675/diff`

Si on crée un fichier "toto" dans le conteneur, on pourra le retrouver dans cette arborescence sur le système hôte



