# Setup benchmark

Dans ce repository, vous pouvez retrouver la configuration de JMeter utilisé pour effectuer
les tests, le protocol ainsi qu'un dump de la BDD.

## BDD

La base de données est du MariaDB.

## Protocole de test

Avant chacun de nos tirs de performances, nous avons préchauffé les applications avec des
tirs à blanc.

Nous avons mesuré la consommation énergétique en watt
directement sur la prise de courant du serveur.

Afin d’injecter facilement des requêtes, nous utilisons l’outil JMeter. Le script de test
est paramétrable pour jouer sur le ratio de requête de modification et de requête de
lecture. Afin de minimiser l’effet d’impact de
l’injecteur sur les mesures, l’injecteur possède sa propre machine et celle-ci
est directement branchée en Ethernet sur le serveur.

Les lectures sont faites sur l'endpoint `GET /api/chantier`
Les écritures sur `POST /api/chantier`

Nos tests ont été réalisé avec le paramétrage suivant :

__Test 1__
100 concurrents 100 itérations = 10 000 requêtes, ratio lecture/écriture = 2

__Test 2__
100 concurrents 500 itérations = 50 000 requêtes, ratio lecture/écriture = 2

__Test 3__
200 concurrents 100 itérations = 20 000 requêtes, ratio lecture/écriture = 2

## JMeter

``jmeter.sh -n -t POWER_BENCHMARK.jmx -JthreadCount=10 -JtimerDelayOffset=500 -JtimerDeviation=300 -JpostCount=2 -JgetCount=8``

- `threadCount` nombre de threads simultanés (default 10)
- `timerDelayOffset` Constant delay offset du timer gaussien, c'est la valeur moyenne (ms, default 500)
- `timerDeviation` deviation du timer gaussien, c'est les variations par rapport au delay offer (ms, default 300)
- `postCount` nombre de POST par thread (default 2)
- `getCount` nombre de GET par thread (default 8)

##### Exemple des paramètres du timer :

si `timerDelayOffset = 500` et `timerDeviation = 300`, le delay va varier 68% du temps entre 200 et 800. 
