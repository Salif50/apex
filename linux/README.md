## INSTALLATION SUR LINUX

## **Table des Matières**

1. [Prérequis](#1-prérequis)
2. [Téléchargements](#2-téléchargements)
3. [Installation de la Base de Données Oracle](#3-installation-de-la-base-de-données-oracle)
4. [Installation et Configuration d'Oracle APEX](#4-installation-et-configuration-doracle-apex)
5. [Installation et Configuration d'ORDS en Mode Autonome](#5-installation-et-configuration-dords-en-mode-autonome)
6. [Lancement d'ORDS en Mode Autonome](#6-lancement-dords-en-mode-autonome)
7. [Accès à Oracle APEX via le Navigateur](#7-accès-à-oracle-apex-via-le-navigateur)
8. [Configuration Avancée et Résolution des Problèmes](#8-configuration-avancée-et-résolution-des-problèmes)
9. [Résumé des Étapes](#9-résumé-des-étapes)

---

## **1. Prérequis**

Avant de commencer, assurez-vous d'avoir les éléments suivants installés et configurés sur votre système Linux :

- **Accès root ou sudo** : Pour installer des logiciels et configurer le système.
- **Java Development Kit (JDK) 8 ou ultérieur**
- **Oracle Database** (versions 11g, 12c, 18c, 19c, etc.)
- **Oracle APEX** (la version souhaitée)
- **Oracle REST Data Services (ORDS)**
- **SQL Developer** (optionnel, pour faciliter l'exécution des scripts SQL)
- **Ports ouverts** : Assurez-vous que les ports **1521** (Oracle DB) et **8080** (ORDS) sont ouverts.

---

## **2. Téléchargements**

Téléchargez les composants nécessaires depuis les sites officiels d'Oracle :

| **Composant**                | **Lien de Téléchargement**                                      | **Remarques**                                |
|------------------------------|-----------------------------------------------------------------|----------------------------------------------|
| **Java Development Kit**    | [Télécharger JDK](https://www.oracle.com/java/technologies/javase-downloads.html) | Téléchargez la version appropriée pour Linux |
| **Oracle Database**          | [Télécharger Oracle DB](https://www.oracle.com/database/)       | Sélectionnez la version désirée              |
| **Oracle APEX**              | [Télécharger APEX](https://www.oracle.com/tools/downloads/apex-downloads.html) | Téléchargez le fichier ZIP                    |
| **ORDS**                     | [Télécharger ORDS](https://www.oracle.com/tools/downloads/ords-downloads.html) | Téléchargez le fichier ZIP                    |

---

## **3. Installation de la Base de Données Oracle**

### **Étape 3.1 : Préparation de l'Environnement**

1. **Mettre à jour le système :**
   ```bash
   sudo yum update -y  # Pour les distributions basées sur Red Hat/CentOS
   sudo apt update && sudo apt upgrade -y  # Pour les distributions basées sur Debian/Ubuntu
   ```

2. **Installer les dépendances nécessaires :**
   ```bash
   sudo yum install -y libaio libaio-devel unzip zip
   sudo apt install -y libaio1 libaio-dev unzip zip
   ```

### **Étape 3.2 : Installer Oracle Database**

1. **Extraire le fichier téléchargé :**
   ```bash
   unzip LINUX.X64_193000_db_home.zip -d /opt/oracle/
   ```

2. **Créer les répertoires nécessaires :**
   ```bash
   sudo mkdir -p /u01/app/oracle
   sudo chown -R $USER:$USER /u01/app/oracle
   sudo mkdir -p /opt/oracle/product/19c/dbhome_1
   sudo chown -R $USER:$USER /opt/oracle/product/19c/dbhome_1
   ```

3. **Installer Oracle Database :**
   ```bash
   cd /opt/oracle/LINUX.X64_193000_db_home/
   ./runInstaller
   ```
   - **Note :** Suivez les instructions de l'assistant d'installation GUI. Assurez-vous de sélectionner le type d'installation approprié (Typique ou Avancée) et de définir les paramètres tels que le répertoire d'installation, les mots de passe SYS et SYSTEM, etc.

4. **Configurer les variables d'environnement :**
   Ajoutez les lignes suivantes à votre fichier `~/.bashrc` ou `~/.profile` :
   ```bash
   export ORACLE_HOME=/opt/oracle/product/19c/dbhome_1
   export PATH=$PATH:$ORACLE_HOME/bin
   export ORACLE_SID=ORCL
   ```
   Rechargez le fichier de configuration :
   ```bash
   source ~/.bashrc
   ```

5. **Démarrer la base de données :**
   ```bash
   sqlplus / as sysdba
   ```
   Dans SQL*Plus :
   ```sql
   STARTUP;
   EXIT;
   ```

6. **Vérification de l'installation :**
   ```bash
   sqlplus sys/<votre_mot_de_passe> @localhost:1521/ORCL as sysdba
   ```
   Puis, exécutez :
   ```sql
   SELECT name FROM v$database;
   EXIT;
   ```

---

## **4. Installation et Configuration d'Oracle APEX**

### **Étape 4.1 : Extraire APEX**

1. **Télécharger et extraire APEX :**
   ```bash
   unzip apex_21.2.zip -d /opt/oracle/
   ```

### **Étape 4.2 : Installer APEX dans la Base de Données**

1. **Connectez-vous à SQL*Plus en tant que SYSDBA :**
   ```bash
   sqlplus sys/<votre_mot_de_passe>@localhost:1521/ORCL as sysdba
   ```

2. **Exécuter le script d'installation d'APEX :**
   ```sql
   @/opt/oracle/apex/apexins.sql SYSAUX SYSAUX TEMP /i/
   ```
   - **SYSAUX** : Tablespace pour les composants APEX.
   - **TEMP** : Tablespace temporaire.
   - **/i/** : Répertoire virtuel pour les images.

   **Remarque :** L'installation peut prendre plusieurs minutes.

3. **Configurer l'administrateur APEX :**
   Toujours dans SQL*Plus :
   ```sql
   @/opt/oracle/apex/apxchpwd.sql
   ```
   - Suivez les instructions pour définir le nom d'utilisateur (par défaut `ADMIN`) et le mot de passe.

4. **Configurer les images APEX :**
   ```sql
   @/opt/oracle/apex/apxldimg.sql /i/
   ```

5. **Définir le mode de déploiement d'APEX :**
   ```sql
   @/opt/oracle/apex/apex_epg_config.sql <APEX_PUBLIC_USER_password>
   ```
   - Remplacez `<APEX_PUBLIC_USER_password>` par un mot de passe sécurisé.

6. **Quitter SQL*Plus :**
   ```sql
   EXIT;
   ```

### **Étape 4.3 : Vérification de l'Installation d'APEX**

1. **Reconnectez-vous en tant qu'administrateur APEX :**
   Ouvrez votre navigateur et accédez à :
   ```
   http://localhost:8080/ords
   ```
   - Si ORDS n'est pas encore configuré, continuez avec l'installation d'ORDS.

---

## **5. Installation et Configuration d'ORDS en Mode Autonome**

### **Étape 5.1 : Installer Java (JDK)**

1. **Vérifier si Java est déjà installé :**
   ```bash
   java -version
   ```
   - Si Java n'est pas installé ou si la version est inférieure à 8, procédez à l'installation.

2. **Installer JDK (si nécessaire) :**
   - **Télécharger JDK depuis Oracle :**
     Rendez-vous sur [Télécharger JDK](https://www.oracle.com/java/technologies/javase-downloads.html) et téléchargez le fichier tar.gz approprié pour Linux.

   - **Extraire et installer JDK :**
     ```bash
     tar -xzf jdk-17.0.1_linux-x64_bin.tar.gz -C /opt/oracle/
     ```

   - **Configurer les variables d'environnement :**
     Ajoutez les lignes suivantes à votre fichier `~/.bashrc` ou `~/.profile` :
     ```bash
     export JAVA_HOME=/opt/oracle/jdk-17.0.1
     export PATH=$PATH:$JAVA_HOME/bin
     ```
     Rechargez le fichier de configuration :
     ```bash
     source ~/.bashrc
     ```

   - **Vérifier l'installation de Java :**
     ```bash
     java -version
     ```
     Vous devriez voir la version de Java installée.

### **Étape 5.2 : Extraire ORDS**

1. **Télécharger et extraire ORDS :**
   ```bash
   unzip ords-21.x.zip -d /opt/oracle/ords/
   ```

### **Étape 5.3 : Configurer ORDS**

1. **Naviguer vers le répertoire ORDS :**
   ```bash
   cd /opt/oracle/ords
   ```

2. **Exécuter la configuration initiale d'ORDS :**
   ```bash
   java -jar ords.war install
   ```
   - **Remarque :** Pour une configuration avancée, utilisez `install advanced`. Cependant, pour un déploiement autonome, la configuration par défaut suffit généralement.

3. **Suivre les instructions de configuration :**
   - **Base de données hôte** : `localhost` (ou l'adresse IP de votre base de données)
   - **Port** : `1521`
   - **SID** ou **Service Name** : `ORCL`
   - **Utilisateur** : `SYS`
   - **Mot de Passe** : Mot de passe défini lors de l'installation d'Oracle DB
   - **Rôle** : `SYSDBA`

4. **Configurer ORDS pour APEX :**
   - Lorsque vous êtes invité, répondez `y` (oui) pour configurer APEX.
   - Spécifiez le chemin d'installation des images APEX, par exemple `/i/`.

### **Étape 5.4 : Configurer ORDS en Mode Autonome**

1. **Créer un Répertoire de Configuration (optionnel) :**
   ```bash
   mkdir /opt/oracle/ords/config
   ```

2. **Déplacer le Fichier de Configuration :**
   Après la configuration initiale, ORDS génère des fichiers de configuration dans le répertoire courant. Déplacez ces fichiers vers le répertoire de configuration créé.
   ```bash
   mv /opt/oracle/ords/*.xml /opt/oracle/ords/config/
   ```

---

## **6. Lancement d'ORDS en Mode Autonome**

### **Étape 6.1 : Démarrer ORDS**

1. **Naviguer vers le répertoire ORDS :**
   ```bash
   cd /opt/oracle/ords
   ```

2. **Exécuter ORDS en Mode Autonome :**
   ```bash
   java -jar ords.war standalone
   ```
   - ORDS démarrera et écoutera sur le port **8080** par défaut.

3. **Options Optionnelles :**
   - **Changer le Port :**
     ```bash
     java -jar ords.war standalone --port 9090
     ```
     - Utilisera le port `9090` au lieu de `8080`.

   - **Configurer SSL (HTTPS) :**
     - **Générer un Keystore SSL (si ce n'est pas déjà fait) :**
       ```bash
       keytool -genkey -alias ords -keyalg RSA -keystore /opt/oracle/ords/keystore.jks -keysize 2048
       ```
       - Suivez les instructions pour définir le mot de passe et les informations du certificat.

     - **Démarrer ORDS avec SSL :**
       ```bash
       java -jar ords.war standalone --ssl-port 8443 --ssl-keystore /opt/oracle/ords/keystore.jks --ssl-keystore-password <votre_mot_de_passe>
       ```

### **Étape 6.2 : Vérifier le Fonctionnement d'ORDS**

1. **Ouvrir un Navigateur Web :**
   - Accédez à l'URL :
     ```
     http://localhost:8080/ords
     ```
   - Si vous avez changé le port, utilisez le port correspondant (par exemple, `9090`).

2. **Vérifier la Page de Connexion d'Oracle APEX :**
   - Vous devriez voir la page de connexion d'Oracle APEX.

---

## **7. Accès à Oracle APEX via le Navigateur**

### **Étape 7.1 : Connexion à APEX**

1. **Ouvrir votre Navigateur Web :**
   - Allez à :
     ```
     http://localhost:8080/ords
     ```
   - Si vous avez configuré SSL, utilisez :
     ```
     https://localhost:8443/ords
     ```

2. **Se Connecter en tant qu'Administrateur :**
   - Utilisez les identifiants définis lors de l'installation d'APEX (par exemple, `ADMIN` et le mot de passe).

3. **Créer des Workspaces et des Utilisateurs :**
   - Une fois connecté, vous pouvez créer des workspaces pour développer vos applications APEX.

---

## **8. Configuration Avancée et Résolution des Problèmes**

### **Étape 8.1 : Changer le Port d'ORDS**

- **Utiliser un autre port (par exemple, 9090) :**
  ```bash
  java -jar ords.war standalone --port 9090
  ```

### **Étape 8.2 : Configurer SSL (HTTPS)**

1. **Générer un Keystore SSL (si ce n'est pas déjà fait) :**
   ```bash
   keytool -genkey -alias ords -keyalg RSA -keystore /opt/oracle/ords/keystore.jks -keysize 2048
   ```
   - **Remarque :** Suivez les instructions pour définir le mot de passe et les informations du certificat.

2. **Démarrer ORDS avec SSL :**
   ```bash
   java -jar ords.war standalone --ssl-port 8443 --ssl-keystore /opt/oracle/ords/keystore.jks --ssl-keystore-password <votre_mot_de_passe>
   ```

3. **Accéder à APEX via HTTPS :**
   ```
   https://localhost:8443/ords
   ```

### **Étape 8.3 : Optimiser la Mémoire Java pour ORDS**

- **Ajuster les paramètres de mémoire lors du lancement d'ORDS :**
  ```bash
  java -Xms512m -Xmx1024m -jar ords.war standalone
  ```
  - **Remarque :** `-Xms` définit la mémoire minimale et `-Xmx` la mémoire maximale allouée à la JVM.

### **Étape 8.4 : Résolution des Erreurs Courantes**

| **Problème**                         | **Solution**                                                                                                 |
|--------------------------------------|--------------------------------------------------------------------------------------------------------------|
| **Erreur `404 Not Found`**           | - Vérifiez que APEX est correctement installé dans la base de données.<br>- Assurez-vous que ORDS est correctement configuré et en cours d'exécution. |
| **ORDS ne démarre pas**              | - Vérifiez l'installation de Java (`java -version`).<br>- Assurez-vous que les ports nécessaires ne sont pas bloqués ou déjà utilisés.<br>- Consultez les logs d'ORDS pour plus de détails. |
| **Problèmes de connexion à la DB**   | - Vérifiez les paramètres de connexion (hôte, port, SID/service name).<br>- Assurez-vous que le listener Oracle est actif (`lsnrctl status`). |
| **Performance lente**                | - Augmentez les paramètres de mémoire Java (`-Xms` et `-Xmx`).<br>- Optimisez la configuration de la base de données Oracle (indices, paramètres de performance). |
| **Accès refusé à l'interface de gestion** | - Vérifiez les configurations d'ORDS.<br>- Assurez-vous que les utilisateurs et rôles sont correctement définis dans `tomcat-users.xml` si applicable. |

---

## **9. Résumé des Étapes**

| **Étape**                           | **Description**                                                                                             |
|-------------------------------------|-------------------------------------------------------------------------------------------------------------|
| **1. Prérequis**                    | Vérifiez les composants nécessaires (JDK, Oracle DB, APEX, ORDS) et préparez l'environnement Linux.           |
| **2. Téléchargements**              | Téléchargez JDK, Oracle Database, APEX et ORDS depuis les sites officiels d'Oracle.                          |
| **3. Installation de la DB Oracle** | Installez Oracle Database, configurez le listener, définissez les variables d'environnement et vérifiez l'installation via SQL*Plus. |
| **4. Installation d'APEX**          | Extrayez APEX, installez-le dans la base de données, configurez l'administrateur et les images APEX.           |
| **5. Installation d'ORDS**          | Installez Java, extrayez ORDS, configurez ORDS pour la base de données et APEX en mode autonome.               |
| **6. Lancement d'ORDS**             | Démarrez ORDS en mode autonome et vérifiez son fonctionnement via le navigateur.                             |
| **7. Accès à APEX**                  | Connectez-vous à Oracle APEX via l'URL fournie et commencez à développer vos applications.                      |
| **8. Configuration Avancée**        | Changez les ports, configurez SSL, optimisez la mémoire Java et résolvez les problèmes courants.                |

---

