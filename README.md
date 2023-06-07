# daci-rc

Acesta este repo-ul care conține fișierele cu programele care stau la baza funcționării robotului.

## Conținut

- folderul `detect_cubes` conține în fișierul `main.py` un program care determină ce culoare are cubul care se află in fața robotului.
- folderul `raspberry` conține în fișierul `main.py` programul care se ocupă de mișcarea robotului pe hartă, în funcție de pereți.
- folderul `servo_calib` conține în fișierul `servo_calib.ino` calibrarea servo-motorului de pe robot.

## main.py (detect_cubes)

Acest program utilizează informațiile transmise de cameră printr-un cablu USB (culoare, dimensiune, poziție si confidență). Întâi determină ce fel de obstacol se află în fața lui, cub sau perete, calculând suprafața acestuia. Pentru fiecare cub găsit, se modifică set-pointul în funcție de culoarea sa. 

## main.py (raspberry)

Programul folosește *time of light* (timpul necesar luminii pentru a ajunge la obiect) pentru a calcula distanța până la obstacol, calculează unghiul cu care trebuie să ia curba și reglează viteza pentru a schimba direcția într-un mod cât mai eficient. 

## SSH

Modul de comunicare cu placa Raspberry Pi este prin SSH (Secure Shell), un protocol de comunicare securizat prin care placa poate să fie accesată de pe orice dispozitiv oferind acces la un terminal. Conectarea se face prin comanda `ssh [user]@[ipadress] -p [port]` unde user este user-ul de pe placa (implicit "pi"); ipadress este adresa ip locală a plăcii și port este portul ssh deschis pe placă (implicit 22) de exemplu modul implicit de conectare arată așa: `ssh pi@192.168.x.x -p 22` și când este solicitată se introduce parola user-ului, setată în momentul creării imaginii și sistemului de operare (implicit Raspberry). 

## Instrucțiuni instalare 

1. Instalarea sistemului de operare pe placa Raspberry
2. Conectarea prin SSH la placă
3. Clonarea repository-ului de git: `git clone git@github.com:inventronica/druid-rc.git` 
4. Deschiderea folderului: `cd druid-rc/raspberry`
5. Rularea aplicației cu comanda `python3 main.py`

## Conexiunea cu Raspberry 

Descarca si instaleaza Raspberry Pi Imager pe un calculator cu un cititor de carduri SD. Pune cardul SD pe care o sa il folosesti cu Raspberry Pi in cititor si deschide Raspberry Pi Imager. 
www.raspberrypi.com/software/
