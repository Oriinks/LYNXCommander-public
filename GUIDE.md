# Przewodnik dla nowego gracza — LYNXCommander

_Ostatnia aktualizacja: 2026-11-11_

Cześć! Trafiłeś tu, bo ktoś Ci nas polecił albo znalazłeś nas przez organizację Star Citizen. Ten dokument prowadzi Cię od założenia konta, przez instalację nakładki, po pierwszą grę z innymi.

**Czytanie zajmie ~5 minut. Konfiguracja: ~10 minut.**

---

## Spis treści

1. [Co to jest LYNXCommander](#1-co-to-jest-lynxcommander)
2. [Czego potrzebujesz](#2-czego-potrzebujesz)
3. [Krok 1 — Pierwsze logowanie na stronie](#3-krok-1--pierwsze-logowanie-na-stronie)
4. [Krok 2 — Pobranie i instalacja](#4-krok-2--pobranie-i-instalacja)
5. [Krok 3 — Pierwsze uruchomienie aplikacji](#5-krok-3--pierwsze-uruchomienie-aplikacji)
6. [Krok 4 — Krótka konfiguracja (opcjonalna)](#6-krok-4--krótka-konfiguracja-opcjonalna)
7. [Krok 5 — Pierwsza sesja z drużyną](#7-krok-5--pierwsza-sesja-z-drużyną)
8. [Główne funkcje w pigułce](#8-główne-funkcje-w-pigułce)
9. [Częste problemy](#9-częste-problemy)
10. [Gdzie szukać pomocy](#10-gdzie-szukać-pomocy)
11. [Bezpieczeństwo i anti-cheat](#11-bezpieczeństwo-i-anti-cheat)

---

## 1. Co to jest LYNXCommander

LYNXCommander to **pasywne narzędzie wspomagające** dla graczy Star Citizen: grających solo, w doraźnych grupach i w organizacjach. Składa się z:

- 🌐 **Strony** (`lynxcommander.pl`): profil, organizacje i rekrutacja, planowanie misji i operacji, kontrakty, hangar, pobranie nakładki
- 🖥️ **Nakładki na Windows**: panele HUD i MobiGlass nad grą (skanery skał i IFF, głos, grupy, ready check, kontrakty, misje, beacony) ze zintegrowanym klientem głosowym Mumble
- 🎙️ **Serwera Mumble**: głos o niskim opóźnieniu dla grup i organizacji

> 💡 **Krótko:** Discord do rozmowy, LYNX do koordynacji w grze. Strona służy do konta, organizacji i planowania, a skanery, głos i panele działają w nakładce.

**Co LYNX robi:** pokazuje status ludzi obok gry, rozpoznaje graczy skanerem IFF, daje radio z niską latencją i łączy organizację z grupą operacyjną.

**Czego LYNX NIE robi:** nie modyfikuje gry, nie wysyła klawiszy do gry, nie automatyzuje akcji i nie pomaga celować. Pełna lista jest w sekcji 11 i w [Regulaminie](/legal/terms), sekcja 3.

---

## 2. Czego potrzebujesz

Zanim ruszysz:

| Element | Dlaczego |
|---|---|
| **Konto Discord** | Logujesz się przez Discord OAuth (jedyna metoda) |
| **Konto Star Citizen / handle RSI** | LYNX synchronizuje członków Twojej organizacji z RSI |
| **Link zaproszenia** (nieobowiązkowy) | Tylko jeśli chcesz od razu wejść do konkretnej organizacji. Bez niego zakładasz konto solo |
| **Windows 10 (build 17763+) lub Windows 11, architektura x64** | Aplikacja jest natywnie windowsowa |
| **~500 MB wolnego miejsca** | Self-contained .NET 8 + zależności |
| **Mikrofon** | Do gadania przez Mumble |
| **Star Citizen w trybie Borderless Fullscreen** | Overlay rysuje nad grą — w Exclusive Fullscreen nie będzie widoczny |

### Link zaproszenia: skąd go wziąć i czy jest potrzebny

Link zaproszenia wystawia organizacja, do której dołączasz: jej zarząd albo
ranga z prawem zapraszania (na stronie w Bazie organizacji, w nakładce
w MobiGlass, w kartach REKRUTACJA i ZESPÓŁ). Link wygląda tak:

```
https://lynxcommander.pl/login?invite=LYNX-A1B2C3D4
```

Link działa jeden raz i jest ważny 7 dni. Kto zaloguje się przez niego po raz
pierwszy, od razu jest członkiem tej organizacji, bez podania.

**Nie masz linku? Nie jest potrzebny, żeby zacząć.** Zaloguj się przez Discorda
bez linku, a na ekranie startowym wybierzesz:

- **Gram sam** (solo): nakładka, skanery, crafting i doraźne grupy z głosem,
  bez organizacji;
- **Chcę dołączyć do organizacji**: lista organizacji z otwartym naborem.
  Wysyłasz podanie, a rozpatrują je osoby wyznaczone w organizacji. Do czasu
  przyjęcia grasz jak solo;
- **Zakładam organizację**: kreator nowej organizacji.

Link albo sam kod (np. `LYNX-A1B2C3D4`) wkleisz też później, na ekranie
startowym albo na stronie Rekrutacja. Możesz też otworzyć link i zalogować się
przez niego jeszcze raz: konto bez organizacji zostanie dopisane do organizacji,
która wystawiła link. Konta, które należy już do innej organizacji, link nie
przenosi; najpierw trzeba z niej wyjść.

Po pierwszym logowaniu Twoje konto Discord jest zapamiętane i kolejne
logowania nie wymagają linku.

---

## 3. Krok 1 — Pierwsze logowanie na stronie

1. Otwórz w przeglądarce https://lynxcommander.pl/login albo link
   zaproszenia, jeśli dostałeś go od organizacji:
   ```
   https://lynxcommander.pl/login?invite=LYNX-A1B2C3D4
   ```

2. Zobaczysz ekran "C&C Net Gateway" z dużym przyciskiem **"Zainicjuj
   Bezpieczne Łącze (Discord)"** — kliknij.

3. Discord OAuth poprosi o autoryzację. Zgódź się.

4. Po powrocie z Discorda konto z linku zaproszenia trafia do panelu
   organizacji. Konto bez organizacji trafia na ekran startowy: nick z gry,
   wybór drogi (solo, nabory organizacji albo własna organizacja)
   i instalacja nakładki.

> ⚠️ Jeśli zobaczysz błąd, że **token rekrutacyjny wygasł, został zużyty lub
> jest nieprawidłowy**, link był już użyty albo minęło 7 dni. Poproś organizację
> o nowy albo zaloguj się bez linku i dołącz przez nabór.

---

## 4. Krok 2 — Pobranie i instalacja

Instalator pobierzesz bez logowania:

1. Otwórz https://lynxcommander.pl/pobierz (po zalogowaniu znajdziesz tę stronę też w menu, sekcja **Informacje**, **„Pobieranie nakładki”**).
2. Kliknij **„Pobierz LynxInstaller.exe”**. Plik zapisze się w `Pobrane`/`Downloads`.
3. Uruchom pobrany plik. Strona pobierania ma też listę „Instalacja krok po kroku” i sama pokaże, że nakładka połączyła się z Twoim kontem.

> ⚠️ **Windows SmartScreen może ostrzec przy pierwszym uruchomieniu.**
> Zobaczysz okno *„Windows zapobiegł uruchomieniu nierozpoznanej aplikacji”*.
> To normalne: nasz certyfikat nie jest na globalnej liście Microsoftu.
> Kliknij **„Więcej informacji”** → **„Uruchom mimo to”**. Podpis możesz sprawdzić sam (opis na stronie pobierania i w sekcji 11).

W instalatorze:

1. Wybierz język (polski, angielski albo czeski).
2. **Zaznacz** *„Akceptuję Regulamin i Politykę Prywatności”* (dokumenty otworzysz i przeczytasz przed akceptacją).
3. Przejdź dalej przyciskiem głównym, aż dojdziesz do **„ZAINSTALUJ”**.

Instalator pobiera aplikację, sprawdza jej podpis cyfrowy (bez UAC) i uruchamia LYNX Commander. Nakładka aktualizuje się potem sama.

---

## 5. Krok 3 — Pierwsze uruchomienie aplikacji

Są dwie ścieżki:

### 🟢 Ścieżka szybka (zalecana) — „Otwórz w aplikacji”

Jeśli jesteś już zalogowany na **lynxcommander.pl**, wróć na **stronę pobierania**. Zobaczysz panel *„Masz już zainstalowaną aplikację?”* z przyciskiem **„Otwórz w aplikacji”**. Kliknij, a przeglądarka zapyta o pozwolenie na otwarcie linku `lynxcommander://`. Zgódź się, a aplikacja uruchomi się **zalogowana**, bez ponownego logowania przez Discord.

### 🟡 Ścieżka standardowa — logowanie w aplikacji

Jeśli uruchomisz aplikację z menu Start albo ze skrótu, otworzy się okno logowania Discord. Zaloguj się **tym samym kontem Discord**.

W obu przypadkach sesja jest zapisywana lokalnie i szyfrowana kluczem Twojego konta Windows, więc **kolejne uruchomienia są automatyczne**.

> 💡 **Aplikacja nie prosi o instalację żadnego certyfikatu.** Jeśli starsza wersja instalatora dodała nasz certyfikat do zaufanych w Windows, usuniesz go przyciskiem w Ustawienia → ☕ WSPARCIE.

---

## 6. Krok 4 — Krótka konfiguracja (opcjonalna)

> 💡 **Przy pierwszym uruchomieniu nakładka pokaże samouczek:** skróty na start, układ paneli, głos i opcjonalnie logi gry. Możesz go pominąć, a później uruchomić ponownie w Ustawienia → ☕ WSPARCIE (karta SAMOUCZEK) albo z menu ikony LYNX przy zegarze.

> 💡 **Star Citizen musi działać w oknie bez ramki.** W grze otwórz **Graphics → Window Mode** i wybierz **Borderless**. W trybie Fullscreen Windows zasłania nakładki i paneli nie widać, choć nakładka działa. Dopóki gra jest wyłączona, panele są schowane i pokażą się same, gdy nakładka wykryje Star Citizen.

Po pierwszym uruchomieniu **domyślne ustawienia działają**, więc możesz od razu grać. Jeśli chcesz dopasować nakładkę, otwórz **Ustawienia** z menu ikony LYNX przy zegarze.

Trzy ustawienia, które warto sprawdzić od razu:

| Ustawienie | Gdzie | Co warto |
|---|---|---|
| **Mikrofon** | 🎙️ DŹWIĘK | Wybrać właściwy mikrofon i słuchawki, kliknąć **„TEST MIKROFONU (LIVE)”** |
| **Klawisz PTT** | 🎙️ DŹWIĘK | Domyślnie **CapsLock**. Wygodniej przypiąć boczny przycisk myszy (Mouse4/Mouse5) |
| **Monitor** | PANELE I TRYBY | „Monitor główny nakładki i skanowania”: wybierz ten, na którym grasz |

Skróty do reszty (skany, koło szybkiego wyboru, MobiGlass) ustawisz w zakładce ⌨️ SKRÓTY. Domyślnie działają **F12** (pokaż/ukryj nakładkę) i **F11** (tryb interakcji: panele dają się klikać, a mysz nie trafia do gry). Pozostałe skróty są na starcie nieustawione.

---

## 7. Krok 5 — Pierwsza sesja z innymi

1. **Dołącz do pokoju głosowego.** W organizacji wybierz pokój na karcie **GŁOS** w MobiGlass (otwiera go dzwonek przy panelach w trybie interakcji, **F11**). Bez organizacji wejdź na kartę **GRUPY**: dołączysz do publicznej grupy z głosem albo założysz własną.

2. **Mów.** Przytrzymaj **CapsLock** (albo skrót, który ustawiłeś). Pozostałe osoby w pokoju usłyszą Cię w Mumble.

3. **Zmień status.** Na karcie **MÓJ STATUS** (albo w kole szybkiego wyboru) oznaczysz się jako *Engaging Target*, *Bingo Fuel*, *Damaged* itp. W organizacji zestaw zależy od Twojej rangi. Inni zobaczą to od razu na swoich ekranach.

4. **Skan IFF.** Gdy widzisz innego gracza, naciśnij skrót skanu (ustawisz go w ⌨️ SKRÓTY, na starcie jest pusty). Nakładka odczyta nick z ekranu, zapyta serwer i pokaże gracza razem z jego organizacjami. W organizacji dochodzi oznaczenie wróg/sojusznik według wspólnej listy; solo skaner pokazuje samego gracza.

5. **Ready check.** Prowadzący sprawdza gotowość grupy, a Ty potwierdzasz albo odmawiasz skrótem (domyślnie `[` i `]`) lub w nakładce.

> 💡 **Pierwszy raz w grze z nakładką?** Najpierw ustaw w Star Citizen **Graphics → Window Mode → Borderless**. W trybie Fullscreen paneli nie widać.

---

## 8. Główne funkcje w pigułce

| Funkcja | Co robi | Gdzie ją znajdziesz |
|---|---|---|
| **Panele HUD i MobiGlass** | Statusy, powiadomienia i karty na jednym ekranie nad grą | Nakładka: F12 pokazuje, F11 włącza tryb interakcji |
| **Głos (Mumble)** | Radio z niską latencją, pokoje, rozmowy 1:1, „kto mówi” | Karta GŁOS |
| **Grupy** | Publiczne grupy z głosem i ready checkiem, także bez organizacji | Karta GRUPY |
| **Koło szybkiego wyboru** | Statusy, rozkazy, wyciszenie mikrofonu i Mumble, kanały dwóch przycisków radia, beacony, Ready Check i skany pod jednym klawiszem | Ustawienia → ⌨️ SKRÓTY |
| **Skaner IFF** | Rozpoznaje gracza i jego organizacje; w organizacji oznacza wroga i sojusznika | Skrót w ⌨️ SKRÓTY |
| **Skaner skał** | OCR skał dla górnictwa | Skrót w ⌨️ SKRÓTY, karta SKANER |
| **Crafting, Beacony, Mapa** | Schematy i materiały, zapisane miejsca z nawigacją, mapa 3D | Karty CRAFTING, BEACONY, MAPA |
| **Kontrakty, Misje, Operacje** | Zlecenia, misje na żywo, plan i przebieg operacji (dla organizacji) | Karty KONTRAKTY, MISJE, OPERACJE; strona: Kontrakty, Tablica misji, Kreator misji, Operacje |
| **Organizacja** | Baza członków, role, rekrutacja i zaproszenia | Strona: Centrum organizacji, Baza organizacji, Rekrutacja; nakładka: karty ZESPÓŁ i REKRUTACJA |
| **Profil i hangar** | Nick z gry, statki, znajomi | Strona: Profil operacyjny, Znajomi; nakładka: karty PROFIL, HANGAR, ZNAJOMI |

Część kart i funkcji działa tylko w organizacji. Gracz solo ma skanery, crafting, beacony, timery, znajomych i grupy z głosem.

**Koło szybkiego wyboru** ma zawsze 8 pól i każde pole stoi zawsze w tym samym
kierunku, więc po kilku grach trafiasz z pamięci. Pierwsza strona:

| Pole | Co |
|---|---|
| 1 ↑ | Statusy |
| 2 ↗ | Rozkazy |
| 3 → | Wyciszenie: **Mój mikrofon** (nie nadajesz) albo **Dźwięk Mumble** (nie słyszysz nikogo; wycisza też mikrofon) |
| 4 ↘ | Głos: cel (kanał, cała gałąź, cały serwer), potem Przycisk 1 albo Przycisk 2 |
| 5 ↓ | Beacony: zapis, wpisanie `/showlocation`, nawigacja, wybór celu |
| 6 ↙ | Ready Check (lider) |
| 7 ← | Skany IFF i skał, MobiGlass, ukrycie nakładki, ustawienia |
| 8 ↖ | TERAZ — odpowiedź na to, co czeka (połączenie, zaproszenie, ready check); w spokoju tryb interakcji |

Wybierasz myszą, klawiszami **1–8** albo joystickiem. Dłuższa lista dzieli się
na strony: 7 opcji i **DALEJ ▶** w polu 8, a na kolejnych stronach **◀ WSTECZ**
w polu 1 (strony zmienia też kółko myszy). Środek koła cofa o poziom, Esc zamyka.
Dawne menu z samymi rozkazami i statusami da się przywrócić w Settings → Skróty →
„Stare menu radialne".

**Klawisz koła i analog.** Klik klawisza menu taktycznego otwiera koło, kolejny klik
zatwierdza podświetlone pole (w kategorii wchodzi poziom niżej), a przytrzymanie
klawisza przez 0,5 s zamyka koło od razu, z każdego poziomu. Pole wskazujesz myszą
albo analogiem: w Settings → Skróty kliknij **USTAW OSIE** i porusz analogiem, którym
chcesz sterować (grzybek joysticka lub HOTAS-a albo gałka pada Xbox). Wskazane pole
zostaje podświetlone także po puszczeniu drążka, więc możesz spokojnie kliknąć.

**Dwa przyciski radia.** Przycisk 1 to zwykłe PTT: mówisz na kanał, na którym stoisz.
Przycisk 2 (Settings → Skróty, „Przycisk 2 — drugie radio”) działa tak samo, tylko nadaje
na własny cel, jak drugie radio. Cel wybierasz w kole: **Głos → kanał → Przycisk 1 /
Przycisk 2**. Przycisk 1 przechodzi na wybrany kanał, przycisk 2 zostawia Cię tam, gdzie
jesteś, i tylko nadaje na wybrany kanał. Na liście jest też kanał piętro wyżej (↑), cała
gałąź i cały serwer. Domyślnie przycisk 2 nadaje na całą gałąź.

Gdy celem przycisku 2 jest jeden kanał, słyszysz też wszystkich, którzy na nim mówią,
choć na nim nie stoisz. Działa to jak wspólna częstotliwość: gracze z różnych pokoi,
którzy mają ten sam kanał na przycisku 2, słyszą się nawzajem, nawet gdy na samym kanale
nikt nie stoi. Tak masz łączność na dwa kanały naraz: przyciskiem 1 mówisz na swój kanał,
przyciskiem 2 na drugi, a słyszysz oba. Wskaźnik pokazuje wtedy dopisek
„(słyszysz)” przy celu PTT 2. Cała gałąź i cały serwer działają jak dotąd: przycisk 2
na nie nadaje, ale ich nie słuchasz. Kanał przycisku 2 trwa
do zmiany pokoju; po restarcie nakładki przycisk 1 wraca na własny kanał, a przycisk 2
na gałąź.

---

## 9. Częste problemy

### „Nie słyszę nikogo w Mumble”

1. Czy jesteście w tym samym **pokoju**? Karta **GŁOS** pokazuje, w którym stoisz.
2. Czy wybrane **urządzenie wyjściowe** w Ustawienia → 🎙️ DŹWIĘK jest właściwe (głośniki lub słuchawki, których używasz)?
3. Czy głośność nie jest na 0%? Ustawienia → 🎙️ DŹWIĘK → **„Głośność Odbioru (Global)”**. Głośność pojedynczej osoby ustawisz na jej wpisie w składzie.

### „Mikrofon nie działa / nikt mnie nie słyszy”

1. Ustawienia → 🎙️ DŹWIĘK → **wybierz właściwy mikrofon**.
2. Sprawdź klawisz PTT w tej samej zakładce. Domyślnie CapsLock.
3. Kliknij **„TEST MIKROFONU (LIVE)”**: podczas mówienia pasek poziomu powinien się ruszać.
4. Jeśli używasz NVIDIA Broadcast, Krisp albo Voicemod, upewnij się, że ich wirtualne urządzenia są poprawnie wskazane.

### „Nakładka nie pokazuje się w grze”

1. Star Citizen MUSI działać w **Graphics → Window Mode → Borderless**, nie w Fullscreen.
2. Naciśnij **F12**, żeby pokazać albo ukryć nakładkę. Dopóki gra jest wyłączona, panele są schowane.
3. Sprawdź monitor: Ustawienia → PANELE I TRYBY → „Monitor główny nakładki i skanowania”.
4. Przy wielu kartach graficznych albo monitorach o różnym DPI często pomaga restart aplikacji.

### „SmartScreen / antywirus blokuje aplikację”

1. SmartScreen: zobacz krok 2 powyżej. Kliknij „Więcej informacji” → „Uruchom mimo to”.
2. Antywirus (Defender, Avast, Bitdefender) czasem oznacza nieznane binarki jako podejrzane (heurystyka, nie sygnatura). Dodaj `%LOCALAPPDATA%\LynxOverlay\` do wyjątków.
3. Aplikacja jest podpisana cyfrowo: `Properties` na `LynxOverlay.exe` → „Digital Signatures” → „LYNXCommander Project, valid”.

### „Aplikacja się crashuje / nie startuje”

Ustawienia → **🩺 DIAGNOSTYKA** → **„WYŚLIJ RAPORT DIAGNOSTYCZNY”** spakuje logi i prześle je zespołowi razem ze zgłoszeniem. Błąd albo pomysł możesz też opisać przyciskiem **🐞 ZGŁOŚ BŁĄD** w Ustawieniach albo na stronie w **Pomoc i zgłoszenia**.

### „Logowanie przez Discord nie kończy się / wraca z błędem”

1. Jeśli logujesz się z linku zaproszenia, sprawdź, czy jest aktualny (działa raz i 7 dni). Bez linku logowanie też działa: powstaje konto solo.
2. Wyloguj się z Discorda w przeglądarce i zaloguj ponownie.
3. Spróbuj w trybie incognito albo w innej przeglądarce: rozszerzenia czasem blokują logowanie.

---

## 10. Gdzie szukać pomocy

| Kanał | Kiedy używać |
|---|---|
| **Discord LYNX** | Pytania społecznościowe, awarie real-time |
| **GitHub Issues** | Zgłoszenie buga (techniczny) |
| **GitHub Security Advisory** | Zgłoszenie podatności bezpieczeństwa |
| **E-mail support** | Sprawy licencyjne / komercyjne / wrażliwe |
| **W aplikacji** | Ustawienia → ☕ WSPARCIE — wszystkie linki w jednym miejscu |

Dokładne adresy znajdziesz w [README na GitHubie](https://github.com/Oriinks/LYNXCommander-public#kontakt-i-wsparcie).

---

## 11. Bezpieczeństwo i anti-cheat

LYNXCommander jest świadomie zaprojektowany tak, by **nie naruszać**
regulaminu Cloud Imperium Games (CIG) / Roberts Space Industries (RSI):

- ❌ **NIE czyta pamięci procesu** Star Citizen (`ReadProcessMemory` etc.).
- ❌ **NIE wstrzykuje** DLL-ek, hooków ani żadnego kodu do procesu gry.
- ❌ **NIE wysyła syntetycznego inputu** (klawiatury, myszy, pada) do okna
  gry. Wcześniejsza funkcja "Anti-AFK" została **całkowicie usunięta** żeby
  wyeliminować to ryzyko.
- ❌ **NIE automatyzuje** żadnej akcji w grze (brak aimbot, auto-mine,
  auto-warp, follow-target).
- ❌ **NIE omija** anti-cheat-a (EAC, BattlEye itp.).
- ✅ OCR to **bierne odczytanie pikseli pulpitu** (jak OBS / accessibility
  reader) — żadnego dostępu do stanu gry.
- ✅ Overlay HUD rysuje **nad oknem gry** (topmost WPF window), nie
  modyfikuje gry.
- ✅ Jedyny wyjątek to **opcjonalne tłumaczenie Kraken** (StarStrings od
  MrKrakena, Settings → Start i połączenie): na Twoje kliknięcie i przy
  wyłączonej grze nakładka wgrywa plik `data\Localization\english\global.ini`
  i linię `g_language = english` w `user.cfg`. To droga, którą CIG
  przewidziało dla tłumaczeń społeczności; odinstalowanie przywraca stan
  sprzed instalacji.
- ✅ Aplikacja jest **podpisana cyfrowo** (Authenticode SHA-256, ważne do
  2031).
- ✅ Zgłoszona do **Microsoft Defender Security Intelligence (WDSI)**
  i **Cloud Imperium Games**.

### Skrócony fingerprint LYNXCommander

Aby zweryfikować, że pobrana wersja **naprawdę pochodzi od nas** (a nie od
podszywacza), porównaj ten skrócony fingerprint certyfikatu — pierwsze 4
grupy thumbprint-u SHA-1:

```
D990 1F2B ACA1 831A
```

Publikujemy go w **3 niezależnych miejscach**:

1. Tu, w tym przewodniku (GUIDE.md w repo i pod `/guide` na stronie).
2. Strona pobierania https://lynxcommander.pl/download (widget "FINGERPRINT
   LYNXCommander").
3. Pinned message na oficjalnym Discordzie LYNX.

Jeśli wszystkie trzy miejsca pokazują **tę samą wartość** — masz mocne
potwierdzenie, że nikt nie podszywa się pod naszą aplikację (atakujący
musiałby przejąć jednocześnie GitHub, domenę i Discord). W aplikacji
możesz też kliknąć **Ustawienia → ☕ WSPARCIE → „WERYFIKACJA INTEGRALNOŚCI INSTALACJI”** — odpalimy automatyczne sprawdzenie SHA-256
binarki względem manifestu wydania.

**Jeśli mimo wszystko CIG kiedyś zinterpretuje narzędzie taktycznego
overlay jako problematyczne — końcowa odpowiedzialność za zgodność z ich
regulaminem leży po stronie użytkownika.** Pełne wyjaśnienie i zrzeczenie
się odpowiedzialności w [Regulaminie](/legal/terms) sekcje 3 i 8.

W razie wątpliwości — pełna polityka prywatności (co zbieramy, gdzie
idzie) jest w [Polityce Prywatności](/legal/privacy). Polityka
zgłaszania podatności w [SECURITY](/legal/security). Licencja
w [LICENSE](/legal/license).

---

## Co dalej

Po pierwszej sesji warto:

- 📖 Przeczytać [Regulamin](/legal/terms): krótki, w prostym języku, mówi jasno, co aplikacja robi i czego NIE.
- 🎛️ Pobawić się skrótami w Ustawienia → ⌨️ SKRÓTY (obsługiwane są klawisze boczne myszy, joystick HOTAS, pad Xbox i dwa takie same drążki naraz). W „Urządzenia i osie” nadasz drążkom nazwy, zamienisz ich numery i ustawisz martwą strefę osi.
- 🎯 Ustawić skrót skanu IFF i skał oraz sprawdzić, czy skan trafia w nick na Twojej rozdzielczości.
- 🎨 Ułożyć panele w wygodnych miejscach (tryb układania paneli w nakładce).

Powodzenia w lotach! 🚀

---

_Wersja źródłowa tego dokumentu w repo:
[`GUIDE.md`](https://github.com/Oriinks/LYNXCommander-public/blob/main/GUIDE.md).
W razie wątpliwości lub pytań — skontaktuj się z nami przez Discord lub
GitHub Issues._
