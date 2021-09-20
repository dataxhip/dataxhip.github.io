# Définition du modèle DevOps

DevOps est une combinaison de philosophies culturelles, de pratiques et d'outils qui améliore la capacité d'une entreprise à livrer des applications et des services à un rythme élevé. Il permet de faire évoluer et d’optimiser les produits plus rapidement que les entreprises utilisant des processus traditionnels de développement de logiciels et de gestion de l’infrastructure. Cette vitesse permet aux entreprises de mieux servir leurs clients et de gagner en compétitivité.


# Fonctionnement de DevOps

Dans un modèle DevOps, les équipes de développement et d'opérations ne sont plus isolées. Il arrive qu'elles soient fusionnées en une seule et même équipe. Les ingénieurs qui la composent travaillent alors sur tout le cycle de vie d'une application, de la création à l’exploitation, en passant par les tests et le déploiement, et développent toute une gamme de compétences liées à différentes fonctions.


# Importance de DevOps
Les logiciels et Internet ont transformé le monde et les secteurs d'activité, du commerce au divertissement en passant par les banques. Les logiciels ne se contentent plus de soutenir les entreprises : ils sont aujourd'hui un composant essentiel de leurs activités. Les entreprises interagissent avec leurs clients à travers des logiciels livrés en tant que services ou applications en ligne et sur toutes sortes d'appareils. Elles peuvent également utiliser les logiciels pour gagner en efficacité opérationnelle en transformant chaque aspect de la chaîne de valeurs, comme la logistique, les communications et les opérations. Autant les entreprises spécialisées dans les biens physiques ont transformé leurs méthodes de conception, de création et de livraison de produits à l'aide de l'automatisation industrielle tout au long du 20e siècle, autant les sociétés modernes doivent adapter leur façon de créer et de livrer les logiciels.



# La philosophie culturelle DevOps
La transition vers DevOps implique un changement de culture et d'état d'esprit. Pour simplifier, le DevOps consiste à éliminer les obstacles entre deux équipes traditionnellement isolées l'une de l'autre : l'équipe de développement et l'équipe d'opérations. Certaines entreprises vont même jusqu'à ne pas avoir d'équipes de développement et d'opérations distinctes, mais des ingénieurs assurant les deux rôles à la fois. Avec DevOps, les deux équipes travaillent en collaboration pour optimiser la productivité des développeurs et la fiabilité des opérations. Elles ont à cœur de communiquer fréquemment, de gagner en efficacité et d’améliorer la qualité des services offerts aux clients. Elles assument l'entière responsabilité de leurs services, et vont généralement au-delà des rôles ou postes traditionnellement définis en pensant aux besoins de l'utilisateur final et à comment les satisfaire. Les équipes d'assurance qualité et de sécurité peuvent également s'intégrer étroitement aux équipes de développement et d’opérations. Quelle que soit leur structure organisationnelle, les organisations adoptant un modèle DevOps disposent d’équipes qui considèrent l’ensemble du cycle de développement et d'infrastructure comme faisant partie de leurs responsabilités.


Pratiques DevOps
Liste des bonnes pratiques DevOps :  

- Intégration continue
- Livraison continue
- Microservices
- Infrastructure en tant que code
- Surveillance et journalisation
- Communication et collaboration


## Intégration continue
L'intégration continue est une méthode de développement de logiciel dans laquelle les développeurs intègrent régulièrement leurs modifications de code à un référentiel centralisé, suite à quoi des opérations de création et de test sont automatiquement menées. Les principaux objectifs de l'intégration continue sont : trouver et corriger plus rapidement les bogues, améliorer la qualité des logiciels et réduire le temps nécessaire pour valider et publier de nouvelles mises à jour de logiciels.


## Livraison continue
La livraison continue est une méthode de développement de logiciels dans laquelle les changements de code sont automatiquement générés, testés et préparés pour une publication dans un environnement de production. Cette pratique étend le principe de l'intégration continue en déployant tous les changements de code dans un environnement de test et/ou un environnement de production après l'étape de création. Une bonne livraison continue permet aux développeurs de toujours disposer d'un artéfact prêt au déploiement ayant suivi un processus de test normalisé.

