# Analiza publicznych grup Facebook w 2025/2026 — praktyczny przewodnik dla projektu "Spotted Politechnika Lubelska"

## TL;DR
- **Rekomendowana ścieżka praktyczna na 2026 rok**: zbieraj dane manualnie/półautomatycznie z poziomu zalogowanego konta członka grupy za pomocą **rozszerzenia Zeeschuimer (Firefox) → 4CAT** (Digital Methods Initiative, Uniwersytet Amsterdamski; Peeters & Hagen, *Computational Communication Research* 4(2), 2022); do analizy tekstu polskiego użyj **HerBERT-large** (Mroczkowski et al. 2021, KLEJ benchmark 88.4) lub **Bielik-11B-v2.0-Instruct** (SpeakLeash + ACK Cyfronet AGH, arXiv:2505.02410, Open PL LLM Leaderboard 64.98). Oficjalny Meta Content Library API jest dla studenta licencjackiego/magisterskiego praktycznie nieosiągalny, a Graph API od 2018 (Cambridge Analytica) i 2024 (deprecjacja Groups API v19.0) nie pozwala czytać postów grup, których nie jesteś administratorem.
- **Ramy prawne — kluczowy wniosek**: scrapowanie publicznie dostępnych danych jest legalne w USA (Meta v. Bright Data, N.D. Cal. 23.01.2024, sędzia E. Chen), ale w UE/Polsce dane z publicznych grup FB pozostają **danymi osobowymi w rozumieniu RODO** (komentarze Oxford IDPL 2020, doktryna "pseudo-public" + Decyzja Prezesa UODO ZSZZS.440.108.2019); podstawą prawną dla projektu akademickiego jest art. 6(1)(e) lub art. 6(1)(f) + art. 89 ust. 1 RODO ("badania naukowe") wraz z art. 5 ust. 1 lit. b Ustawy z 10 maja 2018 r. o ochronie danych osobowych (Dz.U. 2018 poz. 1000), pod warunkiem pseudonimizacji/anonimizacji i zgody komisji etyki uczelni.
- **Metodologia merytoryczna**: stosuj **netnografię** Roberta Kozinetsa (*Netnography: The Essential Guide*, 3rd ed., SAGE 2020) + **wytyczne AoIR IRE 3.0** (Franzke, Bechmann, Ess, Zimmer 2020) jako podstawę etyczną; do analizy treści — content analysis (Krippendorff) wspomagana komputerowo w MAXQDA/ATLAS.ti, plus **BERTopic z embeddingami HerBERT** dla modelowania tematów subkultury parkingowej.

## Key Findings

### 1. Co umarło, a co jeszcze działa w 2025/2026
- **Graph API dla grup**: Po skandalu Cambridge Analytica (2018) Meta drastycznie ograniczyła dostęp; w styczniu 2024 ogłoszono deprecjację **Groups API w wersji v19.0** (TechCrunch, 5.02.2024) — odpowiedź na zapytanie do publicznej grupy zwraca błąd `(#200) Requires either admin with granted managed_group permissions or member using installed app`. Praktycznie: **nie da się legalnie pobrać postów grupy, której nie jesteś administratorem**, nawet jako jej członek.
- **CrowdTangle**: wyłączony **14 sierpnia 2024** mimo petycji badaczy. Wg raportu Coalition for Independent Technology Research (CITR) *Blocking our Right to Know: Surveying the Impact of Meta's CrowdTangle Shutdown* (lipiec 2024, n=36 respondentów z 34 organizacji): *"88% of researchers surveyed expressed concern that CrowdTangle's shutdown will hinder their work, as projects have to be revised or scrapped, with some considering stopping their work entirely."*
- **Meta Content Library (MCL) + Content Library API**: następca CrowdTangle, dostęp przez **ICPSR/SOMAR (University of Michigan)** lub Meta SRE. **Dla studenta projektu zaliczeniowego praktycznie nieosiągalny** — wymaga afiliacji instytucjonalnej, podpisanej Restricted Data Use Agreement (RDUA), wskazania PI z doktoratem. Zgodnie z oficjalnym FAQ SOMAR opublikowanym 15.09.2025: *"A monthly fee of 371 USD will be charged to your research team for each month your team accesses your VDE research environment… For New VDE Teams: A one-time fee of 1000 USD will be charged to your research team at the start of your project, as well as a monthly charge of 371 USD."* Meta SRE jest darmowe, ale wymaga oddzielnej aplikacji. Dane obejmują "posts to Pages, groups and events" — w teorii pasują, ale proces aplikacji trwa tygodnie/miesiące.
- **DSA Article 40 (UE)**: Akt Delegowany — formalnie **Delegated Regulation (EU) 2025/2050** — przyjęty **2 lipca 2025** (Komisja Europejska, digital-strategy.ec.europa.eu) i wszedł w życie **29 października 2025**, kiedy badacze mogli składać pierwsze wnioski przez DSA Data Access Portal. Art. 40(4) — dane wewnętrzne tylko dla badaczy systemowych ryzyk (nie dla projektu o parkowaniu). Art. 40(12) — dostęp do **publicznie dostępnych danych** dla szerszego grona; podstawa prawna chroniąca scraping w celach badawczych w UE. Wymaga jednak afiliacji z instytucją badawczą zdefiniowaną w Dyrektywie 2019/790 (CDSM).

