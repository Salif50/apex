Utiliser **ORDS (Oracle REST Data Services)** comme seul serveur en production présente plusieurs limites et inconvénients, en particulier pour des applications à grande échelle. Voici un aperçu des principaux points à considérer :

### **1. Limites de Scalabilité**
- **Gestion de la Charge** : ORDS en mode autonome ne gère pas efficacement le load balancing. Pour des applications avec un trafic élevé, cela peut entraîner des goulets d'étranglement et des temps de réponse plus lents.
- **Évolutivité Verticale** : ORDS peut rencontrer des limitations en termes de montée en charge, surtout si plusieurs utilisateurs tentent d'accéder simultanément à des ressources intensives.

### **2. Haute Disponibilité**
- **Single Point of Failure** : En déployant ORDS seul, vous introduisez un point de défaillance unique. Si ORDS tombe en panne, toute l'application devient inaccessible.
- **Clustering Limité** : ORDS ne prend pas en charge le clustering natif, ce qui complique la mise en place d'une architecture haute disponibilité.

### **3. Performances**
- **Optimisation des Performances** : ORDS peut manquer de certaines optimisations avancées que l'on trouve dans des serveurs d'applications complets (comme Tomcat ou WebLogic), telles que la gestion des sessions ou la mise en cache avancée.
- **Gestions des Sessions** : ORDS ne gère pas les sessions utilisateur de manière aussi robuste que les serveurs d'applications, ce qui peut poser problème dans des scénarios d'utilisation intensive.

### **4. Fonctionnalités Manquantes**
- **Gestion des Transactions** : ORDS a des fonctionnalités limitées pour la gestion des transactions complexes, ce qui peut être nécessaire pour des applications critiques.
- **Sécurité Avancée** : Les fonctionnalités de sécurité sont plus limitées par rapport aux serveurs d'applications complets, qui offrent des mécanismes plus robustes pour l'authentification, l'autorisation, et la sécurité des applications.

### **5. Complexité de Configuration**
- **Mises à jour et Maintenance** : La gestion des mises à jour d'ORDS peut être plus complexe si elle est la seule composante du système, nécessitant une attention particulière pour éviter les interruptions de service.
- **Intégration avec d'autres Services** : L'intégration avec des services tiers ou des architectures complexes peut être plus difficile sans l'infrastructure et les fonctionnalités d'un serveur d'applications complet.

### **6. Support et Documentation**
- **Documentation et Ressources** : Bien qu'ORDS ait une documentation, les ressources communautaires et les guides peuvent être moins nombreux que pour des serveurs d'applications plus populaires comme Tomcat ou WebLogic.
- **Support Technique** : En cas de problème, le support technique peut être limité si vous ne disposez pas d'une architecture serveur d'applications avec un soutien professionnel.

### **Conclusion**
En résumé, bien que **ORDS** soit une solution efficace pour le développement et le déploiement d'applications RESTful avec Oracle APEX, il présente des limites significatives pour un déploiement en production à grande échelle. Pour des applications critiques, il est fortement recommandé d'utiliser ORDS en combinaison avec un serveur d'applications robuste pour garantir la scalabilité, la haute disponibilité, et la performance.
