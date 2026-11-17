
# Compte rendu TP PetitPanier

## Palier 7 — Automatiser

### 7.1 — Pipeline CI/CD

L'objectif est d'automatiser les étapes de construction, de vérification et de déploiement de l'application PetitPanier.

| Étape | Commande reprise du TP | Si cette étape échoue |
|---|---|---|
| Build | `docker build -t petitpanier:1.0 .` | L'image Docker n'est pas construite. |
| Test | `docker compose ps` | Si l'application n'est pas healthy, la publication est bloquée. |
| Publish | `kind load docker-image petitpanier:1.0 --name petitpanier` | L'image n'est pas disponible dans le cluster local. |
| Deploy | `kubectl apply -f k8s/` | L'application n'est pas correctement déployée sur Kubernetes. |

**Question 7.1 : Pourquoi utiliser l'identifiant du commit plutôt que latest ?**

L'identifiant du commit permet d'identifier précisément chaque version d'une image Docker. Contrairement à `latest`, il garantit la traçabilité et facilite le retour à une version précédente en cas de problème, comme lors du rollback du palier 5.3.

**Remarque :** Dans ce TP, `kind load` remplace localement l'étape de mise à disposition de l'image. Dans un véritable pipeline CI/CD, cette étape utiliserait un registre d'images avec `docker push`.


### 7.2 — Workflow n8n

**Objectif :** Automatiser l'envoi d'une notification à l'équipe lorsqu'un déploiement de PetitPanier échoue.

**Workflow proposé :**

Webhook GitLab → IF (statut = failed) → Slack (#petitpanier-ops)

**Description des nœuds :**

1. Webhook GitLab : reçoit les informations sur l'état du pipeline CI/CD.
2. IF : vérifie si le déploiement a échoué (statut = failed).
3. Slack : si la condition est vraie, envoie une notification dans le canal #petitpanier-ops. Sinon, le workflow se termine sans envoyer de message.

**Message d'alerte :**

"Échec du déploiement PetitPanier. Le pipeline CI/CD a échoué. Veuillez consulter les logs GitLab pour identifier le problème."

**Sécurité des identifiants :**

Les identifiants et tokens de connexion sont stockés dans les Credentials de n8n et ne sont pas inscrits directement dans le workflow.

### 7.3 — Nettoyage

À compléter.

### Synthèse finale — CI/CD, n8n et nettoyage

À compléter.

