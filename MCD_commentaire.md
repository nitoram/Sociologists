# Commentaire du modèle conceptuel

# Person
Peut se rapporter à tout être humain, ici plus spécifiquement à des sociologues, ou à leurs parents. Classe objet.
Propriétés : nom, prénom, genre, éventuelle date de mort.

# Birth
Date à laquelle quelqu'un est né.e. Classe temporelle.
Propriété : date.
Relations : pour chaque date de naissance cette classe est reliée à un lieu géographique unique.

# Geographical place
Endroit où à lieu quelque chose. Classe objet.
Propriétés : nom, longitude, latitude, éventuellement PIB, éventuellement langue parlées, type.
Relations : un lieu géographique est toujours relié à un type qui spécifie le lieu dont on parle.

# Type of geographical place
La spécificité d’un lieu ( pays, villes, etc.). Classe objet.
Propriétés : nom, définition.
Relations : une relation réflexive qui spécifie de quel type de lieu géorgraphique on parle.

# Ideology
Courant idéologique. Classe temporelle.
Propriétés : date de début, date de fin éventuelle.
Relations : un courant de pensée ou une idéologie est toujours relié à une personne ainsi qu'à un type qui spécifie à quoi on se réfère.

# Type of ideology
La spécificité d’un courant de pensée (politique, intellectuel), nom, définition, objectifs. Classe objet.
Propriétés : nom, définition.
Relations : une relation réflexive qui spécifie de quel type de courant de pensées ou d'idéologie il s'agit.

# Education
Le fait de faire telle formation pendant telle durée de temps. Classe temporelle.
Propriétés : date de début, date de fin.
Relations : la catégorie éduction est toujours reliés à une personne, à un type qui spécifie le domaine d'éduction, ainsi qu'à une organisation au sein de laquelle à pris place le cursus.

# Area of education
La spécificité de la formation. Classe objet.
Propriétés : nom, définition.
Relations : une relation réflexive qui spécialise le domaine d'études.

# Pursuit
Le fait d'avoir un certaine occupation pendant une période donnée. Classe temporelle.
Propriétés : date de début, éventuelle date de fin.
Relations : la classe pursuit est toujours associée a une personne ainsi qu'à une occupation qui spécifie son type, mais aussi à une organisation au sein de laquelle l'activité prend place.

# Occupation
Un métier ou tout autre travail exercé professionnellement pendant une période donnée. Classe objet.
Propriétés : nom, date de début, éventuelle date de fin, définition.
Relations : relation réflxive qui spécifie le type d'occupation particulier.

# Organisation 
Entité sociale réunissant des personnes et via laquelle on peut poursuivre une formation ou une occupation. Classe objet.
Propriétés : nom, définition.
Relations : une oragnisation est toujours reliée à un seul lieu géographique.