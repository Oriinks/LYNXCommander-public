# Polityka bezpieczeństwa — LYNXCommander

Dziękujemy, że pomagasz utrzymać LYNXCommander bezpiecznym! Niniejszy dokument
opisuje, jak zgłaszać podatności i jak na nie reagujemy.

## Wspierane wersje

| Wersja | Wsparcie bezpieczeństwa |
|---|---|
| najnowszy release na `main` | ✅ aktywne |
| wszystkie wcześniejsze | ❌ aktualizuj |

LYNXCommander ma **auto-updater** (Velopack) — w normalnym scenariuszu Twoja
aplikacja sama pobiera najnowszą wersję. Jeśli wyłączyłeś auto-update, pamiętaj
o ręcznej aktualizacji.

## Jak zgłosić podatność

### NIE otwieraj publicznego GitHub Issue z opisem podatności.

Zamiast tego:

1. **Preferowana droga — prywatny advisory na GitHubie:**
   - https://github.com/Oriinks/LYNXCommander/security/advisories/new
   - Wybierz "Report a vulnerability". Tylko opiekun repo to zobaczy.

2. **Alternatywnie — e-mail:** napisz na adres kontaktowy zespołu (patrz
   [README.md → Kontakt i wsparcie](README.md#kontakt-i-wsparcie)).
   - W tytule: `[SECURITY] LYNXCommander – <krótki opis>`.
   - W treści: opis podatności, kroki reprodukcji, wersja aplikacji, system,
     potencjalny wpływ.
   - Jeśli chcesz szyfrowania PGP — napisz najpierw bez szczegółów, prześlemy
     klucz publiczny.

3. **Discord (kanał prywatny):** tylko jako sygnał awaryjny ("zgłaszam
   security issue mailem"). **Nie publikuj szczegółów na czacie.**

## Co zgłaszać

Interesują nas wszystkie problemy, które mogą:

- naruszyć poufność / integralność danych użytkowników,
- pozwolić nieuprawnionej osobie na działanie w imieniu innego użytkownika,
- pozwolić na wstrzyknięcie kodu / RCE,
- doprowadzić do bypassu zabezpieczeń (autoryzacja, walidacja, podpis),
- spowodować eskalację uprawnień,
- ujawnić sekrety (klucze API, tokeny, hasła) w binarce / repo / logach,
- pozwolić atakującemu na utworzenie scenariusza, w którym CIG/EAC mogłaby
  zinterpretować zachowanie aplikacji jako narzędzie zabronione (to dla nas
  **krytyczne** — chronimy konta naszych użytkowników).

### Przykłady **w zakresie**:
- Podpis cyfrowy może być sfałszowany / pominięty.
- Backend API ujawnia dane innego użytkownika.
- Webhook Discord URL w binarce można wydobyć i nadużyć (znana sprawa —
  rotujemy okresowo, ale przyspieszymy jeśli będzie nadużycie).
- DPAPI token można odczytać przez inny proces.
- OCR pipeline można nakłonić do wysłania ekranu na inny serwer.
- W pliku `.pfx`/`.key` znalezionym w repozytorium (NIE POWINNO BYĆ — patrz
  `.gitignore`).
- **Manipulacja URI protokołem `lynxcommander://`** — np. nakłonienie
  użytkownika do kliknięcia złośliwego linka, który próbuje zapisać sfałszowany
  token do aplikacji. **Status: ZAADRESOWANE w wersji 1.1.0+.**
  Po refactorze auth (patrz [`docs/AUTH_ARCHITECTURE.md`](docs/AUTH_ARCHITECTURE.md)
  sekcja 4.5) URI scheme zawiera **jednorazowy kod** (one-time code), a nie
  pełny JWT. Kod jest single-use, ważny 60 sekund, i sam w sobie nie autoryzuje
  niczego — overlay wymienia go przez <code>POST /api/auth/exchange-code</code>
  na pełną parę tokenów (access JWT + refresh). Atakujący który przechwyci URI
  ma okno 60s na wymianę kodu — i nawet jeśli mu się uda, dostanie sesję
  z innym IP/UA niż prawowity user, co zostanie wykryte w audit logach.
  Stary mechanizm <code>TryConsumeDeepLinkToken</code> (akceptujący JWT w URI)
  jest deprecated i zostanie usunięty po <code>LegacyTokensEnabled=false</code>.
- **Leak tokena JWT w event log Windows** przez deep link `lynxcommander://`.
  **Status: ZAADRESOWANE.** Po refactorze auth w URI scheme znajduje się tylko
  one-time code (single-use, 60s lifetime), a nie pełny JWT. Wyciek code'a
  z Event Log nie daje atakującemu działającego dostępu — code wygasa zanim
  ktokolwiek zdąży go znaleźć w logach.
- **Refresh token reuse / kradzież** — atakujący który przechwyci refresh token
  (np. przez kradzież DPAPI store'a na zainfekowanej maszynie usera) próbuje
  go użyć w <code>/api/auth/refresh</code>. **Mitigacja: Refresh Token Rotation
  (RTR).** Każde odświeżenie tokenu unieważnia poprzedni — jeśli atakujący
  użyje skradzionego refresh-a, prawowity user przy następnej próbie refresh-u
  trafi na "reuse detected" i CAŁA rodzina tokenów (atakującego i usera) zostanie
  unieważniona. Obaj muszą się zalogować ponownie, ale dostęp atakującego jest
  natychmiastowo przerwany.
- **CSRF z innej strony WWW** — atakujący nakłania zalogowanego usera do
  wejścia na stronę, która submituje formularz na <code>lynxcommander.pl/api/...</code>.
  **Mitigacja: double-submit cookie pattern.** Backend wystawia cookie
  <code>lynx_csrf</code> + frontend musi dokleić wartość do nagłówka
  <code>X-CSRF-Token</code>. Atakująca strona nie ma JS-owego dostępu do
  cookie usera (cross-origin), więc nie może wystawić poprawnego nagłówka.
  Dodatkowo wszystkie cookies sesyjne mają <code>SameSite=Strict</code>,
  co blokuje cross-site requesty na poziomie przeglądarki.

### Przykłady **poza zakresem** (mile widziane do issue / PR, ale nie security):
- Błędy UI, crashe niezwiązane z bezpieczeństwem.
- Brak rate-limit w endpointach niewrażliwych.
- Niedziałający przycisk.
- "Aplikacja używa internetu" (tak, używa, to feature).

## SLA / czas odpowiedzi

| Wydarzenie | Czas |
|---|---|
| Pierwsza odpowiedź na zgłoszenie | **72 godziny** (zwykle < 24h) |
| Triaż i wstępna klasyfikacja | **7 dni** |
| Wydanie poprawki (zależnie od krytyczności) | Critical: 7 dni · High: 30 dni · Medium: 90 dni · Low: best-effort |
| Publiczne ujawnienie (CVE / advisory) | Skoordynowane z Tobą, zwykle 90 dni od zgłoszenia lub po wydaniu poprawki — co nastąpi wcześniej |

## Bezpieczne ujawnianie (responsible disclosure)

Prosimy o:

- **dawaniu nam czasu** na naprawę przed publicznym ujawnieniem (90 dni
  to standard branżowy),
- **nieujawnianiu** podatności publicznie podczas embarga,
- **nieeksploatowaniu** podatności poza testem dowodowym,
- **nie naruszaniu** prywatności innych użytkowników.

W zamian:

- **podziękujemy publicznie** w changelogu / advisory (jeśli chcesz),
- **nie skierujemy** roszczeń prawnych za działania zgodne z tą polityką
  (safe harbor),
- **rozważymy nagrodę** (bug bounty) — projekt jest hobbystyczny, więc
  to symboliczna kwota lub kosmetyka w grze, ale doceniamy.

## Co możesz zrobić jako użytkownik dla swojego bezpieczeństwa

1. **Pobieraj tylko z oficjalnych źródeł** (patrz [README.md → Pobieranie](README.md#pobieranie)).
2. **Weryfikuj SHA256** pobranego pliku z opublikowaną wartością.
3. **Aktualizuj** aplikację (auto-update jest domyślnie włączony).
4. **Nie udostępniaj** swojego tokena / pliku `token.dat`.
5. **Zgłoś nam** wszystko podejrzanego (niespodziewany ruch sieciowy,
   pliki w `%APPDATA%\LynxOverlay` których się nie spodziewasz).

## Historia advisory

Brak wydanych advisory. Po pierwszym zostaną wymienione tutaj (z linkiem
do CVE / GHSA jeśli dotyczy).

---

_Wersja: 1.0 · Ostatnia aktualizacja: 2026-05-17_
