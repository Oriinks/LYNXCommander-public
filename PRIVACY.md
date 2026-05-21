# Polityka prywatności — LYNXCommander

_Wersja: 1.0 · Data wejścia w życie: 2026-05-17_

Niniejszy dokument opisuje, jakie dane LYNXCommander zbiera, gdzie je przetwarza,
komu je przekazuje i jak długo je przechowuje. Pisaliśmy go po polsku i w prostym
języku, żeby był zrozumiały dla wszystkich graczy.

W razie pytań — kontakt znajdziesz w [README.md](README.md#kontakt-i-wsparcie).

---

## TL;DR (skrót)

- **OCR ekranu** odbywa się **lokalnie** na Twoim komputerze. Zrzuty pikselowe
  **nie są** wysyłane na nasze serwery. Wysyłamy tylko **sparsowane nazwy graczy**
  (tekst) w celu klasyfikacji IFF (swój / wróg / neutralny).
- **Voice (Mumble)** to klasyczne VoIP — głos leci przez serwer Mumble do innych
  graczy w Twoim kanale. **Nie nagrywamy** rozmów.
- **Token uwierzytelniający** trzymamy **lokalnie i zaszyfrowany kluczem Twojego
  konta Windows** (DPAPI). Nie wysyłamy go nigdzie poza naszym API.
- **Telemetria błędów** (crash, wyjątki) idzie na nasze kanały Discord — tylko
  Twoje logi techniczne, bez treści rozmów Mumble.

---

## 1. Kto jest administratorem danych

Administratorem danych jest **LYNXCommander Team** (zespół projektu, dane
kontaktowe w [README.md](README.md#kontakt-i-wsparcie)).

Dane są przetwarzane zgodnie z RODO (Rozporządzenie Parlamentu Europejskiego
i Rady (UE) 2016/679, dalej "RODO"). Twoje prawa wynikające z RODO opisane są
w sekcji 8.

## 2. Jakie dane zbieramy i przetwarzamy

### 2.1. Dane konta

| Dane | Skąd pochodzą | Cel | Lokalizacja |
|---|---|---|---|
| Handle RSI (Star Citizen) | Wpisujesz w aplikacji / OAuth | Identyfikacja gracza, mapowanie do organizacji | Backend LYNXCommander |
| Discord User ID i nick | OAuth Discord (jeśli używasz logowania) | Identyfikacja gracza, członkostwo w grupie | Backend LYNXCommander |
| Token JWT | Generowany przez backend po zalogowaniu | Utrzymanie sesji | **Lokalnie** w `%APPDATA%\LynxOverlay\token.dat`, szyfrowany DPAPI (klucz Twojego konta Windows) |

### 2.2. Dane gameplay (przesyłane między klientem a backendem)

| Dane | Cel |
|---|---|
| Status załogi (gotowy / w drodze / ranny itp.) | Synchronizacja stanu drużyny w czasie rzeczywistym (SignalR) |
| Aktualny kanał Mumble (party / lobby) | Auto-join / przełączanie kanałów głosowych |
| Trafienia OCR — **tylko sparsowane handle'e graczy** | Klasyfikacja IFF (swój / wróg) na podstawie bazy organizacji |
| Manualne oznaczenia (np. "to jest wróg") | Wzbogacanie cache'a graczy |
| Skany skał (mining tool) | Logowanie znalezisk na kanale Discord (zarząd zrzutek) |

**Czego NIE przesyłamy:**
- Surowych zrzutów ekranu / pikselowych snapshotów. OCR działa lokalnie,
  zrzut jest natychmiast po zeskanowaniu odrzucany z pamięci, nigdy nie zapisywany
  na dysk (chyba że włączysz `ScanImageLogEnabled` w settings — wtedy tylko
  lokalnie, dla Twojego debugowania).
- Twojej pozycji w grze (poza tym, co OCR ekranowy wykryje jako tekst — jeśli
  np. w HUD-zie SC widać nazwę planety, to słowo trafia do parsera; nie czytamy
  pamięci procesu gry).
- Zawartości głosowej z Mumble (sekcja 2.4 poniżej).

**Monitor obecności gry:** Aplikacja sprawdza co 5 sekund czy proces
`StarCitizen.exe` jest uruchomiony (przez Windows API
`Process.GetProcessesByName`). Robi to **lokalnie**, nie wysyła tej
informacji nigdzie. Cel: automatyczne pokazywanie/ukrywanie HUD oraz
jednorazowe ostrzeżenie o trybie ekranu *Borderless vs Exclusive Fullscreen*.
Nie czytamy ani jednej linijki pamięci procesu gry — tylko **nazwa**
procesu, której Windows udostępnia każdej aplikacji bez specjalnych
uprawnień.

### 2.3. Telemetria i logi

| Dane | Kiedy | Dokąd | Cel |
|---|---|---|---|
| Logi wyjątków aplikacji (stacktrace) | Automatycznie przy crashu / wyjątku | Discord webhook (kanał techniczny LYNX) | Diagnostyka błędów |
| Post-mortem BSOD/freeze | Po nieczystym ubiciu procesu, przy następnym starcie | Discord webhook | Wykrywanie problemów stabilności |
| ZIP z logami (manualny przycisk "Wyślij raport") | Tylko kiedy świadomie klikniesz | Discord webhook | Wsparcie techniczne |
| `mumble_debug.txt` | Lokalnie, zawsze | `%APPDATA%\LynxOverlay\` lub katalog aplikacji | Debug Mumble (rotowany lokalnie, nigdzie nie wysyłany automatycznie) |
| Settings (preferencje overlay) | Lokalnie | `%APPDATA%\LynxOverlay\settings.json` | Twoje preferencje, niewysyłane na serwer |

**Webhooki Discord są publicznymi URL-ami "write-only"** — kanały są prywatne dla
zespołu LYNX. Nikt z zewnątrz nie czyta ich zawartości poza moderatorami.

### 2.4. Komunikacja głosowa (Mumble)

LYNXCommander zawiera klienta protokołu **Mumble** (open source, https://www.mumble.info).

- Twój głos jest enkodowany lokalnie (Opus) i wysyłany **bezpośrednio** do
  serwera Mumble (operowanego przez LYNX), który dystrybuuje go do innych
  graczy w tym samym kanale.
- **Nie nagrywamy** rozmów. Mumble jest stateless w warstwie audio — gdy pakiet
  jest dostarczony do odbiorcy, znika.
- Logi serwera Mumble zawierają metadane: kto kiedy dołączył, do jakiego kanału,
  z jakiego IP. Te logi są **rotowane co 7 dni** i służą wyłącznie diagnostyce.

### 2.5. Dane organizacyjne (z RSI API)

Backend LYNXCommander synchronizuje **publiczne** dane członków zarejestrowanych
w aplikacji organizacji Star Citizen z oficjalnego API Roberts Space Industries
(spectrum / robertsspaceindustries.com), raz dziennie ~03:00.

- Pobieramy: handle, awatar URL, ranga w organizacji.
- Nie pobieramy żadnych prywatnych danych konta RSI.
- Te dane są **publicznie dostępne** na profilu Spectrum każdego gracza.

## 3. Komu udostępniamy dane

| Odbiorca | Co dostaje | Podstawa prawna |
|---|---|---|
| Backend LYNXCommander (nasz VPS w UE) | Wszystkie dane z sekcji 2.1, 2.2 | Wykonanie umowy (Twoja zgoda na korzystanie) |
| Discord (Discord Inc., USA) | Telemetria błędów (przez webhooki) | Uzasadniony interes administratora — diagnostyka |
| Roberts Space Industries (CIG) | Publiczne zapytania API o członków org | Brak Twoich danych osobowych w zapytaniu |
| Serwer Mumble (operowany przez LYNX) | Pakiety głosowe | Wykonanie usługi |

**NIE sprzedajemy** Twoich danych. **NIE udostępniamy** ich reklamodawcom ani
żadnej innej stronie trzeciej poza wymienionymi powyżej.

## 4. Lokalizacja przetwarzania i transfery międzynarodowe

- Backend LYNX i serwer Mumble są hostowane w **Unii Europejskiej**.
- Discord (telemetria błędów) — USA. Transfer odbywa się na podstawie
  **standardowych klauzul umownych (SCC)** zgodnie z RODO.
- RSI API — USA. Wysyłamy tylko anonimowe zapytania publiczne (handle
  organizacji), brak transferu Twoich danych osobowych.

## 5. Czas przechowywania (retention)

| Dane | Czas przechowywania |
|---|---|
| Dane konta (handle, Discord ID) | Do momentu usunięcia konta lub żądania usunięcia |
| Logi sesji gameplay | 30 dni od ostatniej aktywności |
| Cache profili graczy z RSI | 90 dni od ostatniego widzenia / odświeżenia |
| Telemetria błędów na Discord | Wg polityki retencji kanału Discord (max 1 rok, zwykle krócej) |
| Logi serwera Mumble | 7 dni |
| Lokalne pliki (`%APPDATA%\LynxOverlay\`) | Do momentu odinstalowania aplikacji lub ręcznego usunięcia |

## 6. Bezpieczeństwo danych

- Token JWT lokalnie: **szyfrowanie DPAPI** scope CurrentUser (klucz Twojego
  konta Windows, niedostępny dla innych użytkowników komputera).
- Komunikacja z backendem: **HTTPS/TLS 1.2+**.
- Komunikacja Mumble: **TLS** na kontrolnym kanale, opus-over-UDP/TCP-tunnel
  na voice.
- Hasła użytkowników (jeśli dotyczy): hashowane bcrypt po stronie backendu,
  nigdy nie trzymane w plaintext.

## 7. OCR i automatyzacja — co LYNXCommander **nie robi**

Dla pełnej transparentności i zgodności z regulaminem Star Citizen:

- **NIE czytamy pamięci procesu Star Citizen.** Brak `ReadProcessMemory`,
  brak DLL injection, brak hooków.
- **NIE wysyłamy syntetycznego inputu** (klawiatury / myszy) do okna gry.
  Funkcja "Anti-AFK", która wcześniej to robiła, została **całkowicie usunięta**
  z aplikacji w celu zgodności z regulaminem CIG.
- **NIE modyfikujemy** klienta gry, plików gry, ani żadnej części silnika.
- OCR to **bierne odczytywanie ekranu** (tak samo jak OBS / Windows accessibility
  reader) — z technicznego punktu widzenia bliżej mu do screenshota niż do
  hooka anti-cheata.

## 8. Twoje prawa (RODO)

Masz prawo:

- (a) dostępu do swoich danych — możesz poprosić o kopię,
- (b) sprostowania — jeśli coś jest błędne,
- (c) usunięcia ("prawo do bycia zapomnianym"),
- (d) ograniczenia przetwarzania,
- (e) przenoszenia danych do innego administratora,
- (f) sprzeciwu wobec przetwarzania,
- (g) wycofania zgody w dowolnym momencie (bez wpływu na legalność
  przetwarzania sprzed wycofania).

Aby skorzystać z któregoś z praw, napisz na adres kontaktowy z README.md —
odpowiemy w ciągu 30 dni.

Masz też prawo wnieść skargę do **Prezesa Urzędu Ochrony Danych Osobowych**
(https://uodo.gov.pl).

## 9. Cookies i podobne technologie

Aplikacja desktopowa LYNXCommander **nie używa cookies**. Token JWT
przechowywany lokalnie nie jest cookie — to plik aplikacji.

Strona internetowa LYNXCommander może używać minimalnych cookies technicznych
(sesja). Te są opisane na samej stronie www.

## 10. Zmiany polityki prywatności

Możemy aktualizować ten dokument. Wersja i data wejścia w życie są na górze
pliku. Istotne zmiany ogłosimy na Discordzie LYNX i przez powiadomienie
w aplikacji.

---

_Ostatnia aktualizacja: 2026-05-17. Wersja źródłowa: w repo GitHub w pliku
[`PRIVACY.md`](PRIVACY.md)._
