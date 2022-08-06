# config.md - Configuration pour LFS
*Procédures d'installations d'outils sous Linux*
---
**Linux :** `Debian 11 Bullseye` + LFS V11.1
## Ajouter un utilisateur aux *sudoers*
```bash
> su root # Changement d'utilisateur -> root
[root] > sudo visudo
# nano
---
# User privilege specification
root ALL=(ALL:ALL) ALL
<username> ALL=(ALL:ALL) ALL
---
[root] > su <username>
```
Autre version plus propre :
```bash
> su root
[root] > sudo usermod -a -G sudo <username>
[root] > su <username>
```
## Installation
---
```bash
> sudo apt install m4 gawk build-essential texinfo bison
```
## `version-check.sh` - Vérification du système
Pour régler `ERROR: /bin/sh does not point to bash`:
```bash
> sudo unlink /bin/sh
> sudo ln -s bash /bin/sh
```
Script officiel pour les spécifications requises :
```bash
#!/bin/bash
# Simple script to list version numbers of critical development tools
export LC_ALL=C
bash --version | head -n1 | cut -d" " -f2-4
MYSH=$(readlink -f /bin/sh)
echo "/bin/sh -> $MYSH"
echo $MYSH | grep -q bash || echo "ERROR: /bin/sh does not point to bash"
unset MYSH

echo -n "Binutils: "; ld --version | head -n1 | cut -d" " -f3-
bison --version | head -n1

if [ -h /usr/bin/yacc ]; then
  echo "/usr/bin/yacc -> `readlink -f /usr/bin/yacc`";
elif [ -x /usr/bin/yacc ]; then
  echo yacc is `/usr/bin/yacc --version | head -n1`
else
  echo "yacc not found"
fi

echo -n "Coreutils: "; chown --version | head -n1 | cut -d")" -f2
diff --version | head -n1
find --version | head -n1
gawk --version | head -n1

if [ -h /usr/bin/awk ]; then
  echo "/usr/bin/awk -> `readlink -f /usr/bin/awk`";
elif [ -x /usr/bin/awk ]; then
  echo awk is `/usr/bin/awk --version | head -n1`
else
  echo "awk not found"
fi

gcc --version | head -n1
g++ --version | head -n1
grep --version | head -n1
gzip --version | head -n1
cat /proc/version
m4 --version | head -n1
make --version | head -n1
patch --version | head -n1
echo Perl `perl -V:version`
python3 --version
sed --version | head -n1
tar --version | head -n1
makeinfo --version | head -n1  # texinfo version
xz --version | head -n1

echo 'int main(){}' > dummy.c && g++ -o dummy dummy.c
if [ -x dummy ]
  then echo "g++ compilation OK";
  else echo "g++ compilation failed"; fi
rm -f dummy.c dummy
```
## Partionner le disque et créer le système de fichier `ext4`
```bash
> sudo fdisk -l
> sudo cfdisk /dev/diskname
```
Créer une table de partition de type `dos` > partition n°1 : `max(Space)` - 2 GB (bootable, 83 Linux) + partition n°2 (2GB, 82 Linux swap/Solaris.
```bash
> sudo mkfs -v -t ext4 <diskname/part1>
> sudo mkswap <diskname/part2>
> echo "LFS=/mnt/lfs" >> ~/.bashrc
> bash 
> echo $LFS # Renvoie /mnt/lfs
> sudo mkdir -pv $LFS
> sudo mount v -t ext4 <diskname/part1> $LFS
> cd $LFS
> sudo /sbin/swapon -v <diskname/part2>
```