### 2. Co realnie działa do zebrania danych z grupy "Spotted Politechnika Lubelska"

**Opcja A — Zeeschuimer + 4CAT (rekomendowana dla akademika)**
- **Zeeschuimer** (Digital Methods Initiative, DOI 10.5281/zenodo.7525702, MPL 2.0, darmowy) — rozszerzenie Firefox, które monitoruje ruch HTTP podczas przeglądania platformy i zbiera metadane z postów, które widzisz; export do NDJSON.
- **4CAT Capture and Analysis Toolkit** (Peeters & Hagen, *Computational Communication Research* 4(2), 2022, doi:10.5117/CCR2022.2.007.HAGE) — wspiera "Facebook and Instagram (via CrowdTangle or Facepager exports)" jako import CSV; uruchamiany lokalnie w Dockerze. Wdrożony "at dozens of universities around the world" (KCL KingsCAT).
- Workflow: zaloguj się do FB → włącz Zeeschuimer → przewijaj grupę → wyślij dane do 4CAT → analizuj słowa kluczowe, wykresy aktywności, sieci wzmianek.

**Opcja B — Facepager** (Jünger & Keyling, GitHub strohne/Facepager) — narzędzie GUI do API + webscrapingu, SQLite + CSV export. Autor jawnie ostrzega: "support for the official Facebook, Twitter and YouTube APIs is limited"; działa głównie z otwartymi API (Reddit, Mastodon, Wikidata).

**Opcja C — Apify Facebook Groups Scraper** (komercyjny, ale tani)
- Cena: **2,60 USD / 1000 postów** dla aktora apify/facebook-groups-scraper; plan startowy 29 USD/m daje do ok. 5800 postów; obsługuje tylko grupy publiczne. Wbudowane residential proxy. Eksport JSON/CSV/Excel.

**Opcja D — Eksport HTML + Python (BeautifulSoup/Playwright)**
- Otwórz grupę w przeglądarce, zapisz strony jako HTML (Ctrl+S "complete webpage"), zparsuj BeautifulSoup. Pracochłonne, ale w 100% pod kontrolą i nie narusza limitów technicznych — bo to pobieranie ręczne. Możesz też uruchomić **Playwright/Selenium** ze swoim własnym ciasteczkiem sesyjnym.

**Opcja E — Rozszerzenia Chrome (członkowie/posty)**: FB Group Extractor, FaceGM, Group Extractor for Facebook — większość celuje w *członków* (lead-gen), nie posty; jakość niska, część narusza ToS Mety. **Nie zalecane do badań naukowych** (brak kontroli nad próbą, brak metadanych, ryzyko utraty konta).

### 3. Polski NLP — narzędzia 2025/2026 dla tekstu Spotted (slang studencki, polski potoczny)

