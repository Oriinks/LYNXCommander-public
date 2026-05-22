# Regulamin korzystania z LYNXCommander

_Wersja: 1.0 · Data wejścia w życie: 2026-05-17_

Niniejszy regulamin ("**Regulamin**") określa zasady korzystania z aplikacji
**LYNXCommander** (dalej "**Aplikacja**"), będącej narzędziem wspomagającym
graczy Star Citizen. Korzystając z Aplikacji, akceptujesz Regulamin w całości.

W razie pytań — kontakt znajdziesz w [README.md](README.md#kontakt-i-wsparcie).

---

## 1. Akceptacja Regulaminu

Pobierając, instalując lub uruchamiając Aplikację, oświadczasz że:

- masz **co najmniej 16 lat** (lub zgodę opiekuna prawnego),
- przeczytałeś, rozumiesz i akceptujesz Regulamin oraz [Politykę Prywatności](PRIVACY.md)
  i [Licencję](LICENSE),
- masz aktywne konto w grze Star Citizen i zgadzasz się przestrzegać regulaminu
  Cloud Imperium Games / Roberts Space Industries (https://robertsspaceindustries.com/tos).

Jeśli nie akceptujesz któregoś z powyższych punktów — nie używaj Aplikacji.

## 2. Czym jest LYNXCommander

LYNXCommander to **pasywne narzędzie wspomagające percepcję sytuacyjną** graczy
Star Citizen. Zapewnia:

- overlay HUD ze statusem załogi (kto żyje / ranny / gotowy),
- komunikację głosową przez protokół Mumble z innymi graczami,
- **bierne** rozpoznawanie tekstu z ekranu (OCR) w celu klasyfikacji
  napotkanych graczy jako "swój / wróg / neutralny" (IFF) na podstawie bazy
  organizacji,
- narzędzie do raportowania znalezisk górniczych do prywatnego kanału
  Discord drużyny.

## 3. Czego Aplikacja **NIE robi** (kluczowe oświadczenie dot. anti-cheat)

Z uwagi na wymogi regulaminu CIG/RSI i poszanowanie dla integralności gry,
Aplikacja **nie wykonuje** i **nie będzie wykonywać** żadnej z poniższych
czynności:

- (a) **nie modyfikuje** klienta gry Star Citizen, jego plików ani pamięci
  procesu,
- (b) **nie wstrzykuje** żadnego kodu, DLL-ek ani hooków do procesu gry,
- (c) **nie czyta pamięci** procesu Star Citizen (`ReadProcessMemory` itp.),
- (d) **nie wysyła** syntetycznego inputu (klawiatury / myszy / pada) do okna
  gry. Funkcja "Anti-AFK", która wcześniej symulowała wciśnięcia klawiszy,
  została **całkowicie usunięta** z Aplikacji w wersji 1.0 z tego powodu,
- (e) **nie automatyzuje** żadnej akcji w grze (strzelanie, miningowanie, lot,
  zaznaczanie celi w grze — Aplikacja jedynie wyświetla informację OBOK gry,
  na overlay-u),
- (f) **nie omija** anti-cheat-a, BattlEye, EAC ani żadnego innego systemu
  detekcji,
- (g) **nie udostępnia** żadnej przewagi mechanicznej (aimbot, wallhack,
  speedhack) — Aplikacja **nie ma** dostępu do stanu gry poza tym, co
  wyświetla się na Twoim ekranie.

Mimo powyższych zabezpieczeń, **finalna odpowiedzialność za zgodność
z regulaminem gry leży po Twojej stronie**. CIG może zmienić swoje zasady
w dowolnym momencie i interpretacja "narzędzia trzeciego" jest po ich
stronie. Zobacz sekcję 8.

## 4. Co Aplikacja **robi** (lista funkcjonalności)

- **OCR**: zrzuty obrazu Twojego pulpitu / okna gry → przetwarzanie LOKALNE
  silnikiem `Windows.Media.Ocr` → ekstrakcja tekstu → wysłanie sparsowanych
  nazw graczy na nasz backend dla klasyfikacji IFF. Surowe obrazy NIE są
  wysyłane (chyba że włączysz lokalny debug-log).
- **Overlay HUD**: rysuje grafikę na ekranie nad innymi oknami (technicznie:
  topmost transparent WPF window). Nie zaznacza nic w samej grze.
- **Mumble client**: standardowy klient VoIP, łączy się z naszym serwerem
  Mumble, transmituje Twój głos. Nie nagrywamy.
- **Hotkeys**: aplikacja rejestruje **globalne** klawisze skrótów przez
  `RegisterHotKey` z user32.dll. Te skróty wywołują akcje WEWNĄTRZ Aplikacji
  (np. otwarcie radiala, skan OCR). Nie wstrzykują niczego do gry.
- **Auto-updater**: silnik Velopack sprawdza dostępność nowej wersji i pobiera
  ją z naszej oficjalnej dystrybucji przy każdym uruchomieniu Aplikacji.
  Auto-update jest **wymagany** — nowe wersje zawierają poprawki bezpieczeństwa
  oraz aktualizacje zgodności z bieżącą wersją gry Star Citizen i protokołów
  serwera Mumble. W bieżącej wersji Aplikacji **nie ma opcji wyłączenia**
  auto-update'u, co jest świadomą decyzją projektową — zapewnia że wszyscy
  użytkownicy korzystają z wersji zgodnej z naszą aktualną polityką
  bezpieczeństwa i regulaminem CIG. Jeśli z istotnych powodów chciałbyś
  trwale zablokować auto-update u siebie, możesz to zrobić zaporą sieciową
  (zablokować dostęp Aplikacji do `lynxcommander.pl/updates/`); pamiętaj
  jednak, że pozostając na starszej wersji rezygnujesz z aktualnych poprawek
  bezpieczeństwa.
- **Telemetria błędów**: opisana w [Polityce Prywatności](PRIVACY.md#23-telemetria-i-logi).

### 4.1. Operacje wykonywane przez instalator na systemie Windows

Instalator (`LynxInstaller.exe`) wykonuje na Twoim systemie operacyjnym
następujące operacje **w zasięgu Twojego konta Windows** (bez modyfikacji
ustawień innych użytkowników komputera):

- (a) **Pliki aplikacji** — instaluje binarki LYNXCommander w
  `%LOCALAPPDATA%\LynxOverlay\` (lokalny profil użytkownika).
- (b) **Certyfikat wydawcy** — wgrywa nasz publiczny certyfikat code-signing
  (CN=LYNXCommander Project, thumbprint dostępny w README) do magazynu
  zaufanych wydawców `CurrentUser\Root` + `CurrentUser\TrustedPublisher`.
  Eliminuje to ostrzeżenia UAC *"Unknown publisher"* dla podpisanych
  binarek Aplikacji. Nie wpływa na cert dla innych aplikacji.
- (c) **Protokół URI** — rejestruje schemat `lynxcommander://` w rejestrze
  Windows (`HKCU\Software\Classes\lynxcommander`). Pozwala to stronie
  `lynxcommander.pl` uruchomić Aplikację z jednorazowym tokenem logowania
  (eliminacja dwukrotnego logowania Discord). Aplikacja parsuje tylko
  argument `lynxcommander://login?token=...` — inne formy URI są ignorowane.
- (d) **Plik konfiguracyjny** — tworzy `settings.json` w
  `%APPDATA%\LynxOverlay\` z zapisem akceptacji Regulaminu (wersja, czas,
  SHA-256 treści) oraz Twoich preferencji UI.

**Odinstalowanie:** możesz w dowolnym momencie usunąć:
- pliki aplikacji — przez Velopack uninstaller (start menu) lub ręcznie
  `%LOCALAPPDATA%\LynxOverlay\`,
- certyfikat — `certmgr.msc` → *Certyfikaty - bieżący użytkownik* →
  *Zaufane główne urzędy certyfikacji*,
- klucz rejestru — `regedit` → `HKCU\Software\Classes\lynxcommander`,
- konfigurację — `%APPDATA%\LynxOverlay\`.

Żadna z tych operacji **nie wpływa na klienta gry Star Citizen** ani na
inne aplikacje na Twoim komputerze (oświadczenie z sekcji 3 pozostaje
w mocy).

## 5. Twoje obowiązki jako użytkownika

Korzystając z Aplikacji, **zobowiązujesz się**:

- nie używać Aplikacji do działań niezgodnych z regulaminem CIG/RSI ani
  prawem powszechnie obowiązującym,
- nie modyfikować, nie dekompilować, nie reverse-engineerować Aplikacji
  (poza wyjątkami opisanymi w [LICENSE](LICENSE)),
- nie próbować obchodzić zabezpieczeń (cert weryfikacja, podpis cyfrowy,
  trust-store),
- nie używać Aplikacji do nękania innych graczy, doxxingu, hate speech ani
  innych zachowań sprzecznych z polityką społeczności,
- pobierać Aplikację **wyłącznie** z oficjalnych źródeł (sekcja 11),
- weryfikować sumę kontrolną SHA256 pobranego pliku z opublikowaną wartością
  (instrukcja w README).

## 6. Co jest zabronione

W szczególności **zabronione jest**:

- (a) używanie Aplikacji w jakichkolwiek narzędziach automatyzujących grę
  (bot farming, AFK farming, automatyczne strzelanie itp.),
- (b) redystrybucja Aplikacji (publikacja na innych stronach, w torrentach,
  na cracksite'ach) — patrz [LICENSE](LICENSE),
- (c) modyfikacja kodu w sposób, który **dodaje** funkcjonalność łamiącą
  regulamin CIG (np. wstrzykiwanie inputu),
- (d) używanie Aplikacji do działalności komercyjnej (np. wynajem usług
  trackingu graczy, sprzedaż dostępu do bazy IFF) bez pisemnej zgody
  zespołu LYNX,
- (e) udostępnianie swojego tokena / konta innym osobom.

Naruszenie powyższych może skutkować:

- natychmiastowym **wypowiedzeniem licencji** (LICENSE sekcja 8),
- usunięciem konta z systemu LYNXCommander,
- zgłoszeniem do CIG, jeśli dotyczy naruszenia ich regulaminu,
- odpowiedzialnością cywilnoprawną.

## 7. Wymagania techniczne

- Windows 10 wersja 17763 (RS5) lub nowsza, Windows 11.
- Architektura **x64**.
- ~500 MB wolnego miejsca na dysku.
- Połączenie internetowe (backend, Mumble, auto-update).
- Aplikacja **nie wymaga** uprawnień administratora do normalnej pracy.
  Wymaga ich **jednorazowo** do instalacji wewnętrznego certyfikatu CA
  (skrypt `InstallCert.ps1`) — szczegóły w [README.md](README.md#certyfikat).

## 8. Wyłączenie odpowiedzialności

APLIKACJA JEST DOSTARCZANA **"TAK JAK JEST"** ("AS IS"), BEZ ŻADNYCH
GWARANCJI WYRAŹNYCH ANI DOROZUMIANYCH, W TYM GWARANCJI PRZYDATNOŚCI
HANDLOWEJ, PRZYDATNOŚCI DO OKREŚLONEGO CELU LUB NIENARUSZALNOŚCI PRAW
OSÓB TRZECICH.

W ZAKRESIE DOZWOLONYM PRZEZ PRAWO, ZESPÓŁ LYNX **NIE PONOSI ODPOWIEDZIALNOŚCI** ZA:

- (a) **bany lub ograniczenia konta** w grze Star Citizen wynikające
  z korzystania z Aplikacji (mimo że dokładamy wszelkich starań aby
  Aplikacja była zgodna z regulaminem CIG, finalna decyzja jest po stronie
  CIG i ich systemów detekcji),
- (b) **utratę danych** (postęp w grze, awatary, itemki),
- (c) **straty pośrednie** (utracone korzyści, czas, reputację),
- (d) **przerwy w działaniu** Aplikacji, serwerów Mumble, backendu, RSI API,
- (e) **działania osób trzecich** (Discord, CIG, dostawca VPS).

Łączna odpowiedzialność zespołu LYNX wobec Ciebie, niezależnie od podstawy
prawnej, **nie przekroczy 50 PLN** (lub równowartości w EUR/USD), chyba że
przepisy bezwzględnie obowiązujące stanowią inaczej.

Jeśli jesteś **konsumentem**, powyższe ograniczenia odpowiedzialności
nie wyłączają obowiązków zespołu LYNX wynikających z bezwzględnie
obowiązujących przepisów konsumenckich (m.in. ustawy o prawach konsumenta).

## 9. Brak zobowiązania do utrzymania usługi

LYNXCommander to **projekt społecznościowy**. Zespół LYNX **nie gwarantuje**:

- ciągłości działania backendu / serwera Mumble,
- aktualizacji / nowych funkcji,
- kompatybilności z każdą wersją Star Citizen.

Możemy w dowolnym momencie:

- zakończyć projekt,
- zmienić warunki korzystania (z 30-dniowym uprzedzeniem dla zmian
  na niekorzyść użytkowników),
- zmigrować do modelu płatnego (sekcja 10).

## 10. Możliwa monetyzacja w przyszłości

Aktualnie Aplikacja jest **darmowa**. Zespół LYNX zastrzega sobie prawo do:

- wprowadzenia w przyszłości wersji płatnej / premium,
- wprowadzenia modelu subskrypcji,
- ograniczenia funkcji w wersji darmowej.

Zmiany te **nie będą działać wstecz** dla wersji już zainstalowanych — będą
dotyczyć nowych wersji, do których będziesz mógł zaktualizować się dobrowolnie.

## 11. Oficjalne kanały dystrybucji

Aplikację możesz pobrać **wyłącznie** z:

- oficjalnej strony LYNXCommander (link w [README.md](README.md#pobieranie)),
- publiczne repozytorium GitHub: https://github.com/Oriinks/LYNXCommander-public.

**Nie pobieraj** Aplikacji z innych źródeł — mogą zawierać złośliwe modyfikacje.
Zawsze weryfikuj sumę kontrolną SHA256 (instrukcja w README).

## 12. Prawo właściwe i jurysdykcja

Regulamin podlega prawu **Rzeczypospolitej Polskiej**. Spory wynikające
z Regulaminu rozstrzygane są przez sąd powszechny właściwy dla siedziby
administratora, z zastrzeżeniem bezwzględnie obowiązujących przepisów
konsumenckich, które mogą przyznać konsumentowi inny sąd właściwy.

Jeśli jesteś konsumentem mieszkającym w UE/EOG, masz też prawo do
**pozasądowego rozwiązywania sporów** za pośrednictwem platformy ODR
Komisji Europejskiej: https://ec.europa.eu/consumers/odr.

## 13. Zmiany Regulaminu

Możemy aktualizować Regulamin. Istotne zmiany zostaną ogłoszone:

- w aplikacji (komunikat przy starcie),
- na Discordzie LYNX,
- w niniejszym pliku w repozytorium GitHub (historia w git logu).

Korzystanie z Aplikacji po wejściu zmian w życie oznacza ich akceptację.
Jeśli nie akceptujesz nowej wersji — odinstaluj Aplikację.

## 14. Postanowienia końcowe

- Jeśli któryś punkt Regulaminu zostanie uznany za nieważny, pozostałe
  postanowienia pozostają w mocy.
- Regulamin stanowi pełną umowę między Tobą a zespołem LYNX
  w zakresie korzystania z Aplikacji.
- W kwestiach nieuregulowanych stosuje się przepisy Kodeksu cywilnego,
  ustawy o prawach konsumenta, RODO i ustawy o świadczeniu usług drogą
  elektroniczną.

---

_Ostatnia aktualizacja: 2026-05-17. Wersja źródłowa: w repo GitHub w pliku
[`TERMS.md`](TERMS.md)._
