# DEV_TOOLS.md - Tutoriels
*Procédures d'installations d'outils sous Linux*
---
## `ghq` - Un gestionnaire de repositories GitHub, Gitlab etc.
---
**Installation sous Ubuntu 22.04 LTS**
```bash
cd /tmp
git clone https://github.com/x-motemen/ghq
sudo apt install golang
cd /tmp/ghq
make install
cd ~
nano .bashrc
```
Ajouter les lignes dans `.bashrc` :
```bash
export GOPATH=$(go env GOPATH)
export PATH=$GOPATH/bin:$PATH
```

