# Description des éléments du modèle

Ici, nous décrivons les éléments minimaux de notre modèle.

## Les éléments principaux

- **Élément (abstrait) :** la classe qui regroupe tout ce qu'on manipule. Il est caractérisé par un titre, une description, un état, une échéance et une progression en % (calculée, pas stockée).
- **Tâche :** c'est l'exécution de quelque chose. Elle est caractérisée par une commande, une durée prévue et un code de retour. Elle contient des paramètres (0 ou plus), des ressources (0 ou 1), un journal pour garder l'historique de ce qui se passe (0 ou plus d'entrées) et une planification (0 ou 1). Sa progression est le temps simulé écoulé divisé par sa durée prévue.
- **Chaîne :** c'est une suite d'éléments lancés l'un à la suite de l'autre. Elle contient donc une liste ordonnée d'éléments (1 ou plus). Sa progression est la moyenne de celle de ses éléments.
- **Workflow :** il représente des éléments dont certains peuvent s'exécuter en parallèle et d'autres s'attendent. C'est un peu plus complexe que de simples chaînes, donc il contient des éléments (1 ou plus) et des dépendances (0 ou plus). Sa progression est la moyenne de celle de ses éléments.

Une tâche, une chaîne ou un workflow sont des types d'Élément. Une chaîne ou un workflow peut contenir n'importe quel type d'élément, y compris d'autres chaînes ou workflows, ce qui permet d'avoir plusieurs niveaux. On avait pensé initialement à des sous-tâches pour une tâche, mais cela revenait à une chaîne de tâches, donc on a abandonné l'idée.

## Les éléments qui composent les éléments principaux

- **Ressources :** il s'agit de ce qu'une tâche demande à la machine. Elles contiennent le nombre de machines, le nombre de cœurs et le temps maximal pour l'exécution de la tâche demandée.
- **Paramètre :** comme le nom le dit, il s'agit d'un paramètre qui servira lors du lancement. Il est caractérisé par un nom et une valeur.
- **Dépendance :** elle permet de fixer un ordre d'exécution pour des éléments qui se suivent. Dans le sens « cet élément attend celui-là », elle contient l'élément qui attend (la source), l'élément attendu (la cible) et la condition d'attente. Cette condition peut être reussie, echouee ou fin, ce dernier cas étant prévu quand on doit effectuer une action peu importe si l'élément précédent a réussi ou pas. La source et la cible appartiennent au même workflow.
- **Planification :** on précise quand la tâche démarre. Elle contient la date de début, le nombre d'exécutions (1 pour une exécution unique) et l'intervalle entre deux exécutions (utile seulement s'il y en a plusieurs).
- **Entrée de journal :** une ligne dans le journal d'exécution. Elle contient l'heure simulée et un message qui donne des infos sur l'état actuel de la tâche.

## Les valeurs fixes utilisées

- **etat :** en attente, en cours, reussie, echouee, annulee, ignoree.
- **condition de dependance :** reussie, echouee, fin.

## Les liens

- Un workflow contient des éléments (1 ou plus) et des dépendances (0 ou plus).
- Une chaîne contient des éléments (1 ou plus) dans l'ordre.
- Une tâche contient ses paramètres, ses ressources, son journal et sa planification (composition).
- Une dépendance pointe vers deux éléments, sa source et sa cible, par référence, sans les contenir.
- Si on supprime un élément, les éléments qu'il contient partent avec lui, et les dépendances dont il est la source ou la cible disparaissent.