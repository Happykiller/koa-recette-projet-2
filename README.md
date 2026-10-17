# Projet de recette KoA — « Liste de courses partagée »

Dépôt jetable du plan 112 de KoA : une demande plus longue que la première, livrée par une équipe
d'agents qu'un orchestrateur ouvre lui-même.

## La demande (du demandeur, à l'orchestrateur)

Une **liste de courses partagée** entre plusieurs navigateurs :

1. **Une API** HTTP en Node (module `http` seul, sans dépendance) : `GET /api/articles`,
   `POST /api/articles` (`{ "nom": "…" }`), `PATCH /api/articles/:id` (cocher, décocher),
   `DELETE /api/articles/:id`. Les articles sont gardés dans un fichier JSON (`DATA_DIR`), et
   survivent au redémarrage du serveur. Un nom vide ou de plus de 100 caractères est refusé (400).
2. **Une page web** qui utilise cette API : ajouter, cocher, supprimer ; elle se met à jour toutes les
   cinq secondes pour voir les changements d'un autre navigateur.
3. **Des tests** lancés par `npm test` (`node --test`, sans dépendance) : la logique des articles,
   et l'API elle-même, démarrée sur un port libre pendant les tests.
4. **Deux conteneurs** et un `compose.yaml` : `api` (Node) et `web` (nginx qui sert la page et
   renvoie `/api/` vers `api`). `docker compose up -d --build` puis `curl http://localhost:8090/` (200)
   et `curl http://localhost:8090/api/articles` (200, JSON).
5. Un `README` qui dit comment lancer et tester, et ce qui a été décidé.

**Fini quand** : `npm test` passe, `docker compose up -d --build` démarre les deux services, les deux
`curl` répondent 200, un article créé par l'API est visible après un redémarrage du conteneur `api`,
et `main` porte le tout. Budget : une heure d'équipe.

## Ce qui vaut pour tout le projet

La base de connaissance du projet est dans [`docs/KB/`](docs/KB/README.md). Ce qu'un agent apprend et
qui vaut pour les autres s'y écrit, pas dans sa mémoire.
