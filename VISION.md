# Card Place — Vision

Je vends des cartes à collectionner (GradedCardShop). Ce marché souffre de trois maux : l'opacité des prix (pas de référence publique, ventes privées, historiques invisibles), l'état réel de la carte (photos du vendeur, notation subjective, litiges) et la contrefaçon (cartes et boîtiers falsifiés). S'y ajoute le coût de chaque revente : transport, commission de 5 à 12 %, jours d'attente.

Card Place vise toutes les cartes, mais attaque d'abord les cartes gradées : déjà authentifiées et notées par un tiers, avec un certificat unique. C'est le marché le plus facile à sécuriser.

La carte reste en coffre et devient un actif Stellar natif : code = certificat, 1 unité. Elle se revend en 5 secondes sur le carnet d'ordres natif, où chaque vente est une transaction publique : le prix de référence et l'historique deviennent vérifiables par tous. Le physique ne voyage qu'une fois, à la sortie finale.

Ce que le contrat Soroban fait déjà (testnet, déployé et appelé) : il lie le jeton au certificat PSA et au hash SHA-256 de la photo, tient l'état physique (en coffre, sortie demandée, expédiée) et expose redeem(), qui récupère le jeton via le Stellar Asset Contract. Le détenteur signe sa sortie, le dépositaire signe l'expédition. Une vraie carte de mon stock, PSA 10 n° 137798077, a fait le cycle complet on-chain.

Autour : un agent IA qui relit l'étiquette, vérifie l'empreinte de la photo contre le contrat et évalue l'état ; une API x402 sur Stellar qui vend cette expertise 0,01 USDC l'appel ; un wallet d'agent plafonné côté client.

Pourquoi Stellar : actif, marché et trustline sont dans le protocole ; les frais permettent une carte à 10 € ; x402 fait payer les agents dans la même monnaie que les collectionneurs.

À HackMeridian : la marketplace data-driven (fiche d'identité, prix de référence offre vs demande, historique des ventes, indice de confiance), la démo avec plusieurs cartes réelles, et l'agent qui achète dans sa limite de dépense. Catégorie Genesis.
