# Requêtes pour explorer la base de donmées

# Pays de naissance
Cette requête nous permet de voir dans quel pays chaque sociologue de notre base de données est né(e).

	SELECT p.name, b.date, gp.name 
	FROM birth b 
		join person p on p.pk_person = b.fk_person 
		join geographical_place gp on gp.pk_geographical_place = b.fk_geographical_place 

# Appartenance à des organisations
Cette requête nous permet de voir à quelles organisations les personnes de notre base de données ont apaprtenues et durant combien de temps, la date de naissance des personnes apparait également à titre indicatif afin de pouvoir mettre en lien l'appartenance à une organisation et la période de la vie d'une personne.

SELECT p2. name, p2. date_birth, o.name, p.date_begin, p.date_end 
FROM pursuit p 
	join person p2 on p2. pk_person = p.fk_person 
	join organisation o on o.pk_organisation = p.fk_organisation 