# Gestionnaire de tâches : fonctionnalités et évolutions

2026-11-25 · Jean-Marie

## Affichage

On voit toutes les tâches d'un coup d'œil et on peut ouvrir le détail de chacune.

- **Liste des tâches :** les tâches sont rangées en arbre, avec leur titre, leur type, leur statut et leur échéance. Les éléments contenus dans une chaîne ou un workflow apparaissent sous leur parent.
- **Détail d'une tâche :** un clic sur une tâche affiche toutes ses informations. Si elle demande des machines, on voit aussi les ressources (machines, cœurs, temps maximal).

## Édition

On modifie une tâche directement, et l'application refuse les valeurs absurdes.

- **Modification directe :** le titre, le statut et l'échéance se changent dans la liste, sans ouvrir de fenêtre.
- **Contrôle de saisie :** un temps mal écrit ou un nombre de machines négatif est refusé, avec un message qui explique pourquoi.

## Création et suppression

On crée trois sortes d'éléments et on peut les supprimer proprement.

- **Créer :** une tâche, une chaîne ou un workflow. On peut ranger n'importe lequel dans une chaîne ou un workflow existant.
- **Supprimer :** un élément part avec tous les éléments qu'il contient, et les dépendances dont il est la source ou la cible disparaissent aussi.

## Planification et déclenchement

Une tâche dit quand et pourquoi elle démarre.

- **Exécution dans le temps :** une tâche a une date de début, un nombre d'exécutions (1 pour une exécution unique) et un intervalle entre deux exécutions.
- **Déclenchement par événement :** une tâche démarre quand quelque chose se passe, à l'extérieur ou à l'intérieur de l'application.

## Enchaînement et dépendances

Les tâches s'enchaînent selon des règles claires, et l'application empêche les blocages.

- **Forme d'un élément :** tâche unique, chaîne (l'une après l'autre) ou workflow (certains éléments en parallèle, d'autres qui en attendent). La répétition se règle dans la planification, avec le nombre d'exécutions.
- **Dépendances :** un élément peut en attendre un autre, dans le même workflow. Il démarre quand la condition est remplie : reussie, echouee ou fin (quel que soit le résultat). S'il en attend plusieurs, toutes les conditions doivent être remplies.
- **Détection des blocages :** si A attend B et B attend A, l'application le refuse avec un message clair.
- **Contrôle de concurrence :** on limite le nombre d'exécutions simultanées et on évite les chevauchements.

## Suivi

Chaque tâche garde son historique : son état (en attente, en cours, reussie, echouee), sa durée, son code de retour et son journal.

## États d'une tâche

Une tâche passe par une suite d'états, et seul le dernier est définitif.

- **en attente :** la tâche existe mais n'a pas démarré. Elle attend son tour ou ses dépendances.
- **en cours :** la simulation la joue, sa durée s'écoule.
- **reussie :** elle est finie normalement, avec un code de retour « ok ».
- **echouee :** elle s'est terminée sur une erreur, avec un code de retour d'échec.
- **annulee :** l'utilisateur l'a arrêtée avant ou pendant son exécution.
- **ignoree :** elle n'a pas été jouée parce qu'une tâche dont elle dépend a échoué.

Le trajet normal est : en attente, en cours, puis reussie ou echouee. L'état d'une chaîne ou d'un workflow se déduit de celui de ses éléments : en cours si l'un est en cours, reussie si tous le sont, echouee si l'un d'eux est echouee.

## Progression en pourcentage

Chaque élément affiche son avancement en pourcentage, sous forme de barre, et il se met à jour tout seul.

- **Pour une tâche seule :** le temps simulé écoulé divisé par sa durée prévue. Elle est à 0 % en attente et à 100 % quand elle est terminée.
- **Pour une chaîne ou un workflow :** la moyenne de l'avancement de ses éléments.
- **Affichage visuel :** une barre de progression dans la liste, à côté du pourcentage, colorée selon l'état (gris en attente, bleu en cours, vert reussie, rouge echouee). La même barre apparaît dans le détail de la tâche.
- **Mise à jour automatique :** les barres avancent à chaque pas de la simulation, sans action de l'utilisateur. Elles se recalculent aussi quand on ajoute ou supprime un élément.
- **Vue d'ensemble :** l'avancement global de toutes les tâches s'affiche dans la barre d'état.

## Simulation de l'exécution

L'application ne lance aucun vrai programme : elle simule l'exécution des tâches.

- **Lancer la simulation :** chaque tâche passe de « en attente » à « en cours », puis à « reussie » ou « echouee », en respectant les dépendances et l'ordre prévu.
- **Durée simulée :** chaque tâche a une durée prévue, jouée en accéléré.
- **Résultat simulé :** un code de retour et un journal sont produits pour chaque tâche, y compris des échecs pour tester les réactions de l'application.
- **Suivi en direct :** on voit les états changer dans la liste pendant que la simulation avance.

## Recherche

On retrouve vite une tâche.

- **Barre de recherche :** recherche par titre.
- **Filtres :** par statut, par type, ou les tâches en retard.

## Enregistrement des données

On peut enregistrer toutes les tâches dans un fichier, puis les rouvrir plus tard.

- **Enregistrer :** tout est sauvegardé d'un coup, dépendances comprises.
- **Ouvrir :** on retrouve les tâches exactement comme on les a laissées.

## Évolutions possibles

Ces idées viennent après le cœur du projet, si le temps le permet.

- **Planification avancée :** expressions cron, intervalles fixes, créneaux horaires minimum et maximum.
- **Déclencheurs précis :** arrivée d'un fichier, appel externe (webhook), fin d'une autre tâche, condition métier.
- **Nouvelles tentatives automatiques :** relance après un échec, avec un délai d'attente entre chaque essai.
- **Tolérance aux pannes :** bascule sur une machine de secours et reprise après incident sans perdre la planification.
- **Alertes :** e-mail, Slack ou outil de supervision en cas d'anomalie.
- **Sécurité et gouvernance :** droits par rôle, stockage sécurisé des mots de passe, traçabilité des modifications et des exécutions.
- **Gestion des problèmes :** l'application repère une tâche trop longue ou bloquée, puis l'arrête proprement ou lance une tâche de secours.