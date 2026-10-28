# Prompt pro Claude Code – kalkulačka pravoúhlého trojúhelníku

Vytvoř mi webovou aplikaci na výpočet pravoúhlého trojúhelníku. Ať je to jeden soubor `pravouhly_trojuhelnik.html` s čistým HTML, CSS a JavaScriptem, bez frameworků a bez build kroku. Musí fungovat offline po otevření v prohlížeči. Ulož ho do `1ZS/_SPOLECNE/nastroje/`. Kód komentuj česky a podrobně, jsem v programování začátečník.

## Co má umět

Zadám libovolné dvě známé veličiny (aspoň jedna musí být délka) a aplikace dopočítá zbytek. Počítá živě při psaní, bez tlačítka „Spočítat“.

**Značení (české školní):** pravý úhel γ = 90° je u vrcholu C, přepona c leží proti C, odvěsna a leží proti úhlu α (vrchol A), odvěsna b proti úhlu β (vrchol B). Tangens se značí `tg`, arkustangens `arctg`.

### 1. Náčrt s poli kolem trojúhelníku
- Uprostřed je velká plocha (SVG) s trojúhelníkem na pozadí milimetrového papíru.
- Je tam 5 vstupních polí: a, b, c, α, β. **Pole nejsou ve formuláři vedle, ale přímo v náčrtu.** Pole pro stranu je vně trojúhelníku u středu strany (za kótou), pole pro úhel je vně u vrcholu ve směru vnější osy úhlu. Pole se nesmí překrývat s trojúhelníkem ani mezi sebou.
- Strany jsou okótované jako na technickém výkrese (kótovací a vynášecí čáry se šipkami). Úhly α a β mají oblouk, pravý úhel čtvereček, vrcholy jsou popsané A, B, C.
- Trojúhelník se kreslí ve skutečném poměru stran podle zadání. Jen u extrémních úhlů (pod 12° nebo nad 78°) se tvar omezí a aplikace napíše, že náčrt není v měřítku. Náčrt se i s poli vždy sám vejde do plochy.
- Barvy stavů: zadané pole je modré s plným rámečkem, dopočítané zelené s čárkovaným rámečkem, chyba nebo nesoulad červeně. Stejnou barvu má i kóta nebo oblouk dané veličiny v náčrtu. Při najetí myší nebo kliknutí do pole se zvýrazní odpovídající strana nebo úhel.
- Pod hodnotou v každém poli je malý řádek: „zadáno“, u dopočítaného pole použitý vzorec (např. `= c · sin α`), nebo chybová hláška.
- Přepínač „Výška a úseky“ zobrazí čárkovaně výšku v_c na přeponu s patou D a u ní popisky v_c, c_a, c_b s hodnotami.

### 2. Otáčení náčrtu
Chci si trojúhelník natočit stejně jako v zadání příkladu (třeba v hydromechanice: šikmá stěna, šikmý manometr, svah kanálu). Proto:
- tlačítka „otočit o 90° doleva“ a „doprava“, posuvník 0–359° a otáčení tažením myší po ploše (přichytávání po 15°),
- tlačítka „Zrcadlit“, „Přepona dole“ (přepona vodorovně, C nahoře) a „Výchozí poloha“,
- otočení i změny tvaru se plynule animují (respektuj `prefers-reduced-motion`). Pole s hodnotami se pohybují s trojúhelníkem a text v nich zůstává vodorovně.

### 3. Logika výpočtu
- Přijímá desetinnou čárku i tečku a ignoruje znak ° na konci.
- Mezivýsledky se nezaokrouhlují, zaokrouhluje se jen zobrazení. Počet desetinných míst je nastavitelný (2–6). Jednotka délky je volitelná (m, mm…), obsah se zobrazuje v jednotkách².
- Řeší všechny dvojice: (a, b), (a, c), (b, c), (a, α), (a, β), (b, α), (b, β), (c, α), (c, β). Když zadám jen úhel, dopočítá druhý úhel a napíše, že chybí délka. Když zadám jen jednu délku, vyzve mě k zadání další veličiny.
- Validace: délky > 0, úhly mezi 0° a 90°, odvěsna < přepona, α + β = 90°. Chyba se ukáže červeně u pole i ve stavovém řádku.
- Dopočítaná hodnota se zobrazuje přímo v poli. Když do něj začnu psát, stane se zadanou. Když pole smažu, je z něj zase dopočítané.
- Když zadám víc než dvě hodnoty, počítá se z posledních dvou zadaných, které tvoří řešitelnou dvojici. Ostatní zadané hodnoty se jen zkontrolují (tolerance 0,5 %) a označí ✓ souhlasí nebo ✗ vychází X.

### 4. Postup výpočtu pod náčrtem
Každá dopočítaná veličina má kartu. Kroky jsou číslované v pořadí, v jakém se počítají:
- **co počítám** (např. „Přepona c, Pythagorova věta“),
- **výchozí vztah** (např. sin α = a/c),
- **obecný vzorec** (c = a / sin α),
- **dosazení** (c = 3 / sin 30°),
- **výsledek** s jednotkou. Zaokrouhlený výsledek má znak ≐, úhly jsou i v radiánech.

Matematika je vysázená hezky (zlomky pod sebou, odmocnina s vodorovnou čarou, horní indexy), a to bez externích knihoven, ať to jde offline.

Pod tím je sekce „Další veličiny“ ve stejném formátu karet: výška na přeponu v_c, úseky přepony c_a a c_b (Eukleidovy věty), obsah S, obvod o a sklon přepony (tg α, v % a jako svah 1 : m).

Tlačítko „Kopírovat postup“ zkopíruje Markdown s LaTeXem (`$…$`, `$$…$$`, desetinná čárka jako `{,}`, `\operatorname{tg}`), abych ho mohl vložit do Obsidianu.

### 5. Ostatní
- Po otevření se načte ukázka. V nabídce „Příklad“ jsou: šikmá stěna nádrže (c = 2,5 m, α = 60°), šikmý mikromanometr (c = 150 mm, α = 20°), svah kanálu (a = 1,2 m, b = 1,8 m) a trojúhelník 3–4–5. Je tam i tlačítko „Vymazat hodnoty“.
- Vzhled technického výkresu: záhlaví jako rohové razítko, světlý i tmavý režim podle systému, funguje i na mobilu (pole se zmenší).
- Celé UI je česky.

### 6. Ověření
Než skončíš, otestuj tyhle případy:
- (a = 3, b = 4) → c = 5, α ≐ 36,870°
- (c = 2,5, α = 60°) → a ≐ 2,165, b = 1,25
- (a = 3, c = 2) → chyba
- tři konzistentní hodnoty → ✓
- po otočení o 90° a zrcadlení jsou pole pořád u správných stran

Pak udělej screenshot přes Playwright na desktopu i v šířce mobilu.
