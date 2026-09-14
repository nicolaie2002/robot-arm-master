# Reguli pentru date si secrete

Proiectul foloseste date biometrice, deci repository-ul trebuie sa ramana privat si sa contina numai cod si documentatie care nu identifica persoane.

Nu comite:

- `encodings.pickle`;
- imagini din `dataset/`;
- fisiere `.db`, `.sqlite` sau `.sqlite3`;
- `.env`;
- parole SMTP;
- `SECRET_KEY` real;
- certificate sau chei private;
- tokenuri GitHub/Azure;
- imagini `.img` ale cardului SD.

Daca o parola a fost inclusa vreodata in cod sau trimisa intr-un chat, considera-o compromisa: revoc-o si genereaza una noua.
