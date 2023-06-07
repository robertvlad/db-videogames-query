# db-videogames-query

## Query
### SELECT

1- Selezionare tutte le software house americane (3)

```
select * 
from software_houses sh 
where sh.country like 'United States'
```

2- Selezionare tutti i giocatori della città di 'Rogahnland' (2)

```
select *
from players p 
where p.city like 'Rogahnland'
```

3- Selezionare tutti i giocatori il cui nome finisce per "a" (220)

```
select *
from players p 
where p.name like '%a'
```

4- Selezionare tutte le recensioni scritte dal giocatore con ID = 800 (11)

```
select *
from reviews r 
where r.player_id = 800
```

5- Contare quanti tornei ci sono stati nell'anno 2015 (9)

```
select *
from tournaments t 
where `year` = 2015
```

6- Selezionare tutti i premi che contengono nella descrizione la parola 'facere' (2)

```
select *
from awards a 
where a.description like '%facere%'
```

7- Selezionare tutti i videogame che hanno la categoria 2 (FPS) o 6 (RPG), mostrandoli una sola volta (del videogioco vogliamo solo l'ID) (287)

```
select distinct cv.videogame_id
from category_videogame cv 
where cv.category_id = 2 or cv.category_id = 6
```

8- Selezionare tutte le recensioni con voto compreso tra 2 e 4 (2947)

```
select *
from reviews r 
where r.rating between 2 and 4
```

9- Selezionare tutti i dati dei videogiochi rilasciati nell'anno 2020 (46)

```
select *
from videogames v 
where year(v.release_date) = 2020
```

10- Selezionare gli id dei videogame che hanno ricevuto almeno una recensione da stelle, mostrandoli una sola volta (443)

```
select distinct r.videogame_id 
from reviews r 
where r.rating = 5
```



#### Bonus

11- Selezionare il numero e la media delle recensioni per il videogioco con ID = 412 (review number = 12, avg_rating = 3.16 circa)

```
select count(*) , avg(rating) 
from reviews r
where r.videogame_id = 412
```

12- Selezionare il numero di videogame che la software house con ID = 1 ha rilasciato nel 2018 (13)

```
select *
from videogames v 
where v.software_house_id = 1 and year(release_date) = 2018
```



### GROUP BY

1- Contare quante software house ci sono per ogni paese (3)

```
select country 
from software_houses sh 
group by country 
```

2- Contare quante recensioni ha ricevuto ogni videogioco (del videogioco vogliamo solo l'ID) (500)

```
select r.videogame_id 
from reviews r 
group by r.videogame_id 
```

3- Contare quanti videogiochi hanno ciascuna classificazione PEGI (della classificazione PEGI vogliamo solo l'ID) (13)

```
select pegi_label_id, COUNT(*) 
from pegi_label_videogame plv 
group by pegi_label_id 
```

4- Mostrare il numero di videogiochi rilasciati ogni anno (11)

```
select year(release_date), count(*) 
from videogames v 
group by year(release_date)
```

5- Contare quanti videogiochi sono disponbiili per ciascun device (del device vogliamo solo l'ID) (7)

```
select device_id, count(*) 
from device_videogame dv 
group by device_id 
```

6- Ordinare i videogame in base alla media delle recensioni (del videogioco vogliamo solo l'ID) (500)

```
select videogame_id, avg(rating) as avgRating
from reviews r 
group by videogame_id
order by avgRating
```



### JOIN

1- Selezionare i dati di tutti giocatori che hanno scritto almeno una recensione, mostrandoli una sola volta (996)

```
select r.player_id, p.*
from players p 
join reviews r on p.id = r.player_id 
group by r.player_id 
```

2- Sezionare tutti i videogame dei tornei tenuti nel 2016, mostrandoli una sola volta (226)

```
select distinct  v.*
from videogames v 
join tournament_videogame tv on v.id = tv.videogame_id 
join tournaments t on t.id = tv.tournament_id 
where t.year = 2016
```

3- Mostrare le categorie di ogni videogioco (1718)

```
select v.name, c.name 
from categories c 
join category_videogame cv on c.id = cv.category_id 
join videogames v on v.id = cv.videogame_id 
```

4- Selezionare i dati di tutte le software house che hanno rilasciato almeno un gioco dopo il 2020, mostrandoli una sola volta (6)

```
select distinct sh.name 
from software_houses sh 
join videogames v on v.software_house_id = sh.id
where year(release_date) > 2019
```

5- Selezionare i premi ricevuti da ogni software house per i videogiochi che ha prodotto (55)

```
select sh.*, a.*
from awards a 
join award_videogame av on a.id = av.award_id 
join videogames v on v.id = av.videogame_id 
join software_houses sh on sh.id = v.software_house_id 
```

6- Selezionare categorie e classificazioni PEGI dei videogiochi che hanno ricevuto recensioni da 4 e 5 stelle, mostrandole una sola volta (3363)

```
select distinct v.name, c.name, pl.name 
from videogames v 
join category_videogame cv on v.id = cv.videogame_id 
join categories c on c.id = cv.category_id 
join pegi_label_videogame plv on plv.videogame_id = v.id 
join pegi_labels pl on plv.pegi_label_id = pl.id 
join reviews r on r.videogame_id = v.id 
where r.rating >= 4
```

7- Selezionare quali giochi erano presenti nei tornei nei quali hanno partecipato i giocatori il cui nome inizia per 'S' (474)

```
select distinct v.name, v.id
from videogames v 
join tournament_videogame tv on tv.videogame_id = v.id 
join tournaments t on t.id = tv.tournament_id 
join player_tournament pt on t.id = pt.tournament_id 
join players p on p.id = pt.player_id 
where p.name like 's%'
```

8- Selezionare le città in cui è stato giocato il gioco dell'anno del 2018 (36)

```
select distinct t.city 
from awards a 
join award_videogame av on a.id = av.award_id 
join videogames v on v.id = av.videogame_id 
join tournament_videogame tv on tv.videogame_id = v.id 
join tournaments t on t.id = tv.tournament_id 
where a.name like "gioco dell'anno" and av.`year` = 2018
```

9- Selezionare i giocatori che hanno giocato al gioco più atteso del 2018 in un torneo del 2019 (3306)

```
select p.*
from awards a 
join award_videogame av on a.id = av.award_id 
join videogames v on v.id = av.videogame_id 
join tournament_videogame tv on tv.videogame_id = v.id 
join tournaments t on t.id = tv.tournament_id 
join player_tournament pt on pt.tournament_id = t.id
join players p on pt.player_id = p.id 
where a.name like 'Gioco più atteso' and av.`year` = 2018 and t.`year` = 2019
```


#### Bonus

10- Selezionare i dati della prima software house che ha rilasciato un gioco, assieme ai dati del gioco stesso (software house id : 5)

```
select *
from software_houses sh 
join videogames v on v.software_house_id = sh.id 
order by v.release_date
limit 1
```

11- Selezionare i dati del videogame (id, name, release_date, totale recensioni) con più recensioni (videogame id : potrebbe uscire 449 o 398, sono entrambi a 20)

```
select r.videogame_id, v.name, v.release_date, count(*) 
from videogames v 
join reviews r on r.videogame_id = v.id 
group by r.videogame_id
order by count(*) desc 
limit 1
```

12- Selezionare la software house che ha vinto più premi tra il 2015 e il 2016 (software house id : potrebbe uscire 3 o 1, sono entrambi a 3)

```
select sh.*, count(*) 
from software_houses sh 
join videogames v on v.software_house_id = sh.id 
join award_videogame av on v.id = av.videogame_id 
where av.`year` between 2015 and 2016
group by sh.id 
order by count(*) desc 
limit 1

```

13- Selezionare le categorie dei videogame i quali hanno una media recensioni inferiore a 2 (10)

```
select c.*
from categories c 
join category_videogame cv ON c.id = cv.category_id 
join videogames v on v.id = cv.videogame_id 
join reviews r on r.videogame_id = v.id
where r.rating < 2
group by c.id 
order by c.id 
```
