# daci-rc

Acesta este repo-ul care conține fișierele cu programele care stau la baza funcționării robotului.

## Conținut

- folderul [`photos`](../master/photos) care  conține poze cu echipa ([`team_photos`](../master/photos/team_photos)), dar și cu robotul din toate părțile ([`robot_photos`](../master/photos/robot_photos))ș
- folderul [`scheme`](../master/scheme) care conține schema robotului.
- folderul [`src`](../master/src) conține toate fișierele cu programul propriu-zis al robotului;

## [follower.py](../master/src/follower.py)

Funcțiile esențiale pentru mișcarea robotului pe hartă, bazate pe informațiile furnizate de giroscop și senzorii de distanță și culoare, sunt implementate în fișierul [`Follower.py`](../master/src/follower.py).

În secțiunea `_init_` a clasei, se realizează inițializarea componentelor `gyro` și `pid` (pentru distanță). De asemenea, adresele senzorilor sunt modificate pentru asigurarea funcționării corecte. Procesele subiacente sunt de asemenea inițializate în această parte a clasei, permițând citirea independentă a senzorilor în cadrul programului.


Funcția `run_follower()` are rolul de a urmări pereții și de a menține o distanță constantă față de aceștia. Aceasta este esențială în etapa de calificare, în care nu există obstacole pe hartă. Funcția utilizează controlul PID al distanței în colaborare cu controlul PID al giroscopului pentru a crea un traseu optim pentru robot.

Funcția `run_gyro_follower` are un scop similar cu funcția `run_follower()`, cu excepția faptului că distanța setată în acest caz reprezintă un unghi la care se dorește să se ajungă.

`change_lane()`, folosește niște instrucțiuni prin care robotul poate să schimbe "banda" pe care se află. Pentru a fi un transfer cât mai sigur, robotul va face mai întâi un unghi de 45 de grade față de unghiul la care se afla înainte, iar apoi se redresează pe traiectoria inițială, însă cu peretele verificat de senzorul de distanță schimbat.



## [main.py](../master/src/main.py) 

Acest program utilizează informațiile transmise de cameră printr-un cablu USB (**culoare, dimensiune, poziție** și **confidență**). Folosește câte un **set-point** pentru fiecare tip de cub (**verde** sau **roșu**). Mărimi precum **suprafața, profunzimea** și **confidența** sunt inițializate la început, iar când este detectat un obstacol în față, se compară suprafața sa cu cea predefinită (**nulă**). Dacă
 suprafața obiectului este mai mare, este clasificat ca fiind cub. Apoi, în funcție de culoarea pe care o returnează camera, determină dacă este unul verde sau roșu. în fiecare caz, set-pointul pentru culoarea respectivă se schimbă și valoarea suprafeței se înlocuiește cu cea a cubului pentru a se putea repeta procesul. Dacă în fața robotului nu se află nici un cub, este folosită o variabilă care la început are valoarea 'UNKNOWN'. Dacă suprafața detectată nu este mai mare ca cea determinată anterior, variabila `next_cube` rămâne neschimbată.

 ## [motors.py](../master/src/motors.py)

 ## [pid.py](../master/src/pid.py)

 ## [sensors.py](../master/src/sensors.py)

## SSH

Modul de comunicare cu placa Raspberry Pi este prin SSH (Secure Shell), un protocol de comunicare securizat prin care placa poate să fie accesată de pe orice dispozitiv oferind acces la un terminal. Conectarea se face prin comanda `ssh [user]@[ipadress] -p [port]` unde user este user-ul de pe placa (implicit "pi"); ipadress este adresa ip locală a plăcii și port este portul SSH deschis pe placă (implicit 22) de exemplu modul implicit de conectare arată așa: `ssh pi@192.168.x.x -p 22` și când este solicitată se introduce parola user-ului, setată în momentul creării imaginii și sistemului de operare (implicit Raspberry). 

## Instrucțiuni instalare 

1. Instalarea sistemului de operare pe placa Raspberry
2. Conectarea prin SSH la placă
3. Clonarea repository-ului de git: `git clone git@github.com:inventronica/druid-rc.git` 
4. Deschiderea folderului: `cd druid-rc/raspberry`
5. Rularea aplicației cu comanda `python3 main.py`

## Conexiunea cu Raspberry 

Descarcă și instalează Raspberry Pi Imager pe un calculator cu un cititor de carduri SD. Pune cardul SD pe care o să îl folosești cu Raspberry Pi în cititor și deschide Raspberry Pi Imager. 
www.raspberrypi.com/software/