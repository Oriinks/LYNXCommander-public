# Przewodnik dla nowego gracza — LYNXCommander

_Wersja: 1.0 · Data: 2026-05-18_

Cześć! Trafiłeś tu, bo prawdopodobnie ktoś polecił Ci LYNXCommander albo
trafiłeś na nas w organizacji Star Citizen. Ten dokument przeprowadzi Cię
przez pierwsze uruchomienie — od pobrania, przez instalację, po pierwszą
sesję z drużyną.

**Czytanie zajmie ~5 minut. Komplet konfiguracji: ~10 minut.**

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

LYNXCommander to **pasywne narzędzie wspomagające** dla graczy Star Citizen
grających w zorganizowanych drużynach. Składa się z:

- 🌐 **Strony web** (`lynxcommander.pl`) — panel zarządzania składem drużyny,
  oznaczania graczy hostile/friendly (IFF), planowania operacji
- 🖥️ **Aplikacji desktop (overlay)** — nakładka HUD na grę pokazująca
  status załogi w czasie rzeczywistym, plus zintegrowany klient głosowy Mumble
- 🎙️ **Serwera Mumble** — niskolatentny VoIP dla drużyny

> 💡 **Krótko mówiąc:** Discord do gadania, LYNX do koordynacji w grze.

**Co LYNX robi:** pokazuje status drużyny obok gry, pozwala szybko zaznaczać
wrogów/sojuszników, daje radio z niską latencją, łączy organizację z grupą
operacyjną.

**Czego LYNX NIE robi:** nie modyfikuje gry, nie wysyła klawiszy do gry,
nie automatyzuje akcji, nie pomaga celować — pełna lista i wyjaśnienie
w sekcji 12 (i w [Regulaminie](/legal/terms) sekcja 3).

---

## 2. Czego potrzebujesz

Zanim ruszysz:

| Element | Dlaczego |
|---|---|
| **Konto Discord** | Logujesz się przez Discord OAuth (jedyna metoda) |
| **Konto Star Citizen / handle RSI** | LYNX synchronizuje członków Twojej organizacji z RSI |
| **Zaproszenie do LYNX** (invite token) | Pierwsze logowanie wymaga linka zapraszającego od kogoś, kto już jest w systemie |
| **Windows 10 (build 17763+) lub Windows 11, architektura x64** | Aplikacja jest natywnie windowsowa |
| **~500 MB wolnego miejsca** | Self-contained .NET 8 + zależności |
| **Mikrofon** | Do gadania przez Mumble |
| **Star Citizen w trybie Borderless Fullscreen** | Overlay rysuje nad grą — w Exclusive Fullscreen nie będzie widoczny |

### Skąd wziąć invite token?

Zaproszenie wystawia ktoś, kto już jest w organizacji LYNX (kapitan / dowódca).
Link wygląda tak:

```
https://lynxcommander.pl/login?invite=ABCD1234EFGH5678
```

Bez tokena pierwsze logowanie zwróci błąd `missing_invite` lub
`invalid_invite`. Po pierwszej pomyślnej autoryzacji Twoje konto Discord
jest zapamiętane — kolejne logowania **nie wymagają** już tokena.

---

## 3. Krok 1 — Pierwsze logowanie na stronie

1. Otwórz w przeglądarce link zapraszający, który dostałeś od kapitana:
   ```
   https://lynxcommander.pl/login?invite=TwojToken
   ```

2. Zobaczysz ekran "C&C Net Gateway" z dużym przyciskiem **"Zainicjuj
   Bezpieczne Łącze (Discord)"** — kliknij.

3. Discord OAuth poprosi o autoryzację. Zgódź się.

4. Po powrocie z Discorda zostaniesz przeniesiony do panelu (Dashboard).
   Na pasku bocznym po lewej zobaczysz menu sekcji: Konto & Organizacja,
   Party, Operacje, Informacje.

> ⚠️ Jeśli zobaczysz błąd **"Token rekrutacyjny wygasł lub jest nieprawidłowy"** —
> token jest jednorazowy lub stracił ważność. Poproś kapitana o nowy.

---

## 4. Krok 2 — Pobranie i instalacja

Po zalogowaniu na stronie:

