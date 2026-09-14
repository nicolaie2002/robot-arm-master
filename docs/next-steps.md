# Urmatorii pasi

## Faza 0: verificare sistem

```bash
uname -m
cat /etc/os-release
rpicam-hello --list-cameras
```

Rezultatul asteptat pentru arhitectura este `aarch64`.

## Faza 1: test camera

Se va crea un script minimal Picamera2 care porneste camera, salveaza o imagine si elibereaza resursele. Nu se combina inca cu Flask sau recunoastere faciala.

## Faza 2: test servo

Se testeaza un singur servo pe GPIO18, cu unghiuri limitate si miscari lente. Alimentarea separata este obligatorie pentru mai multe servomotoare.

## Faza 3: refactorizare

Se extrage logica din `pitonu.py` in module mici. Fiecare modul trebuie sa poata fi testat separat si sa aiba o interfata clara.

## Faza 4: experiment

Se vor masura cel putin:

- rata de autorizare corecta;
- rata de respingere a persoanelor necunoscute;
- timpul de la detectie la autorizare;
- acuratetea gesturilor;
- latenta comenzii servo;
- comportamentul la pierderea camerei sau la gest invalid.