## Microservices
L'architecture de microservices est une approche de conception qui consiste à diviser une application en un ensemble de petits services. Chaque service est exécuté par son propre processus et communique avec les autres services par le biais d'une interface bien définie et à l'aide d'un mécanisme léger, typiquement une interface de programmation d'application (API) HTTP. Les microservices sont conçus autour de capacités métier ; chaque service est dédié à une seule fonction. Vous pouvez utiliser différents frameworks ou langages de programmation pour écrire des microservices et les déployer indépendamment, en tant que service unique ou en tant que groupe de services.


## Infrastructure en tant que code
L'infrastructure en tant que code est une pratique qui implique la mise en service et la gestion de l'infrastructure à l'aide de code et de techniques de développement de logiciels, notamment le contrôle des versions et l'intégration continue. Le modèle de cloud axé sur les API permet aux développeurs et aux administrateurs système d'interagir avec l'infrastructure de manière programmatique et à n'importe quelle échelle, au lieu de devoir installer et configurer manuellement chaque ressource. Ainsi, les ingénieurs peuvent créer une interface avec l'infrastructure à l'aide d'outils de code et traiter l'infrastructure de la même manière qu'un code d'application. Puisqu'ils sont définis par du code, l'infrastructure et les serveurs peuvent être rapidement déployés à l'aide de modèles standardisés, mis à jour avec les derniers correctifs et les dernières versions ou dupliqués de manière répétable.

## Gestion de la configuration
Les développeurs et les administrateurs système utilisent du code pour automatiser la configuration du système d'exploitation et de l'hôte, les tâches opérationnelles et bien plus encore. Le recours au code permet de rendre les changements de configuration répétables et standardisés. Les développeurs et les administrateurs système ne sont donc plus tenus de configurer manuellement les systèmes d'exploitation, les applications système ou les logiciels de serveurs.

## Politique en tant que code
Une fois l'infrastructure et sa configuration codifiées dans le cloud, les entreprises peuvent surveiller et exécuter la conformité de manière dynamique et à n'importe quelle échelle. L'infrastructure décrite par du code peut ainsi être suivie, validée et reconfigurée automatiquement. Les entreprises peuvent dès lors gérer plus facilement les changements de ressources et s'assurer que les mesures de sécurité sont appliquées de façon appropriée et distribuée (par exemple, la sécurité des informations ou la conformité aux normes PCI-DSS ou HIPAA). Cela permet aux équipes au sein d'une organisation d'avancer plus rapidement, car les ressources non conformes peuvent être automatiquement signalées pour être examinées plus en profondeur, voire même ramenées automatiquement à un état de conformité.


## Surveillance et journalisation
Surveillance et journalisation
Les entreprises surveillent les métriques et les journaux pour découvrir l'impact des performances de l'application et de l'infrastructure sur l'expérience de l'utilisateur final du produit. En capturant, catégorisant et analysant les données et les journaux générés par les applications et l'infrastructure, les organisations comprennent l'effet des modifications ou des mises à jour sur les utilisateurs, afin d'identifier les véritables causes de problèmes ou de changements imprévus. La surveillance active est de plus en plus importante, car les services doivent aujourd'hui être disponibles 24 h/24 et 7 j/ 7, et la fréquence des mises à jour d'infrastructure augmente sans cesse. La création d'alertes et l'analyse en temps réel de ces données aident également les entreprises à surveiller leurs services de manière plus proactive.



## Communication et collaboration
Communication et collaboration
L'instauration d'une meilleure collaboration et d’une meilleure communication au sein de l'organisation est un des principaux aspects culturels de DevOps. Le recours aux outils DevOps et l'automatisation du processus de livraison des logiciels établit la collaboration en rapprochant physiquement les flux de travail et les responsabilités des équipes de développement et d’opérations. Partant de cela, ces équipes instaurent des normes culturelles fortes autour du partage des informations et de la facilitation des communications, et ce par le biais d'applications de messagerie, de systèmes de suivi des problèmes ou des projets et de wikis. Cela permet d'accélérer les communications entre les équipes de développement et d’opérations et même d'autres services, par exemple le marketing et les ventes, afin d'aligner plus étroitement chaque composant de l'organisation sur des objectifs et des projets communs.

Référence : https://aws.amazon.com/fr/devops/what-is-devops/
