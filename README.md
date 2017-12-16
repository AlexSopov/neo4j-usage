# Neo4j usage
In this repository demonstratet using of Neo4j with java driver on Social network example.

[![Build Status](https://travis-ci.org/AlexSopov/neo4j-usage.svg?branch=master)](https://travis-ci.org/AlexSopov/neo4j-usage)
[![codecov](https://codecov.io/gh/AlexSopov/neo4j-usage/branch/master/graph/badge.svg)](https://codecov.io/gh/AlexSopov/neo4j-usage)
[![Codacy Badge](https://api.codacy.com/project/badge/Grade/4f59990c437f4fc880574d879840c35a)](https://www.codacy.com/app/AlexSopov/neo4j-usage?utm_source=github.com&amp;utm_medium=referral&amp;utm_content=AlexSopov/neo4j-usage&amp;utm_campaign=Badge_Grade)
[![Codacy Badge](https://api.codacy.com/project/badge/Coverage/4f59990c437f4fc880574d879840c35a)](https://www.codacy.com/app/AlexSopov/neo4j-usage?utm_source=github.com&utm_medium=referral&utm_content=AlexSopov/neo4j-usage&utm_campaign=Badge_Coverage)

### Existing social network
![graph](https://github.com/AlexSopov/neo4j-usage/blob/master/graph.png?raw=true)


Executed queries:

### Выдать упорядоченный ​ ​список ​ ​ФИО ​ ​персон.

``` MATCH (person:Person) RETURN person.surname, person.name ORDER BY person.surname, person.name ```
```
╒════════════════╤═════════════╕
│"person.surname"│"person.name"│
╞════════════════╪═════════════╡
│"Escalera"      │"Clara"      │
├────────────────┼─────────────┤
│"Gray"          │"Curtis"     │
├────────────────┼─────────────┤
│"Hawkins"       │"Joseph"     │
├────────────────┼─────────────┤
│"Jones"         │"Effie"      │
├────────────────┼─────────────┤
│"Laforce"       │"Richard"    │
├────────────────┼─────────────┤
│"Piere"         │"Joyce"      │
├────────────────┼─────────────┤
│"Pigg"          │"Pedro"      │
├────────────────┼─────────────┤
│"Rainey"        │"Violet"     │
├────────────────┼─────────────┤
│"Sopov"         │"Alex"       │
├────────────────┼─────────────┤
│"Wedding"       │"Earlene"    │
└────────────────┴─────────────┘
```


### Выдать ​ ​список ​ ​ФИО ​ ​мужчин ​ ​с ​ ​указанием ​ ​возраста, ​ ​упорядоченный ​ ​по убыванию ​ ​возраста.

``` MATCH (person:Person) WHERE person.gender=1 RETURN person.surname, person.name, %d-person.birthdate as age ORDER BY age DESC, person.surname, person.name ```
```
╒════════════════╤═════════════╤═════╕
│"person.surname"│"person.name"│"age"│
╞════════════════╪═════════════╪═════╡
│"Pigg"          │"Pedro"      │60   │
├────────────────┼─────────────┼─────┤
│"Laforce"       │"Richard"    │34   │
├────────────────┼─────────────┼─────┤
│"Gray"          │"Curtis"     │26   │
├────────────────┼─────────────┼─────┤
│"Hawkins"       │"Joseph"     │23   │
├────────────────┼─────────────┼─────┤
│"Sopov"         │"Alex"       │19   │
└────────────────┴─────────────┴─────┘
```

### Выдать ​ ​упорядоченный ​ ​список ​ ​ФИО ​ ​друзей ​ ​персоны ​ ​заданными ​ ​ФИО (Sopov).

``` MATCH (person:Person)-[:FRIENDS]-(friends) WHERE person.surname="%s" RETURN friends.surname, friends.name ```
```
╒═════════════════╤══════════════╕
│"friends.surname"│"friends.name"│
╞═════════════════╪══════════════╡
│"Rainey"         │"Violet"      │
├─────────────────┼──────────────┤
│"Laforce"        │"Richard"     │
├─────────────────┼──────────────┤
│"Pigg"           │"Pedro"       │
└─────────────────┴──────────────┘ 
```

###  Выдать ​ ​упорядоченный ​ ​список ​ ​ФИО ​ ​друзей ​ ​друзей ​ ​персоны ​ ​заданными ФИО (Sopov).

``` MATCH (person:Person)-[:FRIENDS]-(firstFriends)-[:FRIENDS]-(secondFriends) WHERE person.surname="Sopov" RETURN DISTINCT secondFriends.surname, secondFriends.name```
```
╒═══════════════════════╤════════════════════╕
│"secondFriends.surname"│"secondFriends.name"│
╞═══════════════════════╪════════════════════╡
│"Hawkins"              │"Joseph"            │
├───────────────────────┼────────────────────┤
│"Gray"                 │"Curtis"            │
├───────────────────────┼────────────────────┤
│"Piere"                │"Joyce"             │
├───────────────────────┼────────────────────┤
│"Jones"                │"Effie"             │
├───────────────────────┼────────────────────┤
│"Wedding"              │"Earlene"           │
├───────────────────────┼────────────────────┤
│"Escalera"             │"Clara"             │
└───────────────────────┴────────────────────┘
```

Other queries are providet without results. You can get results by executing it.

``` MATCH (person:Person)-[:FRIENDS]-(friends) RETURN person.surname, count(friends) as friendsCount ORDER BY friendsCount ```

``` MATCH (group:Group) RETURN group.groupname ORDER BY group.groupname ```

``` MATCH (person:Person)-[:IN_GROUP]-(groups) WHERE person.surname="Escalera" RETURN person.surname, groups.groupname ORDER BY groups.groupname ```

``` MATCH (group:Group)-[:IN_GROUP]-(person) RETURN group.groupname, count(person) as personsCount ORDER BY personsCount DESC ```

``` MATCH (person:Person)-[:IN_GROUP]-(groups) RETURN person.surname, count(groups) as groupsCount ORDER BY groupsCount DESC ```

``` MATCH (person:Person)-[:FRIENDS]-(firstFriends)-[:FRIENDS]-(secondFriends)-[:IN_GROUP]-(groups) WHERE person.surname="Sopov" RETURN count(DISTINCT groups.groupname) ```

``` MATCH (person:Person) WHERE person.surname="Sopov" RETURN person.tweets ```

``` MATCH (person:Person) WITH person.surname as surname, EXTRACT(tweet in person.tweets | size(tweet)) as tweetsSize UNWIND tweetsSize as tweetsSizeC RETURN surname, AVG(tweetsSizeC) ```

``` MATCH (person:Person) WITH COLLECT(person.tweets) as tweets RETURN filter(x IN REDUCE(out=[], r IN tweets | out + r) WHERE size(x)>10) ```

``` MATCH (person:Person) RETURN person.surname, size(person.tweets) as tweetsCount ORDER BY tweetsCount DESC ```

``` MATCH (person:Person)-[:FRIENDS]-(firstFriends)-[:FRIENDS]-(secondFriends) WHERE person.surname="Sopov" RETURN DISTINCT secondFriends.tweets ```
