# daci-rc
Acesta este repo-ul care conține fișierele cu programele care stau la baza funcționării robotului.
## Conținut
- folderul `detect_cubes` conține în fișierul `main.py` un program care determină ce culoare are cubul care se află in fața robotului.
- folderul `raspberry` conține în fișierul `main.py` programul care se ocupă de mișcarea robotului pe hartă, în funcție de pereți.
- folderul `servo_calib` conține în fișierul `servo_calib.ino` calibrarea servo-motorului de pe robot.
## main.py (detect_cubes)
Acest program utilizează informațiile transmise de cameră printr-un cablu USB(culoare, dimensiune, poziție si confidență). Întâi determină ce fel de obstacol se află în fața lui, cub sau perete, calculând suprafața acestuia. Pentru fiecare cub găsit, se modifică set-pointul în funcție de culoarea sa. 
## main.py (raspberry)
Programul folosește *time of light*(timpul necesar luminii pentru a ajunge la obiect) pentru a calcula distanța până la obstacol, calculează unghiul cu care trebuie să ia curba și reglează viteza pentru a schimba direcția într-un mod cât mai eficient. 