| Narzędzie | Organizacja | Zastosowanie | Benchmark |
|---|---|---|---|
| **HerBERT-large-cased** (allegro/herbert-large-cased) | Allegro ML Research (Mroczkowski et al. 2021, arXiv:2105.01735) | sentyment, klasyfikacja, NER | KLEJ 88.4; PolEmo2.0-IN ≈92.8 acc |
| **Bielik-11B-v2.0-Instruct** (speakleash/Bielik-11B-v2.0-Instruct) | SpeakLeash + ACK Cyfronet AGH (arXiv:2505.02410, maj 2025) | LLM, zero-shot sentyment, podsumowania, trenowany na 200B tokenów polskich na superkomputerze Helios | Open PL LLM Leaderboard 58.14 (base) / 64.98 (instruct) |
| **PLLuM** (8B–70B, modele 24.02.2025) | Politechnika Wrocławska (koord.) + NASK + OPI PIB + IPI PAN, finansowane przez Ministerstwo Cyfryzacji (arXiv:2511.03823) | LLM uniwersalny; modele non-comm. na ~150B tokenów polskich | Polish Linguistic and Cultural Competency Benchmark (PLCC) |
| **polish-roberta-large-v2** (sdadas) | OPI-PG (Dadas, Perełkiewicz, Poświata 2020, arXiv:2006.04229) | embeddingi do BERTopic | PolEmo2.0-IN 92.8, NKJP 94.5 |
| **sentimentPL** (PyPI) | philvec, oparty na HerBERT + CLARIN-PL | gotowy klasyfikator regresji sentymentu (-1;+1) | dostępny jako `pip install sentimentpl` |

