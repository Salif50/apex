## configuration
```bash
ords --config /chemin/vers/config serve
```

Cependant, avant de pouvoir utiliser cette commande, vous devez créer et configurer ce répertoire de configuration. Voici une procédure détaillée pour **obtenir et configurer le dossier `config`** pour ORDS :

---

## **Étapes pour Obtenir et Configurer le Dossier `config` pour ORDS**

### **1. Prérequis**

Avant de commencer, assurez-vous d’avoir :

- **Java Development Kit (JDK) 8 ou ultérieur** installé et configuré (`JAVA_HOME` défini).
- **ORDS téléchargé** (le fichier `ords.war`).
- **Oracle Database** et **Oracle APEX** installés et configurés.
- **Accès administrateur** sur le système pour exécuter les commandes nécessaires.

### **2. Créer un Répertoire de Configuration pour ORDS**

Choisissez un emplacement pour stocker les fichiers de configuration d’ORDS. Par exemple :

```bash
mkdir -p /opt/ords/config
```

### **3. Initialiser le Répertoire de Configuration avec ORDS**

Exécutez la commande suivante pour initialiser le répertoire de configuration :

```bash
java -jar ords.war configdir /opt/ords/config
```

**Explications :**

- `java -jar ords.war configdir /opt/ords/config` : Cette commande crée les fichiers de configuration nécessaires dans le répertoire spécifié (`/opt/ords/config`).

### **4. Configurer ORDS pour se Connecter à la Base de Données Oracle et APEX**

Après avoir créé le répertoire de configuration, vous devez configurer ORDS pour qu’il puisse se connecter à votre base de données Oracle et servir Oracle APEX.

#### **Étape 4.1 : Exécuter la Configuration Initiale d’ORDS**

Naviguez vers le répertoire d’ORDS et exécutez la commande d’installation :

```bash
java -jar ords.war install --config /opt/ords/config
```

**Remplissez les informations suivantes lorsque vous y êtes invité :**

1. **Informations de la base de données :**
   - **Host** : `localhost` (ou l’adresse IP de votre serveur Oracle)
   - **Port** : `1521`
   - **SID** ou **Service Name** : `ORCL` (ou le nom de votre base de données)
   - **Utilisateur** : `SYS`
   - **Mot de passe** : Mot de passe de l’utilisateur `SYS`
   - **Rôle** : `SYSDBA`

2. **Configurer pour Oracle APEX :**
   - Lorsque vous êtes invité à configurer APEX, répondez `y` (oui).
   - **Chemin d’installation des images APEX** : `/i/` (ou le chemin que vous avez utilisé lors de l’installation d’APEX).

#### **Étape 4.2 : Vérifier la Configuration**

Assurez-vous que les fichiers de configuration (`ords_params.properties`, etc.) sont correctement générés dans le répertoire `/opt/ords/config`.

### **5. Démarrer ORDS en Mode Serveur avec le Répertoire de Configuration**

Une fois la configuration terminée, vous pouvez démarrer ORDS en spécifiant le répertoire de configuration :

```bash
java -jar ords.war --config /opt/ords/config serve
```

**Options supplémentaires :**

- **Changer le Port :** Si vous souhaitez utiliser un port différent (par exemple, `9090`), utilisez l’option `--port` :
  ```bash
  java -jar ords.war --config /opt/ords/config serve --port 9090
  ```

- **Configurer SSL (HTTPS) :** Pour sécuriser les communications, configurez SSL en spécifiant le keystore et le mot de passe :
  ```bash
  java -jar ords.war --config /opt/ords/config serve --ssl-port 8443 --ssl-keystore /chemin/vers/keystore.jks --ssl-keystore-password votre_mot_de_passe
  ```

### **6. Vérifier que ORDS Fonctionne Correctement**

Après avoir démarré ORDS, vérifiez son bon fonctionnement en accédant à l’URL suivante dans votre navigateur :

```
http://localhost:8080/ords
```

*(Remplacez `8080` par le port que vous avez configuré si vous avez changé le port par défaut.)*

Vous devriez voir la page de connexion d’Oracle APEX. Utilisez les identifiants administrateur que vous avez définis lors de l’installation d’APEX pour vous connecter.

### **7. Automatiser le Démarrage d’ORDS (Optionnel)**

Pour éviter de devoir démarrer ORDS manuellement à chaque redémarrage du système, vous pouvez configurer ORDS pour qu’il démarre automatiquement en tant que service. Voici comment procéder sous **Linux** et **Windows** :

