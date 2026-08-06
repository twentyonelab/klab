---
typ: handoff
data: 2026-08-06
projekt: KLAB — prezentacja interaktywna (odnoga symulatora)
tagi: [handoff, klab, prezentacja, 21-zmyslow]
---

# Handoff — Prezentacja interaktywna KLAB (5 slajdów) — 2026-08-06

## Cel

Nowa odnoga projektu KLAB: **prezentacja przeglądarkowa — 5 interaktywnych slajdów** w designie istniejącego konfiguratora (Symulator fabryki v0.11). Ma pokazać **technologię, efektywność i przewagi** fabryki KLAB — bez lania wody, na konkretach. Praca w tym samym repo (`twentyonelab/klab`), docelowo publikacja na railway.com (opcjonalnie GitHub Pages — patrz „Deploy").

**Termin całego projektu: komplet materiałów 07.08.2026** — prezentacja musi powstać szybko, przez maksymalne przetwarzanie tego, co już zbudowane. „Wystarczająco dobre na czas" > „idealne po terminie".

## Kontekst projektu (dla nowej sesji od zera)

- **Co to za fabryka:** zakład KLAB — recykling akumulatorów kwasowo-ołowiowych (LAB) + produkcja nowych ogniw **w obiegu zamkniętym**. Linia A = recykling (A1–A8), linia B = produkcja ogniw (B1–B14 + B8b). Wsad 20 t/d złomu LAB → frakcje (Pb metaliczne, pasta→PbO, PP→regranulat, H₂SO₄) wracają do produkcji.
- **Zespół:** 21 zmysłów (design/strategia + inżynieria). **W materiałach dla klienta ZERO nazwisk** — pisz „po weryfikacji/doprecyzowaniu specyfikacji technicznej".
- **Główne narzędzie (już istnieje):** `Symulator fabryki v0.11.html` — jednoplikowy konfigurator hali (layout 2D, schemat procesu, bilans mediów, personel, koszty, 3D izometria, karty urządzeń). Prezentacja = **osobny plik/folder**, ale ten sam język wizualny i te same dane.
- **Repo jest PUBLICZNE** — wszystko co trafi na GitHub Pages jest publiczne; dane klienta (CAPEX, media) już tam są, ale świadomie. Railway daje URL publiczny, tylko mniej indeksowalny.

## Stan — co zrobione (sesja macierzysta)

- Symulator v0.11: kompletny, 67× PASS audytu Playwright. Layout z drag&drop, przesuwalne/skalowalne ciągi i strefy (8 uchwytów), Autorozmieszczenie (plan referencyjny 156×72), Auto-układ (3 propozycje z twardą zasadą kolejności procesu), Optymalizuj (toast), widoki M1–M6, karty urządzeń z dostawcami (zweryfikowane linki), eksport HRF CSV.
- **Oryginalne logo KLAB** (wordmark „KLAB®") wbudowane 1:1 jako wektor.
- Intro: wyśrodkowane, znak wodny KLAB, 3 CTA („Zobacz projekt" wczytuje wbudowaną koncepcję A), copyright na dole.
- GitHub Pages live: https://twentyonelab.github.io/klab/ (deploy-from-branch `claude/new-session-07diir`, odświeża się ~1 min po pushu; `index.html` w korzeniu przekierowuje na najnowszą wersję).

## Zasoby do przetworzenia — GDZIE CO JEST w `Symulator fabryki v0.11.html`

Wszystko czego potrzebuje prezentacja już istnieje w jednym pliku. Szukaj po nazwach stałych (grep):

| Zasób | Stała / selektor | Co zawiera |
|---|---|---|
| **Design tokens** | `:root{` na początku `<style>` | --bg #F2F1EF · --card #FFF · --ink #1A1A1A · --mut #8A8A85 · --line #E5E3DF · --acc #FF4D00 · --ok #16A34A · --warn #F59E0B · --bad #DC2626 · lineB #7C3AED · media: energia #F97316, woda #3B82F6, N₂ #9CA3AF, kwas #8B5CF6 |
| **Logo KLAB** | `const KLAB_PATH` + `function KLAB_SVG(color)` | oryginalny wordmark klienta, viewBox 0 0 1486 327, fill-rule evenodd; wersja czarna #1A1A1A i biała (znak wodny `KLAB_WM_URL`) |
| **Katalog urządzeń** | `const DEVICES=[...]` | 23 karty procesowe A1–A8, B1–B14, B8b: wymiary, kW, media, personel, CAPEX, we→wy, przepustowość, pełna `karta` (opis, urządzenia, media, odpady, BHP, dostawcy z linkami) |
| **Dane zbiorcze** | `const DOK={...}` | wsad 20 t/d · 1455 kWh/d · 454/481 MWh/rok · 8,1 kg Pb/kWh · lead time 75→49 h · CAPEX ~35 mln zł · hala 156×72 (11 232 m²) · 84 os./zm. · PV 1 MWp · odzysk ciepła 250–450 kW |
| **Gotowy projekt hali** | `const KONCEPCJA_A={...}` | koncepcja A: hala 112×120, 23 urządzenia, 5 ciągów, 6 stref (0 kolizji) — format JSON projektu |
| **Plan referencyjny** | `function autoReferencyjny()` + pola `ref` w DEVICES | hala 156×72 wg dokumentacji klienta |
| **Grafiki urządzeń** | `const GFX`, `const GFX2` | wektorowe schematy kreskowe + szare „rendery" (placeholdery) |
| **Renderer hali SVG** | `function render()` | rysowanie hali w metrach (viewBox), urządzenia/strefy/ciągi/media — do uproszczenia w wersji read-only |
| **Izometria 3D** | `function renderIso()` | statyczna izometria SVG (wysokości robocze 3 m) |
| **Logika kosztów** | `renderM6()` + `zalozenia` | ceny: energia 0,75 zł/kWh, woda 15 zł/m³, N₂ 0,80 zł/m³, 10 000 zł/os./mies., UR 3% CAPEX/rok |

Osobno w repo: `klab_plan_referencyjny.json` (plan referencyjny jako projekt), `notatki-analiza-dokumentacji-07.2026.md` (wyciąg z 5 PDF + XLSX klienta), `CLAUDE.md` (zasady projektu).

## Twarde zasady (przenoszą się na prezentację)

1. **Jeden plik HTML** na artefakt, zero CDN, zero frameworków, działa offline z podwójnego kliknięcia. (Prezentacja = osobny plik, np. `prezentacja/index.html` — może być własny, ale ta sama filozofia.)
2. **UI po polsku.** Bez nazwisk. Profesjonalny język pod klienta.
3. **Nie walidujemy chemii klienta** — deklaracje z dokumentacji (PbSO₄→PbO „battery-grade", −52% Pb) opisujemy jako „wg dokumentacji technologicznej".
4. **Dane W-draft/D-szac oznaczamy uczciwie** (badge/przypis) — to buduje wiarygodność, nie osłabia.
5. Żadnego dark mode, żadnych ozdobnych animacji, żadnego WebGL (3D tylko statyczna izometria SVG).
6. **Nie zmieniaj plików symulatora** — prezentacja to odnoga; jeśli reużywasz kod z localStorage, zmień klucz (symulator używa `klab_sym_v02`).

## Ślepe uliczki (nie trać czasu drugi raz)

- **Upload plików SVG pada** po stronie przeglądarki w tym kliencie czatu — SVG wklejać jako tekst źródłowy (tak weszło logo).
- **Globalne `svg{width:100%}` w CSS psuje zagnieżdżone SVG** (grafiki w kafelkach) — selektory zawężać (lekcja z v0.3).
- **Zdjęcia z sieci: NIE** — plik offline + repo publiczne (licencje); na Wikimedia brak sensownych zdjęć tego typu urządzeń (sprawdzone). Tylko wektory własne / materiały od dostawców z RFQ.
- **Google Fonts: NIE** (offline) — systemowy stack sans-serif.
- **Drag/pozycjonowanie SVG przy zoomie** — przeliczaj współrzędne przez `getScreenCTM().inverse()`, nie przez getBoundingClientRect (naprawione w v0.11; dotyczy też prezentacji przy pan/zoom hali).

## Plan prezentacji — 5 slajdów (rekomendacja z sesji macierzystej)

Narracja: **Dlaczego → Jak → Gdzie → Ile → Co dalej.** Każdy slajd odpowiada na jedno pytanie i broni się solo (prezenter może skakać). Zasada anty-przeładowania: **1 slajd = 1 teza (nagłówek-zdanie) + 1 interakcja + maks 3 liczby na widoku**; reszta w warstwie na klik.

1. **„Nic nie ginie" — obieg zamknięty** (Dlaczego). Kołowy diagram: zużyty akumulator → breaker → 4 frakcje → nowe ogniwo. Interakcja: klik frakcji podświetla jej ścieżkę (Pb→rafinacja, pasta→PbO→pasty, PP→regranulat 2,2 t/d→obudowy, H₂SO₄→elektrolit). Liczby: 20 t/d wsadu · 4 frakcje w obiegu · 8,1 kg Pb/kWh.
2. **„23 węzły, jeden ciąg" — technologia** (Jak). Schemat procesu jak w M1: linia A nad linią B, strzałki we→wy. Interakcja: klik węzła → mini-karta (we→wy, przepustowość, media) — głębia na żądanie, domyślnie czysty łańcuch. Liczby: 8 węzłów recyklingu · 15 produkcji · lead time 75→49 h.
3. **„Zaplanowany zakład, nie koncepcja" — layout hali** (Gdzie; slajd obowiązkowy wg briefu). Plan 2D z konfiguratora (Koncepcja A lub referencyjny 156×72) read-only: pan/zoom, hover urządzenia → tooltip, przełączniki warstw (strefy/ciągi/media). Liczby: 11 232 m² · 84 os./zm. · 0 kolizji. CTA: „zbudowane w naszym symulatorze — każdy wariant przeliczamy na liczby".
4. **„Policzone, nie obiecane" — efektywność** (Ile). Dashboard KPI w stylu konfiguratora + **1 suwak** (np. liczba zmian albo cena energii) przeliczający OPEX na żywo (logika z M6). Liczby bazowe: 1455 kWh/d · 454→481 MWh/rok · odzysk ciepła 250–450 kW · PV 1 MWp. To jest wyróżnik: prezentacja LICZY, nie twierdzi.
5. **„Trzy przewagi, jeden krok" — przewagi + CTA** (Co dalej). Maks 3 przewagi, każda z dowodem-liczbą: (a) obieg zamknięty = własny surowiec (frakcje z 20 t/d wracają do produkcji), (b) hydrometalurgia PbO zamiast pirometalurgii = niższa energia/emisje *(wg dokumentacji technologicznej)*, (c) gotowy plan wykonawczy: layout + media + HRF, CAPEX ~35 mln zł (W-draft→RFQ), realizacja do 31.12.2030. Zamknięcie: następny krok (RFQ / spotkanie / demo konfiguratora).

**Nawigacja/UX:** górny pasek kropek/tabów 1–5 (metafora tabów M1–M6 z konfiguratora) + strzałki ←/→ + klawisze; hash-routing `#1…#5` (linkowalny start od dowolnego slajdu = elastyczność); Esc → siatka-przegląd 5 slajdów; stopka: logo KLAB + „© 21 zmysłów LAB" + badge W-draft gdzie trzeba. Pełny ekran, bez scrolla w pionie.

## Deploy

- **Najprościej (od zaraz):** folder `prezentacja/` w tym repo → GitHub Pages już to serwuje: `https://twentyonelab.github.io/klab/prezentacja/` (Pages deployuje całą gałąź `claude/new-session-07diir`; push = live w ~1 min). Uwaga: publiczne.
- **Railway (wg planu użytkownika):** serwis z tego repo (lub osobnego), statyczny hosting — najlżejsze opcje: start command `npx serve prezentacja` albo Dockerfile z nginx serwującym folder. Zero backendu — to czysty plik statyczny. Railway też daje URL publiczny; jeśli ma być niejawnie, dodać basic auth na poziomie serwera.
- Plik ma działać też **offline z podwójnego kliknięcia** (wysyłka mailem/Drive) — kolejny argument za zero-dependency.

## Następne kroki

1. Nowa sesja: zbudować `prezentacja/index.html` wg planu 5 slajdów (wyciągnąć z v0.11: tokeny, KLAB_SVG, DEVICES, DOK, KONCEPCJA_A, uproszczony renderer hali).
2. Audyt Playwright jak w symulatorze (Chromium: `/opt/pw-browsers/chromium`): nawigacja klawiszami, hash-routing, interakcje slajdów, 0 błędów JS.
3. Push → sprawdzić na Pages → (opcjonalnie) podpiąć Railway.

## Prompt startowy do nowej sesji

```
Kontynuuję pracę z poprzedniej sesji (repo twentyonelab/klab, projekt KLAB firmy 21 zmysłów).

Projekt: zbuduj INTERAKTYWNĄ PREZENTACJĘ przeglądarkową „5 slajdów" o fabryce KLAB
(recykling akumulatorów kwasowo-ołowiowych + produkcja ogniw w obiegu zamkniętym) —
w designie istniejącego konfiguratora, jako osobną odnogę projektu.

Zacznij od: przeczytaj w repo plik `handoff-2026-08-06-prezentacja-klab.md` — tam jest
komplet: kontekst, mapa zasobów w `Symulator fabryki v0.11.html` (stałe KLAB_PATH/KLAB_SVG,
DEVICES, DOK, KONCEPCJA_A, tokeny CSS w :root), twarde zasady (1 plik HTML, zero CDN,
offline, po polsku, bez nazwisk, dane W-draft oznaczane), ślepe uliczki oraz gotowy plan
5 slajdów (Dlaczego→Jak→Gdzie→Ile→Co dalej) z zasadą: 1 teza + 1 interakcja + ≤3 liczby.

Twoje zadanie: stwórz `prezentacja/index.html` wg tego planu (slajd 3 = layout hali
z konfiguratora read-only), z nawigacją tabami/strzałkami/hash-routingiem i przeglądem
pod Esc. Nie modyfikuj plików symulatora. Po zbudowaniu: audyt Playwright
(executablePath: /opt/pw-browsers/chromium), commit i push na gałąź
claude/new-session-07diir, podgląd: https://twentyonelab.github.io/klab/prezentacja/
```

---
*Handoff z pamięci kontekstu — początek sesji macierzystej był kompaktowany; kluczowe fakty zweryfikowane w plikach repo (CLAUDE.md, Symulator fabryki v0.11.html).*