- **Kluczowy dataset**: **PolEmo 2.0** (Kocoń, Zaśko-Zielińska, Miłkowski 2019, CLARIN-PL, http://hdl.handle.net/11321/710) — ma **dziedzinę "university"**, idealną do transfer learningu na korpus Spotted.
- **Modelowanie tematów**: BERTopic (Grootendorst 2022) z embeddingami HerBERT/Polish-RoBERTa. Dla krótkich postów Spotted BERTopic bije LDA (Egger & Yu, *Frontiers in Sociology* 2022, doi:10.3389/fsoc.2022.886498).

### 4. Wizualizacja danych
- **Gephi** (open source) — sieci użytkowników i wzmianek; szczególnie pod wizualizacje wielkoskalowe.
- **NodeXL Pro** (Social Media Research Foundation, dodatek do Excel) — łatwiejszy, importer Zeeschuimer NDJSON, ale tylko Windows.
- **InfraNodus** (Nodus Labs) — sieci tekstowe (co-occurrence słów), użyteczne do mapowania tematów (np. wokół "parking", "miejsce", "PŁ").
- **4CAT wbudowane procesory**: word clouds, time series, n-gramy, wykresy aktywności, sieci wzmianek — wystarczające dla projektu studenckiego.
- **R: tidytext + ggraph + igraph** lub Python: NetworkX + Plotly/Bokeh do dashboardów aktywności.

### 5. Etyka i prawo — RODO/GDPR i polski porządek prawny

- **Dane z publicznych grup FB są danymi osobowymi**. Komentarze Oxford International Data Privacy Law (Stalla-Bourdillon, IDPL 2020) i UGent (re)search tips traktują takie dane jako "pseudo-public": *"the fact that some data is public on social media does not mean that there are no limits to its use. The principles of the GDPR apply."*
- **Podstawa prawna**: dla projektu akademickiego standardowo art. 6(1)(e) RODO (zadanie w interesie publicznym) lub art. 6(1)(f) (uzasadniony interes), w obu przypadkach łącznie z **art. 89 ust. 1 RODO** (badania naukowe + odpowiednie zabezpieczenia).
- **Polski porządek prawny**: Ustawa z dnia 10 maja 2018 r. o ochronie danych osobowych (Dz.U. 2018 poz. 1000) + Ustawa z dnia 20 lipca 2018 r. — Prawo o szkolnictwie wyższym i nauce.
- **Kluczowa Decyzja Prezesa UODO ZSZZS.440.108.2019**, cytat verbatim: *"Artykuł 89 ust. 1 i 2 RODO jednoznacznie wskazuje, że na administratorze przetwarzającym dane w celach badań naukowych ciąży obowiązek zabezpieczenia i ochrony danych, a także podejmowania czynności przetwarzania z poszanowaniem prawa do ochrony danych osobowych osób, których dane są przetwarzane. (...) Wniosek o udostępnienie danych powinien być zatem uargumentowany w wyczerpujący sposób, tak aby wskazywał na niezbędność ich pozyskania przez osobę prowadzącą badania w formie umożliwiającej identyfikację."* W praktyce: **identyfikowalne treści muszą być zminimalizowane/zanonimizowane**, a konieczność ich zachowania udokumentowana.
- **Praktyczne wymogi etyczne** (AoIR IRE 3.0, Franzke et al. 2020, https://aoir.org/reports/ethics3.pdf):
  1. Pseudonimizacja autorów postów (hash ID, nie nazwiska).
  2. Parafrazowanie cytatów wrażliwych zamiast dosłownego cytowania (aby uniknąć wyszukiwarki Google→identyfikacji).
  3. Brak udostępniania surowego datasetu (AoIR IRE 3.0 explicite ostrzega: dane mogą zawierać informacje, które *"could be used directly or indirectly against individuals"*).
  4. Zgoda komisji etyki PŁ.
  5. Dokumentacja DPIA (data protection impact assessment) jeśli skala > 1000 osób lub treści wrażliwe.
- **ToS Mety**: zakazuje "automated collection of data". Naruszenie ToS to ryzyko utraty konta, ale w USA Meta v. Bright Data (N.D. Cal. 23.01.2024, sędzia E. Chen) potwierdziło, że scraping danych publicznych nie narusza CFAA, a klauzule "survival" w ToS są nieegzekwowalne wobec niezalogowanego scrapera. To jednak orzecznictwo amerykańskie, **nie przesądza o RODO**.

### 6. Akademickie ramy metodologiczne

- **Netnografia** (Robert Kozinets, *Netnography: The Essential Guide*, 3rd ed., SAGE 2020) — etnografia online, opracowana w 1995 r. (oryginalnie do analizy fandomu Star Trek), do badania kultur i społeczności sieciowych. Sześć faz: planning → entrée → data collection → interpretation → ethics → representation.
- **Content analysis** (Krippendorff, *Content Analysis: An Introduction to Its Methodology*, 4th ed., SAGE 2018) — kodowanie ilościowe i jakościowe treści; idealne do mierzenia frekwencji tematów ("parking", "miejsce", "samochód") w korpusie Spotted.
- **Computer-mediated discourse analysis (CMDA)** (Herring) — analiza pragmatyczna interakcji online.
- **AoIR IRE 3.0** (Franzke, Bechmann, Ess, Zimmer 2020) — standard etyczny dla badań internetowych; "non-participatory observation of public communication" jest dopuszczalna, ale zachęca do anonimizacji + niedzielenia datasetu.
- **Polska literatura referencyjna**: Anna Wileczek (UJK Kielce), *Kod młodości. Młodomowa w kontekstach społeczno-kulturowych* (PWN 2018) oraz "Kod młodzieży czy kod młodości? Społeczno-kulturowe aspekty 'mediatyzacji' młodomowy" (PAN 2020) — socjolingwistyka młodomowy, baza interpretacyjna slangu Spotted; Mateusz Halawa (2013) "Facebook — platforma algorytmicznej towarzyskości i technologia siebie" (*Kultura i Społeczeństwo*); Olcoń-Kubicka & Batorski (2006) "Prowadzenie badań przez Internet" (*Studia Socjologiczne* 3). **Uwaga**: po systematycznym przeszukaniu CEJSH, bibliotekanauki.pl, pressto.amu, ResearchGate i Google Scholar **nie zidentyfikowano peer-reviewed polskiego artykułu poświęconego konkretnie fenomenowi "Spotted" stron na polskich uczelniach** — to luka badawcza, którą projekt może wypełnić (do zaargumentowania we wstępie). Historycznie pierwszą znaczącą polską stroną "Spotted" była "Spotted: BUW" (Biblioteka UW), powstała pod koniec 2012 r. (Piasecki, *Polityka* 2013).

### 7. Alternatywy do bezpośredniego scrapingu
- **Meta Content Library** (omówiony wyżej) — w teorii idealny, w praktyce dla doktorantów+ z PI z PhD i podpisem dziekana.
- **DSA Article 40(12)** — od 29 października 2025 może umożliwić studentom dostęp do publicznych danych przez DSA Data Access Portal, ale na czerwiec 2026 to dopiero faza pilotażowa; sprawdź u Digital Services Coordinator (DSC) w Polsce — UKE.
- **Ankieta uzupełniająca** — zamiast/oprócz scrapingu, możesz uzyskać świadomą zgodę od próby członków grupy na cytowanie i wywiady.
- **Manualna obserwacja**: dla próby ~100-500 postów manualne kodowanie w MAXQDA jest legalnie najbezpieczniejsze (brak automatyzacji = brak naruszenia ToS).
- **Dane od administratorów grupy**: jeśli udałoby się skontaktować z administratorami "Spotted PŁ" i uzyskać dane jako member-of-group via Graph API (`managed_group` permission) — ale wymaga ich współpracy.

## Details

### Architektura rekomendowanego pipeline'u

```
[Konto FB (członek grupy)]
        ↓
[Firefox + Zeeschuimer] ←—— manualne przewijanie grupy (snowball: kategorie, hashtagi, daty)
        ↓ NDJSON eksport
[4CAT lokalnie (Docker)]
        ↓ CSV
[Anonimizacja: hash autorów (SHA-256 + sól), redakcja cytatów wrażliwych]
        ↓
[Pipeline analizy w Pythonie]
   ├── HerBERT-large embeddingi → BERTopic (modelowanie tematów)
   ├── sentimentPL lub HerBERT fine-tuned na PolEmo2.0-university (sentyment)
   ├── Bielik-11B (zero-shot klasyfikacja: parking vs. inne tematy)
   └── pandas + networkx (statystyki, sieci)
        ↓
[Wizualizacja: Gephi (sieci) + Plotly (czasowe) + word clouds]
        ↓
[Raport: kodowanie jakościowe w MAXQDA dla próby 100-200 postów "parking"]
```

### Co konkretnie zbierać (i czego unikać)

**Zbieraj**:
- Treść postu (sparafrazowana w finalnym raporcie).
- Timestamp.
- Liczba reakcji, komentarzy, udostępnień (agregaty).
- Hash autora (np. SHA-256(user_id + sól projektu)).
- Tematykę (kodowaną ręcznie lub przez BERTopic).

**Nie zbieraj/nie publikuj**:
- Nazwisk, imion, zdjęć profilowych.
- Treści identyfikujących konkretne osoby trzecie (np. "ten gość z drugiego roku WIPB").
- Treści wrażliwych w rozumieniu art. 9 RODO (zdrowie, orientacja, polityka) bez dodatkowej podstawy.

### Praktyczna ostrożność operacyjna
- Używaj **dedykowanego konta** do badań (nie głównego), żeby nie ryzykować banem.
- Korzystaj z **rate-limitingu** — nie więcej niż ~1 request/2-3 sekundy.
- Trzymaj surowe dane na **zaszyfrowanym dysku** (np. VeraCrypt) i usuń po obronie pracy.
- Zarejestruj projekt u IOD (Inspektor Ochrony Danych) Politechniki Lubelskiej.

## Recommendations

**Etap 1 — Przygotowanie (1-2 tygodnie)**:
1. Zarejestruj projekt u IOD PŁ + komisji etyki uczelni. Przedstaw DPIA: cel, podstawa prawna (art. 6(1)(e) + 89(1) RODO), środki techniczne (pseudonimizacja, szyfrowanie, retencja).
2. Zainstaluj Firefox + Zeeschuimer + 4CAT (Docker). Sprawdź workflow na 10-20 postach próbnych.
3. Przygotuj kodbook do content analysis: kategorie tematyczne (parking miejsce, parking konflikt, parking koszt, parking inne), kategorie sentymentu (negatywny/neutralny/pozytywny/ironiczny).

**Etap 2 — Zbieranie danych (2-4 tygodnie)**:
4. Zbierz korpus 500-2000 postów z grupy (Zeeschuimer + scrollowanie z FoxScroller dla automatyzacji). Filtruj po słowach kluczowych: "parking", "miejsce", "auto", "samochód", "PŁ parking" itp.
5. Eksportuj do 4CAT, przeprowadź wstępną eksplorację: word clouds, częstość słów, time series aktywności.

**Etap 3 — Analiza (3-4 tygodnie)**:
6. Modelowanie tematów: BERTopic + HerBERT-large embeddings. Identyfikuj klastry tematyczne wokół parkowania.
7. Sentyment: fine-tune HerBERT na PolEmo2.0-university, zaaplikuj do postów "parking".
8. Manualne kodowanie (MAXQDA lub Atlas.ti) próby 100-200 postów — netnograficzna interpretacja, kategorie subkultury (rytuały, normy, językowe markery in-group).
9. Wizualizacja: Gephi dla sieci wzmianek między autorami, Plotly dla czasówek.

**Etap 4 — Pisanie i etyka (2-3 tygodnie)**:
10. **Parafrazuj wszystkie cytaty** — zacytowanie dosłowne pozwala odnaleźć autora przez wyszukiwarkę.
11. Nie publikuj datasetu; w pracy magisterskiej/inżynierskiej w aneksie umieść jedynie agregaty i kodbook.
12. W bibliografii cytuj: Kozinets 2020, Franzke et al. 2020 (AoIR), Krippendorff 2018, Decyzja UODO ZSZZS.440.108.2019, Wileczek 2018.

**Progi decyzyjne (kiedy zmienić strategię)**:
- Jeśli korpus > 5000 postów → rozważ wystąpienie o vetted researcher status DSA Art. 40(12) przez promotora.
- Jeśli grupa się "zamknie" (privacy → closed) → przerwij scraping i opieraj się na uprzednio zebranych danych.
- Jeśli IOD PŁ odmówi → zmień metodę: ankieta dobrowolna wśród członków grupy + wywiady półustrukturyzowane.
- Jeśli zauważysz ban/throttling konta → przejdź na 100% manualne kodowanie próby ~200 postów.

## Caveats

- **Ekosystem narzędzi zmienia się co kilka miesięcy**. Apify, Bright Data, scrapery oparte na cookie-auth mogą przestać działać w dowolnym momencie — Meta aktywnie modyfikuje frontend. Zeeschuimer też wymaga regularnej aktualizacji (jak wskazuje autor Stijn Peeters, UvA / Digital Methods Initiative).
- **Orzeczenie Meta v. Bright Data** dotyczy prawa amerykańskiego (CFAA + contract law). W UE/Polsce nadrzędne pozostaje RODO i Decyzje UODO — orzeczenie nie chroni przed zarzutem naruszenia RODO.
- **Meta Content Library**, mimo że jest oficjalnym narzędziem badawczym, posiada **istotne ograniczenia** względem CrowdTangle. Wg wspólnego śledztwa Proof News, Tow Center for Digital Journalism (Columbia University) i Algorithmic Transparency Institute (autorki Sarah Grevy Gotfredsen i Kaitlyn Dowling, lipiec 2024): *"On eleven key topics, Meta's new tool has fewer features than CrowdTangle."* Cameron Hickey (CEO National Conference on Citizenship) powiedział TechCrunch (15.08.2024), że MCL oferuje tylko *"1% of the features"*. Limit dostępu: 500 000 rekordów / 7 dni / badacz; 60 zapytań synchronicznych/min.
- **DSA Art. 40(12)** — nowy mechanizm; w październiku 2025 Komisja Europejska wstępnie stwierdziła naruszenie przez Metę i TikToka obowiązku udostępniania danych badaczom. Liczne praktyczne problemy z "data stand-off" (DSA Observatory, IViR, marzec 2025).
- **Brak indeksowanej polskiej literatury naukowej o stronach "Spotted"** — to luka, którą projekt może wypełnić, ale wymaga oparcia teoretycznego na adjasentnej literaturze (Wileczek o młodomowie, Halawa o Facebooku, Kozinets o netnografii).
- **PolEmo 2.0 domena "university"** dotyczy recenzji uczelni studenckich, nie postów Spotted — transfer może wymagać dodatkowego fine-tuningu i ręcznej walidacji.
- **Bielik/PLLuM** — modele bardzo nowe (PLLuM publiczny release 24.02.2025); brak długoterminowych benchmarków stabilności, ale szybko rozwijane przez konsorcjum publiczne (Politechnika Wrocławska / NASK / OPI / IPI PAN / IS PAN / Uniwersytet Łódzki).
- **Etyka "Spotted"**: choć grupa jest publiczna, posty często dotyczą trzecich osób, które *nie wyraziły zgody* na bycie podmiotem postu (zakochania, plotki, opisy konkretnych osób). To podnosi etyczne progi: nawet manualne cytowanie postu, który identyfikuje osobę trzecią, może być problematyczne. Skupienie projektu na "subkulturze parkowania" jest etycznie bezpieczniejsze niż wątki personalne — i tak warto pozostać przy tej zawężonej tematyce.