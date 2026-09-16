# Polityka prywatności — LYNXCommander

_Wersja: 1.2 · Data wejścia w życie: 2026-09-16_

Niniejszy dokument opisuje, jakie dane LYNXCommander zbiera, gdzie je przetwarza,
komu je przekazuje i jak długo je przechowuje. Pisaliśmy go po polsku i w prostym
języku, żeby był zrozumiały dla wszystkich graczy.

W razie pytań napisz na **dominik.borylo@gmail.com**.

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
- **Monitor dziennika gry (`Game.log`)** jest **domyślnie wyłączony (opt-in)**.
  Po włączeniu czytamy **lokalnie** plik tekstowy, który **sama gra** zapisuje na
  dysku, i synchronizujemy w drużynie tylko **wyprowadzony stan** (party / lider /
  „powalony" / „w menu"). **Lokalizacja** wymaga **drugiej, osobnej zgody**.
  Nie czytamy pamięci procesu gry. Szczegóły: sekcja 2.6.
- **Twoje dane są pod ręką:** w profilu (sekcja „Twoje dane") pobierzesz komplet danych
  konta jednym kliknięciem i tam też **usuniesz konto** — bez pisania do nas. Sekcja 8.
- **Organizacje widzą tylko swoich członków.** Gracz bez organizacji jest niewidoczny dla
  innych — widzą go wyłącznie osoby, które sam dodał do znajomych. Sekcja 2.7.

---

## 1. Kto jest administratorem danych

Administratorem danych jest **LYNXCommander Team** — zespół prowadzący projekt.
Kontakt w sprawach danych osobowych: **dominik.borylo@gmail.com**.

Dane są przetwarzane zgodnie z RODO (Rozporządzenie Parlamentu Europejskiego
i Rady (UE) 2016/679, dalej "RODO"). Twoje prawa wynikające z RODO opisane są
w sekcji 8.

## 2. Jakie dane zbieramy i przetwarzamy

### 2.1. Dane konta

| Dane | Skąd pochodzą | Cel | Lokalizacja |
|---|---|---|---|
| Handle RSI (Star Citizen) | Wpisujesz w aplikacji / OAuth | Identyfikacja gracza, mapowanie do organizacji | Backend LYNXCommander |
| Discord User ID i nick | OAuth Discord (jeśli używasz logowania) | Identyfikacja gracza, członkostwo w grupie | Backend LYNXCommander |
| Token JWT (access) | Generowany przez backend po zalogowaniu | Utrzymanie sesji (15 min lifetime) | **Lokalnie**: w aplikacji desktop — `%LOCALAPPDATA%\LynxOverlay\tokens.dat`, szyfrowany DPAPI; w przeglądarce — HttpOnly cookie `lynx_access` |
| Refresh token | Generowany razem z access tokenem | Odświeżanie wygasłego access (30 dni lifetime) | **Lokalnie** jak access; w bazie backendu trzymamy tylko **SHA-256 hash** (nie sam token) |

### 2.1.1. Co jest w bazie po stronie backendu (tabela `RefreshTokens`)

Po wdrożeniu refresh token rotation (RTR) backend musi trzymać metadane sesji
do detekcji kompromitacji tokena. W bazie znajdują się:

| Pole | Cel | Retention |
|---|---|---|
| Hash refresh tokena (SHA-256) | Walidacja przy każdym /refresh | 7 dni po wygaśnięciu (revoked tokeny zachowujemy do audytu) |
| `Family` (GUID sesji) | Grupowanie tokenów z jednego logowania, family-wide revoke przy detekcji reuse | Jak wyżej |
| `ClientType` ("web" / "overlay") | Telemetria, przyszły UI "aktywne sesje" | Jak wyżej |
| `UserAgent` (pierwsze 512 znaków) | Audit log — pomoc przy "kto i kiedy się zalogował" | 7 dni po wygaśnięciu |
| `IpAddress` | Audit log — wykrywanie podejrzanych logowań | 7 dni po wygaśnięciu, anonimizujemy po incydencie |

**NIE** trzymamy plaintext'u refresh tokena — atakujący który zdobędzie dump
naszej bazy nie będzie w stanie zalogować się jako Ty (bez znajomości oryginalnego
plaintext'u). Cleanup worker (background service) usuwa wygasłe rekordy raz na dobę.

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
| Automatyczny raport błędu (typ wyjątku, komunikat, stacktrace, wersja aplikacji) | Automatycznie, gdy w aplikacji wystąpi błąd | Serwer LYNX (baza telemetrii) | Wykrywanie błędów, o których nikt nie zgłasza, i porównywanie stabilności wydań |
| Końcówka logu dołączona do zgłoszenia błędu | Tylko gdy wysyłasz zgłoszenie z zaznaczonym polem „Dołącz logi diagnostyczne" (domyślnie zaznaczone, możesz odznaczyć) | Serwer LYNX → zgłoszenie w OpenProject | Ustalenie przyczyny zgłoszonego problemu |
| `mumble_debug.txt` | Lokalnie, zawsze | `%APPDATA%\LynxOverlay\` lub katalog aplikacji | Debug Mumble (rotowany lokalnie, nigdzie nie wysyłany automatycznie) |
| Settings (preferencje overlay) | Lokalnie | `%APPDATA%\LynxOverlay\settings.json` | Twoje preferencje, niewysyłane na serwer |

**Webhooki Discord są publicznymi URL-ami "write-only"** — kanały są prywatne dla
zespołu LYNX. Nikt z zewnątrz nie czyta ich zawartości poza moderatorami.

**Automatyczne raporty błędów** zawierają wyłącznie dane techniczne: typ wyjątku,
komunikat, ślad stosu, nazwę modułu i wersję aplikacji. Przed wysyłką usuwamy z nich
tokeny, hasła, klucze API i adresy webhooków. Powtórki tego samego błędu są zliczane
i wysyłane zbiorczo, a liczba różnych błędów raportowanych w jednej sesji jest ograniczona —
raport nie służy do śledzenia Twojej aktywności, tylko do wykrycia usterki. To samo
dotyczy końcówki logu dołączanej do zgłoszenia: jest redagowana z sekretów i przycinana,
a wysyłasz ją świadomie razem ze zgłoszeniem.

### 2.3.1. Telemetria skanera (IFF i Rock Scanner)

Aby poprawiać **dokładność i skuteczność** rozpoznawania (OCR + dopasowanie nazw/sygnatur),
backend zapisuje **ustrukturyzowane dane skanów** w bazie (tabele `IffScanLog`/`IffScanMatch`/
`IffScanColorWord`/`IffScanFeedback` oraz `RockScanLog`/`RockScanMatch`):

| Dane | Dokąd | Cel |
|---|---|---|
| Tekst odczytany przez OCR (do 2000 znaków) | Baza backendu | Retrospekcja i symulacja progów dopasowania |
| Dopasowane nicki (IFF) wraz z klasyfikacją, similarity, użytym progiem, źródłem | Baza backendu | Strojenie skuteczności rozpoznawania znajomy/wróg |
| Wykryte kolory nazw z HUD | Baza backendu | Korelacja koloru z relacją (jakość wskazówek) |
| Twoja ocena wyniku (✓ trafione / ✗ błędne) | Baza backendu | Pomiar realnej dokładności (prawda bazowa) |
| Minerały, sygnatury, ilości SCU (Rock) | Baza backendu | Poprawa rozpoznawania skał |
| Wersja aplikacji, identyfikator skanu, Twój identyfikator konta | Baza backendu | Analiza per-wersja, łączenie zrzutu z danymi |

- **Zrzut ekranu skanu** trafia **wyłącznie** na prywatny webhook Discord zespołu (jak w 2.3),
  a jego nazwa pliku to identyfikator skanu — pozwala technicznie powiązać obraz z wierszem danych.
- **Dane osób trzecich:** tekst OCR może zawierać **publiczne handle innych graczy** widoczne na
  Twoim ekranie w grze. Są to dane publiczne (widoczne w grze i na profilu Spectrum), używane
  wyłącznie do analizy skuteczności narzędzia, nieudostępniane na zewnątrz.
- **Możesz to wyłączyć:** Ustawienia → „Telemetria skanera". Po wyłączeniu skany nie są zapisywane
  do analizy (na Discord nadal może iść sam zrzut, zależnie od osobnego ustawienia logowania).

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

### 2.6. Monitorowanie dziennika gry (`Game.log`)

**Funkcja opcjonalna, domyślnie WYŁĄCZONA (opt-in).** Włączasz ją ręcznie w
ustawieniach (`Monitorowanie logów gry`), a udostępnianie lokalizacji to **drugi,
osobny przełącznik** (`Udostępnianie lokalizacji z logów`).

Star Citizen sam zapisuje na dysku plik tekstowy `Game.log` (dziennik
deweloperski). Po Twojej zgodzie Aplikacja **czyta ten plik lokalnie na bieżąco**
i wyprowadza z niego stan, który synchronizuje w obrębie Twojej drużyny (tak jak
statusy załogi):

| Co wyprowadzamy z `Game.log` | Co trafia do drużyny (backend → członkowie party) | Wymaga zgody |
|---|---|---|
| Czy jesteś w party w grze / kto jest party-liderem | tak/nie + flaga lidera | Monitorowanie logów |
| Stan „powalony" (INCAP) | flaga zdrowia (0 = OK, 1 = INCAP) | Monitorowanie logów |
| „W menu" (gracz w menu głównym gry) | flaga statusu „MENU" | Monitorowanie logów |
| Bieżąca lokalizacja (stacja / planeta / „w przestrzeni") | czytelna nazwa miejsca — widoczna tylko dla ról z uprawnieniem `intel:view_locations` | **Osobna** zgoda (udostępnianie lokalizacji) |
| Region + numer serwera sesji (shard) | region + adres serwera — widoczne tylko dla ról z uprawnieniem `intel:view_server_details` | Monitorowanie logów |

Dostęp do dwóch ostatnich pozycji jest **podwójnie ograniczony**: musisz je udostępnić
(zgoda po Twojej stronie) **i** odbiorca musi mieć odpowiednie uprawnienie nadane przez
zarząd organizacji. Sama nazwa lokacji jest tłumaczona z kodu z logu na czytelną nazwę
przy użyciu słownika pobieranego z serwera — nie wysyłamy w tym celu żadnych dodatkowych
danych o Tobie; gdy kod jest nam nieznany, wysyłany jest **wyłącznie sam kod lokacji**
(np. `RR_JP_NyxPyro`), bez powiązania z Twoim kontem, żeby dopisać właściwą nazwę.

**Czego NIE robimy:**
- **Nie czytamy pamięci procesu gry** — to wyłącznie odczyt pliku tekstowego,
  który gra sama tworzy (dostęp do niego ma każda aplikacja na Twoim koncie
  Windows). Brak `ReadProcessMemory`, brak hooków (zobacz sekcję 7).
- **Nie wysyłamy surowej treści `Game.log`** na serwer — tylko wyprowadzone,
  wymienione wyżej flagi/nazwy.
- **Nie udostępniamy lokalizacji bez Twojej osobnej zgody.** Po jej wyłączeniu
  pole lokalizacji jest puste.
- Gdy wyłączysz monitorowanie, serwis natychmiast przestaje czytać plik i czyści
  zsynchronizowany stan.

Cel: świadomość sytuacyjna drużyny (gdzie są członkowie, kto jest powalony, kto
w menu) — bez ręcznego raportowania.

### 2.7. Organizacje, znajomi i widoczność

Z LYNXCommandera korzysta wiele organizacji naraz. Każda widzi **wyłącznie własne dane**:
skład, operacje, kontrakty, misje, pokoje głosowe i słowniki innej organizacji są dla niej
niedostępne.

| Kto | Co widzi o Tobie |
|---|---|
| Członkowie Twojej organizacji | nick, awatar, rolę, status online, statusy w pokojach, a przy zgodach z sekcji 2.6 — stan z dziennika gry i (dla uprawnionych) lokalizację |
| Właściciel i zarząd organizacji | powyższe oraz Twoje podanie rekrutacyjne (treść, handle RSI), jeśli je złożyłeś |
| Twoi znajomi (osoby, które dodałeś i które Cię dodały) | nick, awatar, status online, skrót Twojej organizacji |
| Organizacje sojusznicze | tylko to, co Twoja organizacja świadomie im udostępniła (np. kanał głosowy) |
| Wszyscy odwiedzający stronę | nazwa i herb organizacji — wyłącznie gdy organizacja sama się na to zgodziła |

**Gracz bez organizacji** nie jest widoczny dla nikogo poza swoimi znajomymi.
Znajomość wymaga zgody obu stron; każda strona może ją zakończyć w dowolnej chwili.
Zaproszenie wysyła się po dokładnym nicku — lista znajomych nie pozwala przeglądać kont.

**Właściciel organizacji** decyduje o jej wyglądzie, publicznej wizytówce i sojuszach, może
przekazać organizację innemu członkowi albo ją usunąć. Po usunięciu organizacji wszyscy
członkowie stają się graczami bez organizacji, a dane operacyjne organizacji przestają być
dostępne dla kogokolwiek.

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
| Dane konta (handle, Discord ID) | Do momentu usunięcia konta (w profilu) lub żądania usunięcia |
| Twój wkład we wspólną historię (kontrakty, oceny, przebiegi misji, wpisy w dziennikach pokoi) | Tak długo jak historia organizacji — po usunięciu konta **bez Twojego nicku**, z podpisem „Usunięte konto" |
| Znajomości, podania rekrutacyjne, hangar, sesje | Do usunięcia przez Ciebie albo usunięcia konta |
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
  brak DLL injection, brak hooków w procesie gry. Skróty klawiszowe (np. push-to-talk)
  działają przez systemowe hooki Windows, które **wyłącznie odczytują**, czy klawisz
  jest wciśnięty — nie zapisują wpisywanego tekstu, nie wysyłają go nigdzie i nie
  zmieniają tego, co trafia do gry.
- **NIE wysyłamy syntetycznego inputu** do okna gry — z jednym wyjątkiem: opcjonalna
  funkcja „Anti-AFK", domyślnie wyłączona i włączana świadomie przez gracza, opisana
  w Regulaminie (sekcja 3a).
- **NIE modyfikujemy** klienta gry, plików gry, ani żadnej części silnika.
- OCR to **bierne odczytywanie ekranu** (tak samo jak OBS / Windows accessibility
  reader) — z technicznego punktu widzenia bliżej mu do screenshota niż do
  hooka anti-cheata.
- **Monitorowanie `Game.log` (opcjonalne) to odczyt PLIKU TEKSTOWEGO**, który
  sama gra zapisuje na dysku — **nie** czytanie pamięci procesu. Każda aplikacja
  na Twoim koncie Windows ma do tego pliku dostęp; my tylko go „tailujemy" (jak
  `tail -f`) i parsujemy tekst.

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

**Bez pisania do nas** — w aplikacji internetowej, w profilu, sekcja „Twoje dane":

- **Pobierz moje dane** — plik JSON z kompletem danych, które backend trzyma o Twoim
  koncie: profil, organizacja, sesje (typ klienta, adres IP, daty), znajomi, hangar,
  podania, kontrakty i oceny, udział w misjach i operacjach, wpisy w dziennikach pokoi,
  skany i zgłoszone błędy nakładki. To realizuje prawo dostępu (a) i przenoszenia (e).
- **Usuń konto** — natychmiast i nieodwracalnie usuwamy: powiązanie z Discordem, nick,
  awatar, tytuł, hangar, znajomych, sesje, podania, zaproszenia, członkostwo w
  organizacji i w pokojach oraz rejestrację na serwerze głosowym. Historia, którą
  współtworzyłeś z innymi (kontrakty, oceny, przebiegi misji), zostaje, bo należy także
  do nich — ale **bez Twojego nicku**. Telemetria traci powiązanie z Twoim kontem.
  Ponowne zalogowanie tym samym kontem Discord zakłada nowe, puste konto.
  Jeśli jesteś właścicielem organizacji, najpierw przekaż ją innemu członkowi albo ją usuń
  (Ustawienia organizacji). To realizuje prawo do usunięcia (c).
- Sprostowanie (b): nick, specjalizację i hangar zmieniasz w profilu.

Pozostałe prawa (ograniczenie, sprzeciw) oraz wszystko, czego nie da się zrobić w aplikacji,
realizujemy na prośbę wysłaną na **dominik.borylo@gmail.com** — odpowiemy w ciągu 30 dni.
Pliki nakładki na Twoim komputerze (`%APPDATA%\LYNXCommander`) usuwasz sam,
odinstalowując aplikację.

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

_Ostatnia aktualizacja: 2026-09-16 (v1.2 — samoobsługowy eksport i usunięcie konta,
sekcja 2.7: organizacje, znajomi i widoczność; v1.1 — sekcja 2.6: monitor `Game.log`). Wersja źródłowa: w repo GitHub w pliku
[`PRIVACY.md`](PRIVACY.md)._
