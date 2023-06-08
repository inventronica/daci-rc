# daci-rc

Acesta este repo-ul care conține fișierele cu programele care stau la baza funcționării robotului.

## Conținut

- folderul [`photos`](../master/photos) care  conține poze cu echipa ([`team_photos`](../master/photos/team_photos)), dar și cu robotul din toate părțile ([`robot_photos`](../master/photos/robot_photos))ș
- folderul [`scheme`](../master/scheme) care conține schema robotului.
- folderul [`src`](../master/src) conține toate fișierele cu programul propriu-zis al robotului;

## main.py 

Acest program utilizează informațiile transmise de cameră printr-un cablu USB (**culoare, dimensiune, poziție** și **confidență**). Folosește câte un **set-point** pentru fiecare tip de cub (**verde** sau **roșu**). Mărimi precum **suprafața, profunzimea** și **confidența** sunt inițializate la început, iar când este detectat un obstacol în față, se compară suprafața sa cu cea predefinită (**nulă**). Dacă
 suprafața obiectului este mai mare, este clasificat ca fiind cub. Apoi, în funcție de culoarea pe care o returnează camera, determină dacă este unul verde sau roșu. în fiecare caz, set-pointul pentru culoarea respectivă se schimbă și valoarea suprafeței se înlocuiește cu cea a cubului pentru a se putea repeta procesul. Dacă în fața robotului nu se află nici un cub, este folosită o variabilă care la început are valoarea 'UNKNOWN'. Dacă suprafața detectată nu este mai mare ca cea determinată anterior, variabila `next_cube` rămâne neschimbată.

 

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