--1. Liste des potions : Numéro, libellé, formule et constituant principal. (5 lignes)

select *
from potion p ;

--2. Liste des noms des trophées rapportant 3 points. (2 lignes)

SELECT nom_categ 
FROM categorie c 
WHERE nb_points  = 3;

--3. Liste des villages (noms) contenant plus de 35 huttes. (4 lignes)

select nom_village
from village v 
where v.nb_huttes > 35;

--4. Liste des trophées (numéros) pris en mai / juin 52. (4 lignes)

select *
from trophee t 
where date_prise > '2052-05-01'
and date_prise < '2052-06-30'

--5. Noms des habitants commençant par 'a' et contenant la lettre 'r'. (3 lignes)

select *
from habitant h 
where h.nom 
like 'A%r%'

--6. Numéros des habitants ayant bu les potions numéros 1, 3 ou 4. (8 lignes)

select distinct  num_hab 
from absorber a  
where a.num_potion in (1, 3 ,4);

--7. Liste des trophées : numéro, date de prise, nom de la catégorie et nom du preneur. (10lignes)

select c.nom_categ  , h.nom 
from trophee t 
inner join categorie c on t.code_cat = c.code_cat 
inner join habitant h on h.num_hab = t.num_preneur 

--8. Nom des habitants qui habitent à Aquilona. (7 lignes)

select h.nom, v.nom_village 
from habitant h
inner join village v  on h.num_village = v.num_village 
where v.nom_village  = 'Aquilona'

--9. Nom des habitants ayant pris des trophées de catégorie Bouclier de Légat. (2 lignes)

select h.nom 
from habitant h 
inner join trophee t on h.num_hab = t.num_preneur 
inner join categorie c on t.code_cat = c.code_cat 
where c.nom_categ = 'Bouclier de Légat'

--10. Liste des potions (libellés) fabriquées par Panoramix : libellé, formule et constituantprincipal. (3 lignes)

select lib_potion as libellé, formule, constituant_principal
from habitant h 
inner join fabriquer f on f.num_hab  = h.num_hab 
inner join potion on f.num_potion = potion.num_potion 
where h.nom = 'Panoramix'

--11. Liste des potions (libellés) absorbées par Homéopatix. (2 lignes)

select p.lib_potion
from habitant h 
inner join absorber a on a.num_hab  = h.num_hab 
inner join potion p on p.num_potion = a.num_potion 
where h.nom = 'Homéopatix'
group by p.lib_potion 

--12. Liste des habitants (noms) ayant absorbé une potion fabriquée par l'habitant numéro 3. (4 lignes)

select h.nom 
from habitant h 
inner join absorber a on a.num_hab = h.num_hab 
inner join potion p on p.num_potion = a.num_potion 
inner join fabriquer f ON f.num_potion = p.num_potion 
where f.num_hab = 3
group by h.nom 

--13. Liste des habitants (noms) ayant absorbé une potion fabriquée par Amnésix. (7 lignes)

select distinct  h.nom 
from habitant h 
inner join absorber a on a.num_hab = h.num_hab 
inner join potion p on p.num_potion = a.num_potion 
inner join fabriquer f ON f.num_potion = p.num_potion 
inner join habitant h2 on f.num_hab = h2.num_hab 
where h2.nom = 'Amnésix'


--14. Nom des habitants dont la qualité n'est pas renseignée. (2 lignes)

select *
from habitant h 
where h.num_qualite is null 

--15. Nom des habitants ayant consommé la Potion magique n°1 (c'est le libellé de lapotion) en février 52. (3 lignes)

select *
from habitant h 
inner join absorber a on a.num_hab = h.num_hab 
inner join potion p on p.num_potion = a.num_potion 
where a.date_a > '2052-02-01'
and a.date_a < '2052-02-28'
and p.num_potion = 1

--16. Nom et âge des habitants par ordre alphabétique. (22 lignes)

select *
from habitant h 
order by h.nom 

--17. Liste des resserres classées de la plus grande à la plus petite : nom de resserre et nom du village. (3 lignes)

select r.nom_resserre , v.nom_village 
from resserre r 
inner join village v on v.num_village = r.num_village 
order by r.superficie desc 

--18. Nombre d'habitants du village numéro 5. (4)

select count(*)
from habitant h 
inner join village v on v.num_village = h.num_village 
where v.num_village = 5

--19. Nombre de points gagnés par Goudurix. (5)

select sum(c.nb_points)
from categorie c 
inner join trophee t on t.code_cat = c.code_cat 
inner join habitant h on h.num_hab = t.num_preneur 
where h.nom = 'Goudurix'

--20. Date de première prise de trophée. (03/04/52)

select min(t.date_prise)
from trophee t 

--21. Nombre de louches de Potion magique n°2 (c'est le libellé de la potion) absorbées. (19)

select sum(a.quantite) as nombre_de_louches
from potion p 
inner join absorber a on a.num_potion = p.num_potion 
where p.lib_potion = 'Potion magique n°2'

--22. Superficie la plus grande resserre. (895)

select max(r.superficie)
from resserre r 

--23. Nombre d'habitants par village (nom du village, nombre). (7 lignes)

select v.nom_village as village, count (distinct h.nom) as nombre
from habitant h 
inner join village v on v.num_village = h.num_village 
group by v.nom_village 

--24. Nombre de trophées par habitant (6 lignes)

select count(distinct t.num_trophee), h.nom 
from trophee t 
inner join habitant h on h.num_hab = t.num_preneur 
group by h.nom 

--25. Moyenne d'âge des habitants par province (nom de province, calcul). (3 lignes)

select p.nom_province, avg(h.age) as moyenne_age
from province p 
inner join village v ON v.num_province = p.num_province  
inner join habitant h on h.num_village = v.num_village 
group by p.nom_province 

--26. Nombre de potions différentes absorbées par chaque habitant (nom et nombre). (9lignes)

select h.nom, count(distinct p.lib_potion) 
from habitant h 
inner join absorber a on a.num_hab = h.num_hab 
inner join potion p on p.num_potion = a.num_potion 
group by h.nom 

--27. Nom des habitants ayant bu plus de 2 louches de potion zen. (1 ligne)

select h.nom, p.lib_potion, a.quantite  
from habitant h 
inner join absorber a on a.num_hab = h.num_hab 
inner join potion p on p.num_potion = a.num_potion 
where p.lib_potion = 'Potion Zen' and a.quantite > 2

--28. Noms des villages dans lesquels on trouve une resserre (3 lignes)

select nom_village
from village v 
inner join resserre r on r.num_village = v.num_village 

--29. Nom du village contenant le plus grand nombre de huttes. (Gergovie)

SELECT v.nom_village, v.nb_huttes 
FROM village v 
join (SELECT max(v.nb_huttes) as nb_huttes_max from village v ) max_table
on v.nb_huttes = max_table.nb_huttes_max

--30. Noms des habitants ayant pris plus de trophées qu'Obélix (3 lignes).

-- select count(t.num_preneur) , h.nom 
-- from habitant h 
-- inner join trophee t on t.num_preneur = h.num_hab 
-- group by h.nom 

select count(*) as nb_trophees, h.nom  
from habitant h 
inner join trophee t on t.num_preneur = h.num_hab 
group by h.nom 
having count(t.num_preneur) > 
(
	select count(t2.num_preneur)
	from habitant h2 
	inner join trophee t2 on t2.num_preneur = h2.num_hab 
	where h2.nom = 'Obélix'
)
