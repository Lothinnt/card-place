# AGENTS.md — contexte pour les agents de code

## Ce qu'est ce dépôt

Card Place (The first secure card exchange) : registre on-chain de cartes gradées conservées en coffre, sur Stellar.
- `contracts/vault/` — contrat Soroban (Rust, `soroban-sdk` 28). Seule brique vérifiée on-chain.
- `apps/agent/` — agent IA d'expertise (Node 22+, ESM) vendu à l'appel via x402 sur Stellar.
- `demo-web-page/` — page de la carte (marketplace), HTML/CSS/JS statique, lit l'agent et Horizon.
- `docs/` — photos de référence des cartes (recto/verso), nommées `<gradeur>-<cert>-front.jpg`.

## Règles de la chaîne d'outils (ne pas régresser)

- Cible de compilation : `wasm32v1-none`, jamais `wasm32-unknown-unknown`.
- Binaire : `stellar` (pas `soroban`). Crate : `soroban-sdk` (pas `stellar-sdk`).
- Tests : `env.register(Contract, (args,))`, pas `register_contract`.
- Un appel qui doit atteindre le ledger passe `--send=yes`. Sans ce flag, il est seulement simulé.
- Arguments `Symbol` numériques en CLI : les entourer de guillemets JSON, ex. `--cert '"137798077"'`.

## Commandes

```sh
# contrat
cargo test                       # 11 tests, dont le cycle complet avec un SAC de test
stellar contract build           # → target/wasm32v1-none/release/card_vault.wasm

# agent
cd apps/agent && npm install
npm run check                    # syntaxe
node scripts/x402-smoke.mjs      # paiement x402 réel sur testnet (sans IA)
npm start                        # serveur sur :3000

# page web (depuis la racine du dépôt)
node demo-web-page/serve.mjs          # http://localhost:8080/demo-web-page/ — lit l'agent (:3000) et Horizon
sh demo-web-page/seed-market.sh       # ordres de vente/achat sur le DEX pour remplir le carnet
```

`demo-web-page/` est du HTML/CSS/JS sans framework ni build : garder ainsi. Données de démo dans `demo-web-page/data/zekrom.json`.

## Invariants du contrat

- Une carte = une clé `Card(asset_code)` en stockage persistant, TTL prolongé à chaque écriture.
- Transitions strictes : `InVault → RedeemRequested → Shipped`. Toute autre transition renvoie `InvalidStatus`.
- `register_card` et `mark_shipped` : auth du dépositaire. `redeem` : auth du détenteur, qui autorise aussi le transfert SAC.
- `redeem` transfère exactement `ONE_CARD` (1 unité, 7 décimales). Dépositaire = émetteur ⇒ le SAC brûle.

## Ce que l'agent IA ne fait jamais

Il ne détient aucune clé Stellar, ne signe aucune transaction, n'affirme rien qui ne soit sur la photo ou dans la fiche on-chain. Il propose ; le détenteur et le dépositaire disposent.

## Variables d'environnement

Voir `apps/agent/.env.example`. Ne jamais committer `.env`.
