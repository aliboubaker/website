Bien sûr Ali — et bonne nouvelle : même si je ne peux pas **créer directement un fichier téléchargeable** dans ce contexte précis, je peux te fournir **le contenu exact d’un fichier `README.md` prêt à être collé dans ton projet**, sans rien à modifier.

Tu n’as qu’à créer un fichier `README.md` dans ton repo et y coller ce contenu.

---

# 📄 **README.md (prêt à l’emploi)**

```md
# 🏢 DadCorp – Configuration Apache & Virtual Hosts

Ce projet documente la configuration d’hôtes virtuels (VirtualHosts) basés sur l’adresse IP pour un serveur Apache sous RHEL7.  
Deux environnements sont configurés :

- Un VirtualHost sur le **port 80**
- Un VirtualHost sur le **port 8080**

---

## 📁 Structure du projet

```
/opt/website              → Contenu du site DadCorp
/etc/httpd/conf/httpd.conf
/etc/httpd/conf.d/DadCorp.conf
```

---

## ⚙️ 1. Configuration du VirtualHost sur le port 80

### 🔧 Modifier la configuration principale Apache

Ouvrir le fichier :

```bash
vim /etc/httpd/conf/httpd.conf
```

Commenter la directive :

```apache
#Listen 80
```

Enregistrer et quitter :

```
:wq
```

---

## 📄 2. Création du fichier DadCorp.conf

Se déplacer dans le dossier :

```bash
cd /etc/httpd/conf.d/
```

Créer le fichier :

```bash
vim DadCorp.conf
```

Ajouter :

```apache
Listen 80

<Directory "/opt">
    AllowOverride None
    Require all granted
</Directory>

<VirtualHost PRIVATE_IP:80>
    DocumentRoot "/opt/website"
    ServerName www.dadcorp.com
</VirtualHost>
```

> Remplacer **PRIVATE_IP** par l’adresse IP privée du serveur RHEL7.

---

## 🔄 3. Redémarrer Apache

```bash
systemctl restart httpd
```

---

## 🧪 4. Vérification

```bash
curl www.dadcorp.com
```

Vous ne devez **pas** voir la page par défaut CentOS.

---

# ⚙️ 5. Configuration du VirtualHost sur le port 8080

Réouvrir le fichier :

```bash
vim /etc/httpd/conf.d/DadCorp.conf
```

Ajouter :

```apache
Listen 8080
```

Dupliquer le VirtualHost et modifier le port :

```apache
Listen 80
Listen 8080

<Directory "/opt">
    AllowOverride None
    Require all granted
</Directory>

<VirtualHost PRIVATE_IP:80>
    DocumentRoot "/opt/website"
    ServerName www.dadcorp.com
</VirtualHost>

<VirtualHost PRIVATE_IP:8080>
    DocumentRoot "/opt/website"
    ServerName www.dadcorp.com
</VirtualHost>
```

---

## 🔄 6. Redémarrer Apache

```bash
systemctl restart httpd
```

---

## 🧪 7. Vérification

```bash
curl www.dadcorp.com:8080
```

Vous devez voir la même page que sur le port 80.

---

# 🌐 8. Test via navigateur

Remplacer **PUBLIC_IP** par l’IP publique du serveur :

```
http://PUBLIC_IP:8080
```

Puis tester :

```
http://PUBLIC_IP
```

Le site DadCorp doit s’afficher dans les deux cas.

---

## 📝 Notes utiles

### Utilisation de Vim pour coller du texte proprement

```bash
:set paste
i
```

Quitter et enregistrer :

```
:wq
```

Quitter sans enregistrer :

```
:q!
```

---

```

---

Si tu veux, je peux aussi te générer :

- une version **plus stylée** (badges GitHub, table des matières, sections avancées)
- une version **anglaise**
- une version **adaptée à un projet DevOps / Cloud**

Tu veux une version plus pro ou tu gardes celle‑ci ?
