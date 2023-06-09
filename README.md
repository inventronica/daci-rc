# daci-rc

Acesta este repo-ul care conține fișierele cu programele care stau la baza funcționării robotului.

## Conținut

- folderul [`photos`](../master/photos) care  conține poze cu echipa ([`team_photos`](../master/photos/team_photos)), dar și cu robotul din toate părțile ([`robot_photos`](../master/photos/robot_photos))ș
- folderul [`scheme`](../master/scheme) care conține schema robotului.
- folderul [`src`](../master/src) conține toate fișierele cu programul propriu-zis al robotului;

## [follower.py](../master/src/follower.py)

Funcțiile esențiale pentru mișcarea robotului pe hartă, bazate pe informațiile furnizate de giroscop și senzorii de distanță și culoare, sunt implementate în fișierul [`Follower.py`](../master/src/follower.py).

În secțiunea `_init_` a clasei `Follower()`, se realizează inițializarea componentelor `gyro` și `pid` (pentru distanță). De asemenea, adresele senzorilor sunt modificate pentru asigurarea funcționării corecte. Procesele subiacente sunt de asemenea inițializate în această parte a clasei, permițând citirea independentă a senzorilor în cadrul programului.


Funcția `run_follower()` are rolul de a urmări pereții și de a menține o distanță constantă față de aceștia. Aceasta este esențială în etapa de calificare, în care nu există obstacole pe hartă. Funcția utilizează controlul PID al distanței în colaborare cu controlul PID al giroscopului pentru a crea un traseu optim pentru robot.

Funcția `run_gyro_follower` are un scop similar cu funcția `run_follower()`, cu excepția faptului că distanța setată în acest caz reprezintă un unghi la care se dorește să se ajungă.

`change_lane()`, folosește niște instrucțiuni prin care robotul poate să schimbe "banda" pe care se află. Pentru a fi un transfer cât mai sigur, robotul va face mai întâi un unghi de 45 de grade față de unghiul la care se afla înainte, iar apoi se redresează pe traiectoria inițială, însă cu peretele verificat de senzorul de distanță schimbat.

## [main.py](../master/src/main.py) 

Acest program utilizează informațiile transmise de cameră printr-un cablu USB (**culoare, dimensiune, poziție** și **confidență**). Folosește câte un **set-point** pentru fiecare tip de cub (**verde** sau **roșu**). Mărimi precum **suprafața, profunzimea** și **confidența** sunt inițializate la început, iar când este detectat un obstacol în față, se compară suprafața sa cu cea predefinită (**nulă**). Dacă
 suprafața obiectului este mai mare, este clasificat ca fiind cub. Apoi, în funcție de culoarea pe care o returnează camera, determină dacă este unul verde sau roșu. în fiecare caz, set-pointul pentru culoarea respectivă se schimbă și valoarea suprafeței se înlocuiește cu cea a cubului pentru a se putea repeta procesul. Dacă în fața robotului nu se află nici un cub, este folosită o variabilă care la început are valoarea 'UNKNOWN'. Dacă suprafața detectată nu este mai mare ca cea determinată anterior, variabila `next_cube` rămâne neschimbată.

 ## [motors.py](../master/src/motors.py)

Codul necesar controlării servomotorului și a motorului responsabil de mișcarea mașinii este localizat în fișierul [`motors.py`](../master/src/motors.py).

În clasa `Motors()` la începutul codului, sunt inițializate limitele de putere pentru motoare și limitele minime și maxime pentru funcționarea servomotorului, precum și pinii de control necesari.

În această clasă se află funcția `set_speed()`, care inițial verifică integritatea parametrilor și îi încadrează între limite, dacă este cazul, apoi, în funcție de viteza setată, trimite date pe pinii **PWM**, pentru a determina mișcarea mașinii.

În aceeași clasă, se găsește și funcția `set_direction`, care primește ca parametru unghiul dorit pentru servomotor. Prin intermediul unui mapping calculat, această funcție convertește valorile și modifică orientarea roților în consecință.

## [pid.py](../master/src/pid.py)
 
Fișierul [`pid.py`](../master/src/pid.py) conține codul esențial pentru eficientizarea mișcării robotului. PID-ul este folosit pentru a reduce eroarea prin componentele de proporționalitate, derivatele și integralele.

În funcția `set_point()`, este setată pentru PID-ul senzorilor de distanță valoarea dorită pentru a determina distanța robotului față de perete.

Pentru a integra într-un mod cât mai eficient componentele necesare, este folosit un PID pentru giroscop și unul pentru senzorii de distanță.

