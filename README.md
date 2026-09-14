# Control inteligent al unui brat robotic

Proiect de disertatie pentru controlul unui brat robotic prin:

1. autentificare prin recunoastere faciala;
2. recunoasterea unor gesturi;
3. o masina de stari pentru autorizare si control;
4. comenzi sigure catre servomotoare.

## Platforma

- Raspberry Pi 5;
- Raspberry Pi OS 64-bit;
- camera Raspberry Pi de aproximativ 8 MP;
- un servo albastru ieftin pentru prototipul initial;
- PWM servo: GPIO18, pin fizic 12;
- alimentare servo pentru test initial: 5 V si GND;
- Picamera2/libcamera pentru camera;
- Python, OpenCV si componentele reutilizabile din proiectul OmniLock.

## Regula de siguranta

Nu se urca in Git fisiere biometrice, parole, tokenuri, baze de date, certificate sau imagini de card SD. Acestea sunt excluse in `.gitignore` si raman numai local.

## Ordinea de lucru

1. Verificare camera cu `rpicam-hello`.
2. Captura minima cu Picamera2.
3. Test servo fara sarcina mecanica.
4. Refactorizarea recunoasterii faciale din OmniLock.
5. Stabilizare temporala a autorizarii faciale.
6. Recunoastere gesturi.
7. Masina de stari si oprire de urgenta.
8. Simulator si teste fara brat mecanic.
9. Integrarea graduala a bratului.

Citeste `PROJECT_CONTEXT.md` inainte de a modifica sau executa codul.
