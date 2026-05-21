# LYNXCommander — public mirror

To jest **publiczne** repozytorium z dokumentami prawnymi, polityką
bezpieczeństwa i certyfikatami dla LYNXCommander. Kod źródłowy aplikacji
pozostaje w prywatnym repozytorium developerskim.

## Co tu znajdziesz

| Plik | Opis |
|---|---|
| [LICENSE](LICENSE) | Licencja oprogramowania (Proprietary, source-available) |
| [PRIVACY.md](PRIVACY.md) | Polityka prywatności (RODO-zgodna, po polsku) |
| [TERMS.md](TERMS.md) | Regulamin korzystania z aplikacji (EULA) |
| [SECURITY.md](SECURITY.md) | Polityka bezpieczeństwa, zgłaszanie podatności |
| [GUIDE.md](GUIDE.md) | Przewodnik dla nowego gracza (instalacja, pierwsze kroki) |
| [cert/](cert/) | Klucz publiczny certyfikatu (LYNXCommander Project, thumbprint poniżej) |

Pełna, renderowana wersja tych dokumentów dostępna na **<https://lynxcommander.pl>**:

- <https://lynxcommander.pl/guide>
- <https://lynxcommander.pl/legal/terms>
- <https://lynxcommander.pl/legal/privacy>
- <https://lynxcommander.pl/legal/license>
- <https://lynxcommander.pl/legal/security>

## Pobieranie aplikacji

Aplikacja jest dostępna do pobrania **po zalogowaniu** na:
<https://lynxcommander.pl/download>

Logowanie przez Discord OAuth — wymaga zaproszenia (invite token) od
kapitana / dowódcy operacji.

## Weryfikacja autentyczności

Wszystkie binarki są podpisane Authenticode SHA-256 self-signed
certyfikatem z thumbprintem:

```
D9 90 1F 2B AC A1 83 1A 7F B9 62 04 60 0C F5 08 50 E4 48 14
```

Cert ważny: **2026-05-17 → 2031-05-17**. Klucz publiczny w [`cert/LYNXCommander_Public.cer`](cert/LYNXCommander_Public.cer).

Po pobraniu instalatora można zweryfikować integralność pliku:

```powershell
Get-FileHash .\LynxInstaller.exe -Algorithm SHA256
```

Wynik musi pasować do wpisu w `SHA256SUMS.txt` na stronie pobierania.

## Zgłaszanie problemów

- **Bug** lub feature request → [Issues](https://github.com/Oriinks/LYNXCommander-public/issues)
- **Podatność bezpieczeństwa** → [Security Advisory](https://github.com/Oriinks/LYNXCommander-public/security/advisories/new)
- **Wsparcie techniczne** → szczegóły w [GUIDE.md](GUIDE.md#10-gdzie-szukać-pomocy)

## Licencja

Software pod licencją proprietary "source-available" — pełna treść
w [LICENSE](LICENSE). Krótko: każdy może pobrać i używać oficjalnych
binarek; modyfikacja, redystrybucja i tworzenie pochodnych wymagają
pisemnej zgody.

---

_Auto-synchronizowane z prywatnego repozytorium developerskiego przy
każdej zmianie dokumentów. Wersja źródłowa w private repo._
