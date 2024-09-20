# Configuration et installation oracle db, oracle apex, rods
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

Avant de commencer, assurez-vous d'avoir les éléments suivants installés et configurés sur votre machine Windows :

- **Java Development Kit (JDK) 8 ou ultérieur**
- **Oracle Database** (versions 11g, 12c, 18c, 19c, etc.)
- **Oracle APEX** (la version souhaitée)
- **Oracle REST Data Services (ORDS)**
- **SQL Developer** (optionnel, pour faciliter l'exécution des scripts SQL)
- **Ports ouverts** : Assurez-vous que les ports **1521** (Oracle DB) et **8080** (ORDS) sont ouverts.

---

## **2. Téléchargements**

Téléchargez les composants nécessaires depuis les sites officiels d'Oracle :

| Composant                | Lien de Téléchargement                                      | Remarques                                |
|--------------------------|-------------------------------------------------------------|------------------------------------------|
| **Java Development Kit**| [Télécharger JDK](https://www.oracle.com/java/technologies/javase-downloads.html) | Téléchargez la version appropriée pour Windows |
| **Oracle Database**      | [Télécharger Oracle DB](https://www.oracle.com/database/)   | Sélectionnez la version désirée          |
| **Oracle APEX**          | [Télécharger APEX](https://www.oracle.com/tools/downloads/apex-downloads.html) | Téléchargez le fichier ZIP                |
| **ORDS**                 | [Télécharger ORDS](https://www.oracle.com/tools/downloads/ords-downloads.html) | Téléchargez le fichier ZIP                |

---

## **3. Installation de la Base de Données Oracle**

### **Étape 3.1 : Installer Oracle Database**

1. **Extraire et Installer :**
   - Extrayez le fichier téléchargé (par exemple, `winx64_19c_database.zip`) dans un répertoire, par exemple `C:\Oracle\Database\`.
   - Exécutez le fichier `setup.exe` pour lancer l'installation.
   - Suivez les instructions de l'assistant d'installation :
     - **Type d'installation** : Typique
     - **Nom de l'Instance** : Par défaut (ex. `ORCL`)
     - **Mots de Passe** : Définissez le mot de passe pour l'utilisateur `SYS` et `SYSTEM`.

2. **Configuration du Réseau :**
   - Assurez-vous que le **listener** Oracle est configuré pour écouter sur le port **1521**.
   - Vérifiez le fichier `listener.ora` situé dans `C:\Oracle\Network\ADMIN\`.

3. **Vérification de l'Installation :**
   - Ouvrez **SQL*Plus** et connectez-vous en tant que `SYSDBA` pour vérifier que la base de données fonctionne correctement.
     ```bash
     sqlplus / as sysdba
     ```
     Puis, exécutez :
     ```sql
     SELECT name FROM v$database;
     ```

---

## **4. Installation et Configuration d'Oracle APEX**

### **Étape 4.1 : Extraire APEX**

1. Extrayez le fichier ZIP d'APEX téléchargé dans un répertoire, par exemple `C:\Oracle\APEX\`.

### **Étape 4.2 : Installer APEX dans la Base de Données**

1. **Ouvrir une Invite de Commandes :**
   - Appuyez sur `Win + R`, tapez `cmd` et appuyez sur `Entrée`.

2. **Naviguer vers le Répertoire APEX :**
   ```bash
   cd C:\Oracle\APEX
   ```

3. **Se Connecter à SQL*Plus en tant que SYSDBA :**
   ```bash
   sqlplus sys/VOTRE_MOT_DE_PASSE@localhost:1521/ORCL as sysdba
   ```

4. **Exécuter le Script d'Installation d'APEX :**
   ```sql
   @apexins.sql SYSAUX SYSAUX TEMP /i/
   ```
   - **SYSAUX** : Tablespace pour les composants APEX.
   - **TEMP** : Tablespace temporaire.

   **Remarque :** L'installation peut prendre plusieurs minutes.

### **Étape 4.3 : Configurer l'Administrateur APEX**

1. **Exécuter le Script de Configuration de l'Administrateur :**
   ```sql
   @apxchpwd.sql
   ```
2. **Suivre les Instructions :**
   - Définissez le nom d'utilisateur (par défaut `ADMIN`) et le mot de passe.

### **Étape 4.4 : Configurer les Images d'APEX**

1. **Exécuter le Script pour Charger les Images :**
   ```sql
   @apxldimg.sql C:\Oracle\APEX\images
   ```

---

## **5. Installation et Configuration d'ORDS en Mode Autonome**

### **Étape 5.1 : Installer Java (JDK)**

1. **Extraire et Installer JDK :**
   - Suivez les instructions du programme d'installation de JDK téléchargé.
   - Par défaut, JDK est installé dans `C:\Program Files\Java\jdk-<version>`.

2. **Configurer les Variables d'Environnement :**
   - **JAVA_HOME** :
     - Allez dans **Panneau de configuration** > **Système** > **Paramètres système avancés** > **Variables d'environnement**.
     - Cliquez sur **Nouvelle** sous **Variables système**.
       - **Nom** : `JAVA_HOME`
       - **Valeur** : `C:\Program Files\Java\jdk-<version>`
   - **Path** :
     - Toujours dans **Variables d'environnement**, trouvez la variable `Path`, cliquez sur **Modifier**, puis **Nouveau** et ajoutez `%JAVA_HOME%\bin`.

3. **Vérifier l'Installation de Java :**
   - Ouvrez **Invite de Commandes** et tapez :
     ```bash
     java -version
     ```
     - Vous devriez voir la version de Java installée.

### **Étape 5.2 : Extraire ORDS**

1. Extrayez le fichier ZIP d'ORDS téléchargé dans un répertoire, par exemple `C:\Oracle\ORDS\`.

### **Étape 5.3 : Configurer ORDS**

1. **Ouvrir une Invite de Commandes :**
   - Naviguez vers le répertoire ORDS :
     ```bash
     cd C:\Oracle\ORDS
     ```

2. **Exécuter la Configuration Initiale d'ORDS :**
   ```bash
   java -jar ords.war setup
   ```
   - **Remplissez les informations demandées** :
     - **Base de données hôte** : `localhost`
     - **Port** : `1521`
     - **SID** ou **Service Name** : `ORCL`
     - **Utilisateur** : `SYS`
     - **Mot de Passe** : Mot de passe défini lors de l'installation d'Oracle DB
     - **Rôle** : `SYSDBA`

3. **Configurer ORDS pour APEX :**
   - Lorsque vous êtes invité à configurer APEX, répondez `y` (oui).
   - Spécifiez le chemin d'installation des images APEX, par exemple `/i/`.

### **Étape 5.4 : Configurer ORDS en Mode Autonome**

1. **Créer un Répertoire de Configuration :**
   - Par exemple, `C:\Oracle\ORDS\config`.

2. **Déplacer le Fichier de Configuration :**
   - Après la configuration initiale, ORDS génère des fichiers de configuration dans le répertoire courant.
   - Déplacez ces fichiers vers le répertoire de configuration créé.

3. **Démarrer ORDS en Mode Autonome :**
   ```bash
   java -jar ords.war standalone
   ```
   - **Options Optionnelles** :
     - **Changer le Port** :
       ```bash
       java -jar ords.war standalone --port 9090
       ```
     - **Configurer SSL** :
       ```bash
       java -jar ords.war standalone --ssl-port 8443 --ssl-keystore "C:\chemin\vers\keystore.jks" --ssl-keystore-password "votre_mot_de_passe"
       ```

---

## **6. Lancement d'ORDS en Mode Autonome**

### **Étape 6.1 : Démarrer ORDS**

1. **Ouvrir une Invite de Commandes :**
   - Naviguez vers le répertoire ORDS :
     ```bash
     cd C:\Oracle\ORDS
     ```

2. **Exécuter ORDS en Mode Autonome :**
   ```bash
   java -jar ords.war standalone
   ```
   - ORDS démarrera et écoutera sur le port **8080** par défaut.

### **Étape 6.2 : Vérifier le Fonctionnement d'ORDS**

1. **Ouvrir un Navigateur Web :**
   - Accédez à l'URL :
     ```
     http://localhost:8080/ords
     ```
   - Vous devriez voir la page de connexion d'Oracle APEX.

---

## **7. Accès à Oracle APEX via le Navigateur**

### **Étape 7.1 : Connexion à APEX**

1. **Ouvrir votre Navigateur Web :**
   - Allez à :
     ```
     http://localhost:8080/ords
     ```
2. **Se Connecter en tant qu'Administrateur :**
   - Utilisez les identifiants définis lors de l'installation d'APEX (par exemple, `ADMIN` et le mot de passe).

3. **Créer des Workspaces et des Utilisateurs :**
   - Une fois connecté, vous pouvez créer des workspaces pour développer vos applications APEX.

---

## **8. Configuration Avancée et Résolution des Problèmes**

### **Étape 8.1 : Changer le Port d'ORDS**

- Pour utiliser un port différent (par exemple, **9090**) :
  ```bash
  java -jar ords.war standalone --port 9090
  ```

### **Étape 8.2 : Configurer SSL (HTTPS)**

1. **Générer un Keystore SSL** (si ce n'est pas déjà fait) :
   ```bash
   keytool -genkey -alias tomcat -keyalg RSA -keystore C:\chemin\vers\keystore.jks -keysize 2048
   ```
2. **Démarrer ORDS avec SSL :**
   ```bash
   java -jar ords.war standalone --ssl-port 8443 --ssl-keystore "C:\chemin\vers\keystore.jks" --ssl-keystore-password "votre_mot_de_passe"
   ```
3. **Accéder à APEX via HTTPS :**
   ```
   https://localhost:8443/ords
   ```

### **Étape 8.3 : Optimiser la Mémoire Java pour ORDS**

- Ajustez les paramètres de mémoire lors du lancement d'ORDS :
  ```bash
  java -Xms512m -Xmx1024m -jar ords.war standalone
  ```

### **Étape 8.4 : Résolution des Erreurs Courantes**

| Problème                             | Solution                                                                                          |
|--------------------------------------|---------------------------------------------------------------------------------------------------|
| **Erreur `404 Not Found`**           | - Vérifiez l'installation d'APEX.<br>- Assurez-vous que ORDS est correctement configuré.            |
| **ORDS ne démarre pas**              | - Vérifiez l'installation de Java.<br>- Assurez-vous que les ports sont libres.                    |
| **Problèmes de connexion à la DB**   | - Vérifiez les paramètres de connexion (host, port, SID/service name).<br>- Assurez-vous que le listener est actif. |
| **Performance lente**                | - Augmentez les paramètres de mémoire Java.<br>- Optimisez la configuration de la base de données. |

---

## **9. Résumé des Étapes**

| **Étape**                           | **Description**                                                                                         |
|-------------------------------------|---------------------------------------------------------------------------------------------------------|
| **1. Prérequis**                    | Vérifiez les composants nécessaires (JDK, Oracle DB, APEX, ORDS).                                       |
| **2. Téléchargements**              | Téléchargez JDK, Oracle Database, APEX et ORDS depuis les sites officiels d'Oracle.                    |
| **3. Installation de la DB Oracle** | Installez Oracle Database, configurez le listener et vérifiez l'installation via SQL*Plus.               |
| **4. Installation d'APEX**          | Extrayez APEX, installez-le dans la base de données, configurez l'administrateur et les images APEX.     |
| **5. Installation d'ORDS**          | Installez Java, extrayez ORDS, configurez ORDS pour la base de données et APEX en mode autonome.         |
| **6. Lancement d'ORDS**             | Démarrez ORDS en mode autonome et vérifiez son fonctionnement via le navigateur.                        |
| **7. Accès à APEX**                  | Connectez-vous à Oracle APEX via l'URL fournie et commencez à développer vos applications.                |
| **8. Configuration Avancée**        | Changez les ports, configurez SSL, optimisez la mémoire Java et résolvez les problèmes courants.          |

---

## **Conclusion**

Vous avez maintenant une installation complète et fonctionnelle d'**Oracle APEX** avec **Oracle Database** et **ORDS** en mode autonome sur **Windows**. Cette configuration est idéale pour le développement et les environnements de test. Pour les déploiements en production, considérez des configurations plus robustes avec des serveurs d'applications dédiés et des mesures de sécurité supplémentaires.

