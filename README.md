# LYNXCommander

> Pasywne narzędzie wspomagające percepcję sytuacyjną dla graczy Star Citizen.
> Overlay HUD, komunikacja głosowa Mumble, OCR-owa klasyfikacja IFF (swój /
> wróg / neutralny) na podstawie bazy organizacji RSI.

LYNXCommander **nie modyfikuje** klienta gry, **nie wstrzykuje** kodu, **nie
czyta** pamięci procesu i **nie wysyła** syntetycznego inputu do okna gry.
Szczegóły poniżej w sekcji [Bezpieczeństwo i Anti-cheat](#bezpieczeństwo-i-anti-cheat).

---

## Spis treści

- [Pobieranie](#pobieranie)
- [Weryfikacja sumy kontrolnej (SHA256)](#weryfikacja-sumy-kontrolnej-sha256)
- [Certyfikat](#certyfikat)
- [Bezpieczeństwo i Anti-cheat](#bezpieczeństwo-i-anti-cheat)
- [Kontakt i wsparcie](#kontakt-i-wsparcie)
- [Dokumenty prawne](#dokumenty-prawne)
- [Lore / Status flavor](#lore--status-flavor)

---

## Pobieranie

Aplikację pobieraj **wyłącznie** z oficjalnych źródeł:

1. **Strona LYNXCommander**: https://lynxcommander.pl/ _(strona oficjalna)_
2. **GitHub Releases**: https://github.com/Oriinks/LYNXCommander/releases

Każdy release zawiera:

- `LynxInstaller.exe` — instalator one-click,
- `LynxOverlay-<wersja>-full.nupkg` — paczka Velopack auto-update,
- `RELEASES` — manifest Velopack,
- **`SHA256SUMS.txt`** — sumy kontrolne dla wszystkich plików powyżej.

Nigdy nie pobieraj z innych źródeł — modyfikacje mogą zawierać złośliwy kod.

## Weryfikacja sumy kontrolnej (SHA256)

Po pobraniu pliku zweryfikuj jego integralność w PowerShell:

```powershell
# Oblicz SHA256 pobranego pliku:
Get-FileHash .\LynxInstaller.exe -Algorithm SHA256

# Wynikowy Hash MUSI być zgodny z linią w SHA256SUMS.txt dla tego pliku.
```

Jeśli wartości się nie zgadzają — **NIE uruchamiaj pliku**, pobierz ponownie
i skontaktuj się z nami (sekcja [Kontakt](#kontakt-i-wsparcie)).

## Certyfikat

Aplikacja przy pierwszym uruchomieniu prosi o instalację wewnętrznego
certyfikatu CA do magazynu zaufanych wydawców Windows. Robi to skrypt
`InstallCert.ps1` (wymaga uprawnień administratora — jednorazowo).

Cel: redukcja fałszywych ostrzeżeń SmartScreen i kompatybilność z systemami
anty-cheat klienta gry, zanim wprowadzimy pełny podpis Authenticode (zobacz
[ścieżkę rozwoju w SECURITY.md](SECURITY.md)).

### Thumbprint do weryfikacji

Cert, którym podpisane są binarki LYNXCommander, ma thumbprint SHA-1:

```
D9 90 1F 2B AC A1 83 1A 7F B9 62 04 60 0C F5 08 50 E4 48 14
```

#### Skrócony fingerprint (do szybkiej weryfikacji wizualnej)

```
D990 1F2B ACA1 831A
```

To pierwsze 4 grupy thumbprint-u SHA-1. Publikujemy go w **3 niezależnych miejscach**:

1. Tu, w README.md (źródło prawdy w repo).
2. Strona pobierania https://lynxcommander.pl/download (widget "FINGERPRINT LYNXCommander").
3. Pinned message na oficjalnym Discordzie LYNX.

Jeśli wszystkie trzy źródła pokazują tę samą wartość — masz potwierdzenie, że
żadne pojedyncze konto / domena nie zostało skompromitowane (atakujący musiałby
przejąć GitHub + domenę + Discord jednocześnie).

(Cert ważny: 2026-05-17 → 2031-05-17. Self-signed, CN=LYNXCommander Project. Klucz prywatny
trzymany poza repozytorium w GitHub Actions Secrets.)

Możesz sprawdzić po instalacji w `certmgr.msc` →
*Zaufane główne urzędy certyfikacji* → szukaj wystawcy *LYNXCommander*.

Jeśli aplikacja prosi o instalację certyfikatu z **innym** thumbprintem —
**nie zgadzaj się**, to nie jest oficjalna wersja LYNXCommander.

## Bezpieczeństwo i Anti-cheat

LYNXCommander jest świadomie zaprojektowany tak, by **nie naruszać** regulaminu
Cloud Imperium Games / Roberts Space Industries (CIG/RSI). Konkretnie:

- ❌ **Brak** czytania pamięci procesu Star Citizen.
- ❌ **Brak** DLL injection, hooków, modyfikacji plików gry.
- ❌ **Brak** wysyłania syntetycznego inputu (klawiatury / myszy / pada) do
  okna gry. Funkcja "Anti-AFK", która wcześniej to robiła, została
  **całkowicie usunięta** z aplikacji.
- ❌ **Brak** automatyzacji akcji w grze (aimbot, auto-mine, auto-warp, itp.).
- ✅ OCR to **bierne** odczytywanie pikseli pulpitu (analogicznie jak OBS lub
  Windows accessibility reader).
- ✅ Overlay HUD rysuje grafikę **nad** oknem gry, nie ingeruje w jej
  zawartość.
- ✅ Mumble client jest **standardowym** klientem VoIP — głos płynie przez
  serwer Mumble, nie przez grę.

Aplikacja została zgłoszona do:

- **Microsoft Defender Security Intelligence (WDSI)** — w celu eliminacji
  false-positive,
- **Cloud Imperium Games** — w celu weryfikacji zgodności z regulaminem.

Pełna lista funkcji i ich technicznego opisu — zobacz [`TERMS.md`](TERMS.md)
sekcje 3-4. Polityka zgłaszania podatności — [`SECURITY.md`](SECURITY.md).

## Kontakt i wsparcie

| Kanał | Adres |
|---|---|
| 📧 E-mail | `dominik.borylo@gmail.com` |
| 🐛 GitHub Issues (publiczne) | https://github.com/Oriinks/LYNXCommander-public/issues |
| 🔒 Zgłoszenie podatności | https://github.com/Oriinks/LYNXCommander-public/security/advisories/new |
| 💬 Discord | _(link na stronie https://lynxcommander.pl/)_ |

W sprawach licencyjnych, komercyjnych użyć lub pytań prasowych — pisz
na e-mail powyżej.

## Dokumenty prawne

- **[LICENSE](LICENSE)** — pełna licencja oprogramowania (Proprietary,
  source-available).
- **[PRIVACY.md](PRIVACY.md)** — co zbieramy, gdzie idzie, jak długo trzymamy
  (zgodność z RODO).
- **[TERMS.md](TERMS.md)** — regulamin korzystania z aplikacji (EULA).
- **[SECURITY.md](SECURITY.md)** — jak zgłosić podatność, SLA, safe harbor.

Korzystając z LYNXCommander akceptujesz wszystkie powyższe dokumenty.

---

## Lore / Status flavor

_Materiał lore-owy, używany przez aplikację jako kontekst statusów załogi.
Nie ma znaczenia prawnego._

**Level 3 — Crew → Unit HQ (Kapitan Okrętu).** Dla załogi obsługującej systemy
na dużym okręcie.

- "ENGINE POWER 100%" — Klasa: Capital, Sub-Capital
- "SHIELDS CRITICAL" — Wszystkie
- "QUANTUM DRIVE READY" — Wszystkie
- "RELOADING MAIN BATTERY" — Klasa: Capital

**Level 2 — Unit → Division HQ (Dowódca Dywizji).** Dla pilotów i kapitanów
raportujących do dowódcy grupy.

- "BINGO FUEL" — Doktryna: Szturmowa / Klasa: Fighters
- "ENGAGING TARGET" — Wszystkie militarne
- "MISSION COMPLETE" — Wszystkie
- "DAMAGED - RETREATING" — Wszystkie

**Level 1 — Division → Fleet HQ (Admirał Floty).** Dla dowódców dywizji
raportujących stan całych skrzydeł.

- "LZ SECURED" — Doktryna: Desantowa
- "ZONE RECON COMPLETE" — Doktryna: Zwiad
- "DIVISION COMBAT READY" — Wszystkie
- "HEAVY LOSSES - REQUESTING BACKUP" — Wszystkie