1. W bocznym menu (sekcja **INFORMACJE**) kliknij **"Strona pobierania"**.
2. Kliknij duży niebieski przycisk **"Pobierz LynxInstaller.exe"** — plik
   zapisze się w `Pobrane`/`Downloads`.
3. Dwukrotnie kliknij pobrany plik.

> ⚠️ **Windows SmartScreen ostrzeże przy pierwszym uruchomieniu.**
> Zobaczysz okno *"Windows zapobiegł uruchomieniu nierozpoznanej aplikacji"*.
> To normalne — nasz certyfikat nie jest jeszcze w globalnej liście Microsoftu.
> Kliknij **"Więcej informacji"** → **"Uruchom mimo to"**.

W instalatorze:

1. **Zaznacz checkbox** *"Akceptuję Regulamin i Politykę Prywatności"*
   (linki w okienku otwierają dokumenty w przeglądarce — możesz przeczytać
   przed akceptacją).
2. Kliknij **"ZAINSTALUJ"**.

Instalator pobiera aplikację, konfiguruje zaufanie cert (bez UAC — wszystko
dzieje się w tle) i uruchamia LYNX Commander automatycznie. Całość ~30 sekund.

> 💡 Plik instalatora jest dostępny **tylko po zalogowaniu** — nie krąży
> w otwartym internecie. To jest celowe (chronimy graczy i utrudniamy
> podszywanie się pod naszą wersję).

---

## 5. Krok 3 — Pierwsze uruchomienie aplikacji

Są dwie ścieżki:

### 🟢 Ścieżka szybka (zalecana) — "Otwórz w aplikacji"

Jeśli jesteś już zalogowany na **lynxcommander.pl**, wróć na **stronę
pobierania** — pojawi się tam zielony panel *"Masz już zainstalowaną
aplikację?"* z przyciskiem **"Otwórz w aplikacji"**. Klik → przeglądarka
zapyta o pozwolenie uruchomienia linka `lynxcommander://` → kliknij
"Otwórz" → aplikacja podnosi się **zalogowana**, bez ponownego Discord
OAuth. Zobaczysz krótki splash z dźwiękiem, potem HUD.

### 🟡 Ścieżka standardowa — logowanie w aplikacji

Jeśli z jakiegoś powodu nie kliknąłeś "Otwórz w aplikacji" (np. uruchomiłeś
aplikację ze skrótu na pulpicie), aplikacja sama otworzy osadzone okno
logowania Discord. Zaloguj się **tym samym kontem Discord** — po sukcesie
zobaczysz *"WITAJ, [TwojNick]"* i HUD.

W obu przypadkach token sesyjny jest zapisywany lokalnie i szyfrowany
kluczem Twojego konta Windows. **Kolejne uruchomienia są automatyczne**
— bez logowania.

> 💡 **Nie zobaczysz prośby o instalację certyfikatu** — installer wgrał
> go już wcześniej. Jeśli z jakiegoś powodu cert nie został wgrany
> (np. ręcznie skopiowałeś folder aplikacji), aplikacja zapyta jednorazowo
> przy starcie. Wystarczy kliknąć "Tak" → UAC → gotowe.

---

## 6. Krok 4 — Krótka konfiguracja (opcjonalna)

> 💡 **Przy pierwszym uruchomieniu HUD pokaże Ci 5 krótkich tipów na start**
> (toasty po prawej stronie, każdy ~10 sekund). Zobaczysz informacje
> o widżecie załogi, klawiszach F11/F12 i Push-to-Talk. To dzieje się
> **raz w życiu instalacji** — kolejne uruchomienia są ciche.

> 💡 **Gdy uruchomisz Star Citizen po raz pierwszy** z LYNX-em, dostaniesz
> jednorazowy toast: *"Overlay LYNX wymaga trybu Borderless Fullscreen"*.
> To dlatego, że w trybie *Exclusive Fullscreen* Windows zasłania wszystkie
> nakładki — więc nie zobaczysz HUD-u LYNX-a. Zmień **Display Mode** w grze
> na **Borderless** (Settings → Graphics → Display Mode).

Po pierwszym uruchomieniu **wszystkie domyślne ustawienia działają**,
więc możesz od razu zacząć grać. Jeśli jednak chcesz dopasować apkę do
siebie — otwórz **Settings** (ikonka z lewej strony lub menu).

Trzy ustawienia, które warto sprawdzić od razu:

| Ustawienie | Gdzie | Co warto |
|---|---|---|
| **Mikrofon** | 🎙️ Komunikacja Głosowa → "Urządzenie wejściowe" | Wybrać właściwy mic + kliknąć **"TEST MIKROFONU"** |
| **Klawisz PTT** | ⌨️ Skróty → "Mumble PTT" | Domyślnie **CapsLock** — przepnij na boczny przycisk myszy (Mouse4/Mouse5), bo CapsLock w Star Citizen otwiera MobiGlas-a |
| **Monitor overlay** | 🖥️ Wyświetlanie | Wybrać monitor, na którym faktycznie odpalasz Star Citizen |

To wszystko — pozostałe rzeczy ustawisz kiedy zajdzie potrzeba. Domyślnie
działają też skróty **F12** (pokaż/ukryj overlay) i **F11** (przełącz kursor
na overlay — gdy chcesz coś kliknąć w widżetach).

---

## 7. Krok 5 — Pierwsza sesja z drużyną

1. **Wejdź na panel web** (`lynxcommander.pl`) i wybierz lub stwórz Party
   w sekcji "Party" → "Squad Room".

2. **Aplikacja desktop** automatycznie dostanie sygnał o zmianie party
   i przełączy Cię w odpowiedni kanał Mumble. Zobaczysz to w widżecie
   "Crew Status" — będą tam członkowie drużyny.

3. **Mów** — przytrzymaj **CapsLock** (albo skrót, który ustawiłeś).
   Inni członkowie usłyszą Cię w Mumble.

4. **Zmień status** — w widżecie Crew Status możesz oznaczyć siebie jako
   *Engaging Target* / *Bingo Fuel* / *Damaged* itp. (zestaw zależy od
   Twojej rangi w organizacji). Inni od razu zobaczą to na swoich
   ekranach.

5. **Friend/Foe scan** — w grze, gdy widzisz innego gracza, naciśnij skrót
   skanu. Aplikacja zrobi screenshot wybranego obszaru ekranu, wyciągnie
   nick OCR-em i sprawdzi w bazie organizacji — wyświetli tag *FRIEND* /
   *HOSTILE* / *NEUTRAL* w widżecie Friend/Foe.

> 💡 **Pierwszy raz odpalasz w grze?** Najpierw odpal **Star Citizen
> w trybie Borderless Fullscreen** (Settings → Graphics → Display Mode →
> Borderless). W trybie *Exclusive Fullscreen* overlay nie będzie widoczny.

---

## 8. Główne funkcje w pigułce

| Funkcja | Co robi | Gdzie ją znajdziesz |
|---|---|---|
| **Crew Status Widget** | Statusy załogi w czasie rzeczywistym | Na ekranie po włączeniu overlay (F12) |
| **Friend/Foe Widget** | Klasyfikacja widzianych graczy | Wymaga ustawienia obszaru skanu w Settings |
| **Mumble VoIP** | Komunikacja głosowa z drużyną | Auto-łączy po wejściu w party |
| **Tactical Radial Menu** | Szybkie akcje (skrót → koło wyboru) | Settings → Skróty → Tactical Menu |
| **Mission Board** | Zlecenia taktyczne dla drużyny | Panel web → "Operacje" → "Zlecenia Taktyczne" |
| **Fleet Builder** | Planowanie składu flot (uprawnienia) | Panel web → "Operacje" → "Planer operacji" |
| **Organization Database** | Spis członków organizacji | Panel web → "Konto & Organizacja" → "Baza Organizacji" |
| **Rock Scanner** | OCR skanera skał (mining) | Wymaga konfiguracji obszaru w Settings |
| **Contract Tracker** | Tracking zleceń podczas misji | Widżet w HUD + przycisk Track w panelu web |

---

## 9. Częste problemy

### "Nie słyszę nikogo w Mumble"

1. Czy jesteście w tym samym **pokoju Mumble**? Sprawdź widżet Crew Status —
   pokazuje aktualny pokój.
2. Czy wybrane **urządzenie wyjściowe** w Settings → "🎙️ Komunikacja
   Głosowa" jest właściwe (głośniki / słuchawki, których faktycznie używasz)?
3. Czy głośność Mumble nie jest na 0%? Settings → Komunikacja Głosowa →
   suwak "Głośność Wyjściowa".

