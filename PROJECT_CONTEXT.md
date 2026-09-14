# Context pentru continuarea proiectului

## Scop

Autorul dezvolta o disertatie de master despre controlul inteligent al unui brat robotic prin recunoastere faciala si gesturi. Implementarea trebuie sa fie demonstrabila, masurabila si sigura. Se incepe cu un MVP format din camera, autorizare faciala si un singur servo, apoi se extinde gradual.

## Hardware disponibil

- Raspberry Pi 5;
- Raspberry Pi OS 64-bit;
- camera Raspberry Pi de 8 MP, conectata la interfata CSI;
- un servo albastru de tip hobby, cuplu redus;
- servo conectat pentru test astfel:
  - semnal PWM: GPIO18, pin fizic 12;
  - alimentare: 5 V;
  - masa: GND.

Servo-ul actual este potrivit numai pentru calibrare si demonstratie fara sarcina. Un brat robotic cu mai multe articulatii va necesita servomotoare potrivite, sursa separata, masa comuna, driver PWM (de exemplu PCA9685), buton de oprire de urgenta si limite mecanice.

## Cod sursa existent

Codul proiectului de licenta OmniLock a fost analizat local. Fisierele importante sunt:

- `pitonu.py`: aplicatie Flask monolitica, camera, recunoastere faciala, servo, buzzer, QR, baza de date si thread-uri;
- `gui_interface.py`: interfata Tkinter/ttkbootstrap si logica alternativa pentru capturare/control;
- `write.py`: cod RFID MFRC522, in afara MVP-ului initial;
- `requirements.txt`: vechi si incomplet, nu trebuie instalat fara verificarea importurilor;
- `haarcascade_frontalface_default.xml`: cascada Haar pentru detectie;
- `encodings.pickle`: encodari faciale existente, sensibile, pastrate numai local.

Codul reutilizabil dorit este:

`camera -> detectie fata -> encoding -> comparare -> utilizator autorizat -> actiune`

Fluxul vechi deschidea o usa. Fluxul nou va fi:

`recunoastere faciala -> autorizare robot -> gest valid -> comanda servo`

## Probleme cunoscute

1. Codul vechi este monolitic si trebuie separat in module.
2. Acceptarea dupa un singur cadru este instabila; foloseste o regula de tip 5 din 7 cadre.
3. Trebuie evitate conflictele dintre GPIO18 folosit pentru servo si buzzerul vechi.
4. Servo-ul nu trebuie alimentat din Raspberry Pi pentru un brat complet.
5. Nu executa aplicatia Flask veche ca prim test.
6. Parolele SMTP si alte secrete din codul vechi trebuie revocate si mutate in variabile de mediu.
7. Nu copia `encodings.pickle`, `dataset/` sau baze de date in GitHub.
8. Camera CSI a Raspberry Pi nu este aceeasi cu camera browserului obtinuta prin `getUserMedia()`.

## Arhitectura tinta

```text
camera.py
    |
face_auth.py ---- safety_manager.py
    |                    |
gesture_recognizer.py -> state_machine.py
                              |
                      arm_controller.py
                              |
                         servo/PCA9685
```

Modulele trebuie implementate incremental:

- `camera.py`: pornire/oprire Picamera2 si capturi;
- `face_auth.py`: detectie, encodare si autorizare stabilizata;
- `gesture_recognizer.py`: gesturi discrete, cu incredere si cooldown;
- `state_machine.py`: `IDLE`, `FACE_AUTHENTICATING`, `AUTHORIZED`, `GESTURE_CONTROL`, `FAULT`;
- `arm_controller.py`: limite unghiulare, rate limiting si pozitie sigura;
- `safety_manager.py`: oprire de urgenta, timeout si fail-safe.

## Validare obligatorie

Fiecare etapa trebuie testata separat:

1. `rpicam-hello`;
2. captura Picamera2;
3. PWM pe GPIO18, fara sarcina;
4. detectie faciala;
5. autorizare stabilizata;
6. gesturi;
7. simulator de brat;
8. servo real cu limite restrictive.

Nu conecta un brat mecanic si nu aplica sarcina pana cand pozitiile, limitele si oprirea de urgenta nu au fost verificate.

## Stil de colaborare

Explica in limba romana, cu pasi mici si comenzi compatibile cu Raspberry Pi OS 64-bit. Inainte de instalari sau modificari hardware, verifica arhitectura, pinii si rezultatul comenzii anterioare. Nu presupune ca fisierele sensibile exista in repository.
