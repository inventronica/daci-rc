# daci-rc
Acesta este repo-ul care contine fisierele cu programele care stau la baza functionarii robotului.
## Continut
- folderul `detect_cubes` contine in fisierul `main.py` un program care determina ce culoare are cubul care se afla in fata robotului.
- folderul `raspberry` contine in fisierul `main.py` programul care se ocupa de miscarea robotului pe harta, in functie de pereti.
- folderul `servo_calib` contine in fisierul `servo_calib.ino` calibrarea servo-motorului de pe robot.
## main.py (detect_cubes)
Acest program utilizeaza informatiile transmise de camera printr-un cablu USB(culoare, dimensiune, pozitie si confidenta). Intai determina ce fel de obstacol se afla in fata lui, cub sau perete, calculand suprafata acestuia. Pentru fiecare cub gasit, se modifica set-pointul in functie de culoarea sa. 
## main.py (raspberry)
Programul foloseste *time of light* pentru a calcula distanta pana la obstacol, calculeaza unghiul de rotatie si regleaza viteza pentru a lua curba intr-un mod cat mai eficient. 