### "Mikrofon nie działa / nikt mnie nie słyszy"

1. Settings → Komunikacja Głosowa → **wybierz właściwy mikrofon**.
2. Sprawdź skrót PTT (Settings → Skróty → Mumble PTT). Domyślnie CapsLock.
3. Kliknij przycisk **"TEST MIKROFONU"** — powinieneś zobaczyć ruchomy
   pasek poziomu gdy mówisz.
4. Jeśli używasz NVIDIA Broadcast / Discord Krisp / Voicemod — upewnij się
   że ich wirtualne urządzenia są poprawnie skierowane.

### "Overlay nie pokazuje się w grze"

1. Star Citizen MUSI być w trybie **Borderless Fullscreen**, nie
   Exclusive Fullscreen.
2. Naciśnij skrót **F12** żeby zatogglować overlay.
3. Sprawdź czy wybrałeś właściwy monitor w Settings → "🖥️ Wyświetlanie".
4. Jeśli używasz wielu kart graficznych / dwóch monitorów z różnym DPI —
   restart aplikacji często pomaga.

### "SmartScreen / Antywirus blokuje aplikację"

1. SmartScreen — zobacz krok 3 powyżej. Kliknij "Więcej informacji →
   Uruchom mimo to".
2. Antywirus (Defender, Avast, Bitdefender) — czasem oznaczają nieznane
   binarki jako podejrzane (heurystyka, nie sygnatura). Dodaj
   `%LOCALAPPDATA%\LYNXCommander\` do wyjątków.
3. Aplikacja jest podpisana cyfrowo — sprawdź `Properties` na
   `LynxOverlay.exe` → zakładka "Digital Signatures" → "LYNXCommander
   Project, valid".

### "Aplikacja się crashuje / nie startuje"

Settings → **"☕ Wsparcie"** → **"WYŚLIJ RAPORT DIAGNOSTYCZNY"**. Spakuje
logi w ZIP i prześle na nasz kanał diagnostyczny — dev-team dostanie
wszystko czego potrzebuje, żeby zdiagnozować problem. Bez ręcznego
kombinowania z plikami.

### "Discord OAuth nie kończy się / wraca z błędem"

1. Sprawdź czy używasz **zaproszenia z aktualnym tokenem** (token jest
   jednorazowy / wygasa).
2. Wyloguj się z Discorda w przeglądarce i zaloguj ponownie.
3. Spróbuj w trybie incognito / innej przeglądarce — czasem rozszerzenia
   blokują OAuth flow.

---

## 10. Gdzie szukać pomocy

| Kanał | Kiedy używać |
|---|---|
| **Discord LYNX** | Pytania społecznościowe, awarie real-time |
| **GitHub Issues** | Zgłoszenie buga (techniczny) |
| **GitHub Security Advisory** | Zgłoszenie podatności bezpieczeństwa |
| **E-mail support** | Sprawy licencyjne / komercyjne / wrażliwe |
| **W aplikacji** | Settings → "ℹ️ O APLIKACJI" — wszystkie linki w jednym miejscu |

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
możesz też kliknąć **Settings → ☕ Wsparcie / O Aplikacji → "Weryfikuj
integralność instalacji"** — odpalimy automatyczne sprawdzenie SHA-256
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

Po pierwszej sesji z drużyną, warto:

- 📖 Przeczytać [Regulamin](/legal/terms) — krótki, w prostym języku, mówi
  jasno co aplikacja robi i czego NIE.
- 🎛️ Pobawić się skrótami w Settings → "⌨️ Skróty" — dostosuj pod swoje
  zwyczaje (klawisze boczne myszy, joystick HOTAS są obsługiwane).
- 🎯 Skonfigurować obszary skanowania OCR pod swoją rozdzielczość — to
  jednorazowe ustawienie, które bardzo poprawia działanie Friend/Foe scanu.
- 🎨 Przesunąć widżety HUD myszką do wygodnych miejsc — wciśnij **F11**
  (toggle interactivity), wtedy widżety stają się ruchome.

Powodzenia w lotach! 🚀

---

_Wersja źródłowa tego dokumentu w repo:
[`GUIDE.md`](https://github.com/Oriinks/LYNXCommander-public/blob/main/GUIDE.md).
W razie wątpliwości lub pytań — skontaktuj się z nami przez Discord lub
GitHub Issues._
