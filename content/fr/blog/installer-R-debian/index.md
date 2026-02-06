---
title: Installer R sur Debian
format: hugo-md
---


> **Attention !**
>
> Cette page contient des extraits de code `bash`. Il faut toujours comprendre ce que le code proposé fait avant de l'exécuter, d'autant plus plus lorsque ce code nécessite les privilèges d'administration (`sudo`).

### Comment avoir une version de R à jour sur Debian ?

Sur Debian, il est possible d'installer `R` à partir du gestionnaire de paquets `apt`. Toutefois, la version de `R` sur les dépôts Debian est généralement moins à jour que la version disponible sur le CRAN. Pour installer la dernière version de `R` sur Debian il est possible d'ajouter le CRAN à la liste des dépôts gérés par `apt`.

> **Tip**
>
> CRAN (Comprehensive R Archive Network)

### Ajouter le CRAN à la liste des dépôts de `apt`

On commence par ajouter l'adresse de CRAN à la liste des dépôts de `apt` :

``` bash
sudo echo "deb http://cloud.r-project.org/bin/linux/debian bookworm-cran40/" > /etc/apt/sources.list.d/cran.list
```

Le script permet d'ajouter l'adresse dans un nouveau fichier, `cran.list`, que l'on place dans le répertoire `/etc/apt/sources.list.d/`. Ce répertoire contient des fichiers de listes de dépôts, cela permet de ne pas modifier le fichier `/etc/apt/sources.list`.

### Sécuriser les échanges entre `apt` et le dépôt CRAN

Après avoir ajouté l'adresse du dépôt, il faut désormais récupérer la clé GPG publique du dépôt afin de sécuriser les échange avec celui-ci.

On télécharge la clé :

``` bash
gpg --keyserver keyserver.ubuntu.com \
    --recv-key '95C0FAF38DB3CCAD0C080A7BDC78B2DDEABC47B7'
```

La clé est stockée sur un serveur de stockage de clés publiques (`keyserver.ubuntu.com`) et son empreinte (i.e. son identifiant unique) est `95C0FAF38DB3CCAD0C080A7BDC78B2DDEABC47B7`. Il serait aussi possible de trouver cette clé en se rendant directement sur le site [keyserver.ubuntu.com](https://keyserver.ubuntu.com/).

Une fois la clé téléchargée, on la stocke dans le répertoire dédié de `apt` :

``` bash
gpg --armor --export '95C0FAF38DB3CCAD0C080A7BDC78B2DDEABC47B7' | \
    sudo tee /etc/apt/trusted.gpg.d/cran_debian_key.asc
```

La clé est convertie en un format ASCII. Elle est ensuite stockée dans le fichier `cran_debian_key.asc` dans le répertoire `/etc/apt/trusted.gpg.d/` qui contient les clés que le gestionnaire de paquets apt a besoin pour sécuriser la communication avec les dépôts.

> **Tip**
>
> Une clé GPG (GNU Privacy Guard) est un système de signature cryptographique permettant la communication sécurisée entre deux ordinateurs.

### Installer R

Maintenant que le dépôt et sa clé sont configurés, on peut mettre à jour la liste des dépôts :

``` bash
sudo apt update
```

Si tout se passe bien on devrait voir passer une ligne avec l'adresse du dépôt CRAN ajouté plus haut.

Enfin, on peut installer R à partir du nouveau dépôt :

``` bash
sudo apt install r-base r-base-dev
```

On peut vérifier la version de `R` installée :

``` bash
R --version
```

> **Références**
>
> Les étapes décrites ci-dessus sont tirées de la [page Debian de CRAN](https://cran.r-project.org/bin/linux/debian).
