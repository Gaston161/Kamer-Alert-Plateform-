# Traffic-flow-yaounde-


​À Yaoundé, se déplacer est un défi quotidien marqué par l'imprévisibilité. Les embouteillages monstres à des points névralgiques comme le "Rond-point Nlongkak" ou la "Poste Centrale" peuvent doubler ou tripler un temps de trajet sans préavis. Cette incertitude n'est pas seulement une perte de temps ; elle a un coût économique réel. Le problème fondamental n'est pas tant le nombre de véhicules que le manque total d'information en temps réel. Nous proposons une solution basée sur le cloud pour créer un système nerveux central pour le trafic de la ville.

Trafik-Flow une Plateforme Cloud de Crowdsourcing pour la Modélisation du Trafic en Temps Réel à Yaoundé.
​Description du Service
​"Trafik-Flow" n'est pas une autre application de VTC (comme Yango ou Uber). C'est un service de données intelligent qui transforme des milliers de transporteurs informels (motos-taxis et taxis) en capteurs mobiles pour cartographier le trafic de la ville.
​Voici comment ça marche :
​Les Collecteurs de Données (Motos-taxis et Taxis) : Nous offrons une application légère et gratuite aux conducteurs volontaires. En échange de l'activation de leur GPS, qui partage anonymement leur vitesse et leur position, ils reçoivent de petits avantages (par exemple, des alertes sur les accidents ou des zones de contrôle policier signalées par d'autres conducteurs, ou même des micro-paiements sous forme de crédit de communication).
​La Plateforme (Le Cerveau) : Notre système dans le cloud collecte et traite ce flux massif de données GPS. En agrégeant la vitesse de centaines de véhicules sur un tronçon de route, nos algorithmes peuvent déterminer avec une grande précision si le trafic est fluide (vert), ralenti (orange) ou congestionné (rouge).
​Les Utilisateurs (Toute la population) : Via une application gratuite ou un simple site web, n'importe qui peut consulter une carte de Yaoundé affichant l'état du trafic en temps réel. Un employé de bureau peut décider de quitter le travail 30 minutes plus tard pour éviter un blocage. Un bus peut adapter son itinéraire. Un service d'ambulance peut trouver le chemin le plus rapide.
​Justification Technique (Scalabilité, Tolérance aux Pannes, Collaboration)
​1. Scalability (Gérer un Déluge de Données)
​Le système doit être capable de recevoir et traiter les coordonnées GPS de, disons, 5 000 motos-taxis envoyant une mise à jour toutes les 5 secondes. Cela représente 1 000 points de données par seconde. C'est un volume énorme.
​Analyse et Justification :
​Comparaison : Un système monolithique serait comme un seul agent de la circulation essayant de gérer tout un carrefour à la main pendant l'heure de pointe. Il serait instantanément submergé. Notre architecture en microservices est comme une tour de contrôle aérien avec des contrôleurs spécialisés pour chaque secteur. Un service est dédié uniquement à la réception des données (l'ingestion), un autre au traitement (calculer les vitesses moyennes), et un troisième à la distribution (afficher la carte).
​Solution Cloud : Nous utilisons un service de streaming de données comme AWS Kinesis ou Google Cloud Pub/Sub. C'est comme une autoroute à plusieurs voies conçue pour recevoir un flux ininterrompu de petites voitures (les données GPS) sans jamais créer d'embouteillage. Ces données sont ensuite traitées par des fonctions "serverless" (AWS Lambda) qui s'activent et se multiplient automatiquement en fonction de la charge, garantissant que le traitement ne prend jamais de retard.
​2. Fault Tolerance (Ne Jamais Tomber en Panne)
​La carte du trafic doit être disponible 24/7. Une panne de serveur ne peut pas rendre la carte "aveugle".
​Analyse et Justification :
​Comparaison : Le corps humain ne dépend pas d'un seul poumon ou d'un seul rein. Il en a deux pour la redondance. Si l'un a un problème, l'autre peut compenser. De même, un avion a plusieurs moteurs.
​Solution Cloud : Notre infrastructure est entièrement redondante. Chaque microservice est dupliqué sur plusieurs serveurs dans des centres de données physiquement séparés (Availability Zones). Un Load Balancer agit comme un répartiteur intelligent ; si un serveur ne répond pas, il cesse instantanément de lui envoyer du trafic et le redirige vers les serveurs sains. De plus, l'application du conducteur est conçue pour être résiliente aux pannes de réseau. Si un moto-taxi perd sa connexion 3G, l'application stocke les points GPS localement et les envoie en bloc dès que la connexion est rétablie.
​3. Collaboration (L'Intelligence Collective)
​Le cœur du projet est une collaboration massive, bien que passive. Chaque conducteur, sans même y penser, contribue à une vision globale qui profite à toute la communauté.
​Analyse et Justification :

​Solution Cloud : La collaboration est orchestrée par des APIs claires. L'application du conducteur utilise une API pour "pousser" les données de localisation. L'application de l'utilisateur final utilise une autre API pour "tirer" les données de la carte. Pour que la carte se mette à jour en direct sous les yeux de l'utilisateur, nous utilisons des technologies comme les WebSockets. C'est un canal de communication direct et permanent entre le serveur et l'application de l'utilisateur, permettant au serveur d'envoyer de nouvelles informations sur le trafic dès qu'elles sont calculées, créant une expérience fluide et en temps réel.