În funcția `get_error`, se îmbină unghiul de la giroscop cu distanța față de perete, astfel robotul reușeste să se îndrepte pe traseu, acesta urmând un curs cât mai eficient. De asemenea, aici se setează peretele după care să se orienteze și se returnează eroarea, pe care robotul trebuie să o aducă cât mai aproape de 0.

Funcția `get_output()` este esențială pentru eficiența programului, calculează și returnează unghiul la care robotul trebuie să se orienteze pentru a face schimbări mici, prin care acesta să ajungă pe drumul potrivit.

`[dependencies.txt](../master/dependencies.txt)` este fișierul de pe care, la instalarea codului, trebuie inserate comenzile responsabile de încărcarea bibliotecilor necesare funcționării.

 ## [sensors.py](../master/src/sensors.py)

Fișierul [`sensors.py`](../master/src/sensors.py) conține clasele pentru toți senzorii, precum și teste asociate fiecărei clase, permițând verificarea eficienței programului.

În clasa `Color()` se găsește funcția `color_read()` care are rolul de a determina culoarea detectată de senzorul de culoare. Funcția returnează **0** în cazul în care senzorul nu detectează nici albastru, nici oranj, **1** dacă detectează culoarea oranj și **2** dacă detectează culoarea albastră. Această informație este utilă pentru a determina direcția în care se deplasează robotul și pentru a efectua curbele necesare în direcția corespunzătoare. Biblioteca senzorului de culoare oferă capacitatea de a obține date prin intermediul valorilor **R**,**G**,**B**. Prin utilizarea unei funcții matematice adecvate, putem determina culoarea detectată de către senzor.

În această clasă, se mai află și funcțiile `power_off()` și `power_on()`, care se ocupă de resetarea senzorului.

În clasa `Gyro()`, există funcția `calculate_angle()` care utilizează viteza unghiulară furnizată de giroscop pentru a determina unghiul la care se află robotul în raport cu unghiul inițial la care a fost pornit. Aceasta se realizează prin aplicarea unei formule matematice specifice.

În clasa `Tof()`, pentru a gestiona adresele comune ale senzorilor de distanță și a senzorului de culoare, la inițializarea obiectului, folosim instrucțiuni care ne permit să le modificăm adresele în mod consecutiv, doar la începutul programului. Adresa inițială este setată la **0x29**, iar senzorii de distanță sunt echipați cu un pin **XSHUT** pentru resetare. Astfel, la început, avem ambele senzori de distanță conectați și senzorul de culoare oprit (folosind un tranzistor). Apoi, modificăm adresa senzorilor de distanță, resetăm un senzor de distanță și apoi modificăm adresa acestuia. În cele din urmă, pornim senzorul de culoare, care rămâne la adresa inițială, deoarece biblioteca acestuia nu include o funcție pentru schimbarea adresei. Toate aceste operațiuni pot fi apelate utilizând funcțiile `change_address()` și `reset_address()`.

Funcția `get_distance()` returnează cu ușurință, printr-o valoare de tip double, distanța pe care o face cu peretele, în centrimetri.

## SSH

Modul de comunicare cu placa Raspberry Pi este prin SSH (Secure Shell), un protocol de comunicare securizat prin care placa poate să fie accesată de pe orice dispozitiv oferind acces la un terminal. Conectarea se face prin comanda `ssh [user]@[ipadress] -p [port]` unde user este user-ul de pe placa (implicit "pi"); ipadress este adresa ip locală a plăcii și port este portul SSH deschis pe placă (implicit 22) de exemplu modul implicit de conectare arată așa: `ssh pi@192.168.x.x -p 22` și când este solicitată se introduce parola user-ului, setată în momentul creării imaginii și sistemului de operare (implicit Raspberry). 

## Instrucțiuni instalare 

1. Instalarea sistemului de operare pe placa Raspberry;
2. Conectarea prin SSH la placă;
3. Clonarea repository-ului de git: `git clone git@github.com:inventronica/daci-rc.git`;
4. Instalarea bibliotecilor din fișierul `dependencies.txt`, prin terminalul plăcii;
5. Deschiderea folderului: [`cd daci-rc/src/`](../master/src);
6. Rularea aplicației cu comanda `python3 main.py`.

## Conexiunea cu Raspberry 

Descarcă și instalează [`Raspberry Pi Imager`](www.raspberrypi.com/software/) pe un calculator cu un cititor de carduri SD. Pune cardul SD pe care o să îl folosești cu Raspberry Pi în cititor și deschide Raspberry Pi Imager. 