#### **Sous Linux (Systemd)**

1. **Créer un fichier de service :**

   ```bash
   sudo vi /etc/systemd/system/ords.service
   ```

2. **Ajouter les lignes suivantes :**

   ```ini
   [Unit]
   Description=Oracle REST Data Services
   After=network.target

   [Service]
   Type=simple
   User=oracle
   ExecStart=/usr/bin/java -jar /opt/ords/ords.war --config /opt/ords/config serve
   Restart=on-failure

   [Install]
   WantedBy=multi-user.target
   ```

3. **Recharger les configurations systemd et activer le service :**

   ```bash
   sudo systemctl daemon-reload
   sudo systemctl enable ords
   sudo systemctl start ords
   ```

4. **Vérifier le statut du service :**

   ```bash
   sudo systemctl status ords
   ```

#### **Sous Windows**

1. **Télécharger et installer NSSM (Non-Sucking Service Manager) :**

   - Téléchargez NSSM depuis [nssm.cc](https://nssm.cc/download).
   - Extrayez `nssm.exe` dans un répertoire accessible, par exemple `C:\nssm\nssm.exe`.

2. **Créer un service ORDS avec NSSM :**

   Ouvrez **Invite de commandes** en tant qu’administrateur et exécutez :

   ```cmd
   C:\nssm\nssm.exe install ORDS
   ```

3. **Configurer le service dans NSSM :**

   - **Application Path** : `java`
   - **Arguments** : `-jar "C:\chemin\vers\ords.war" --config "C:\chemin\vers\config" serve`
   - **Working Directory** : `C:\chemin\vers\ords\`

4. **Configurer les paramètres supplémentaires si nécessaire**, puis cliquez sur **Install service**.

5. **Démarrer le service :**

   ```cmd
   net start ORDS
   ```

### **8. Résolution des Problèmes Courants**

| **Problème**                         | **Solution**                                                                                                                                          |
|--------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Erreur `404 Not Found`**           | - Vérifiez que APEX est correctement installé dans la base de données.<br>- Assurez-vous que ORDS est correctement configuré et en cours d'exécution.   |
| **ORDS ne démarre pas**              | - Vérifiez l’installation de Java (`java -version`).<br>- Assurez-vous que les ports nécessaires ne sont pas bloqués ou déjà utilisés.<br>- Consultez les logs d’ORDS pour plus de détails. |
| **Problèmes de connexion à la DB**   | - Vérifiez les paramètres de connexion (hôte, port, SID/service name).<br>- Assurez-vous que le listener Oracle est actif (`lsnrctl status`).          |
| **Performance lente**                | - Augmentez les paramètres de mémoire Java (`-Xms` et `-Xmx`).<br>- Optimisez la configuration de la base de données Oracle (indices, paramètres de performance). |
| **Accès refusé à l'interface de gestion** | - Vérifiez les configurations d’ORDS.<br>- Assurez-vous que les utilisateurs et rôles sont correctement définis dans les fichiers de configuration.          |

### **Résumé des Étapes**

| **Étape**                           | **Description**                                                                                                                               |
|-------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------|
| **1. Prérequis**                    | Vérifiez les composants nécessaires (JDK, Oracle DB, APEX, ORDS) et préparez l'environnement.                                               |
| **2. Téléchargements**              | Téléchargez JDK, Oracle Database, APEX et ORDS depuis les sites officiels d'Oracle.                                                        |
| **3. Installation de la DB Oracle** | Installez Oracle Database, configurez le listener, définissez les variables d'environnement et vérifiez l'installation via SQL*Plus.           |
| **4. Installation d'APEX**          | Extrayez APEX, installez-le dans la base de données, configurez l'administrateur et les images APEX.                                         |
| **5. Installation d'ORDS**          | Créez le répertoire de configuration, exécutez la configuration initiale d’ORDS, configurez ORDS pour la base de données et APEX.             |
| **6. Lancement d'ORDS**             | Démarrez ORDS en mode serveur en spécifiant le répertoire de configuration.                                                                  |
| **7. Accès à APEX**                  | Connectez-vous à Oracle APEX via l'URL fournie et commencez à développer vos applications.                                                   |
| **8. Configuration Avancée**        | Changez les ports, configurez SSL, optimisez la mémoire Java et résolvez les problèmes courants.                                              |

---

