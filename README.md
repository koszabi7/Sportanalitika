# Játékosok gólszámának előrejelezhetőségének vizsgálata regressziós modellek alkalmazásával

A projekt célja annak vizsgálata, hogy a Premier League játékosainak következő szezonbeli gólszáma milyen pontossággal becsülhető meg korábbi szezonstatisztikák alapján. A munkában a hangsúly az adatgyűjtésen (web scraping), az adattisztításon és feature engineeringen, valamint a gépi tanulási regressziós modellek kipróbálásán és összehasonlításán volt.

# Adatgyűjtés

Forrás: fbref.com
 (Premier League játékosok statisztikái)

Módszer: Selenium + BeautifulSoup alapú web scraping

Összegyűjtött adatok: szezononkénti gólszám, gólpassz, percek, meccsek, ligahelyezés, xG, stb.

Kimenet: saját készítésű, több ezer soros CSV adatbázis

# Adatelőkészítés

Felesleges oszlopok törlése

Duplikált sorok összevonása (pl. szezon közbeni klubváltás esetén)

Hiányzó értékek (NaN) kezelése

Magas hiányarányú oszlopok eldobása (pl. xG)

Egyéb NaN értékek mediánnal való helyettesítése

Szöveges változók numerikus átalakítása (pl. ligahelyezés → szám)

Kiugró értékek kezelése (nulla meccsszámú játékosok elhagyása)

# Feature engineering

Célváltozó: következő szezon góljainak száma (Goals_next_season)

Új változók:

Korcsoportok (young, peak, veteran, old) → one-hot encoding

Életkor × percek interakció

Átlagos játékperc meccsenként

Játékosonkénti szezonadatok eltolása (shift), hogy a modell ténylegesen múltból jövőt tanuljon

# Modellépítés
# Alkalmazott algoritmusok

Random Forest Regressor

baseline modell

MAE = 2.32, RMSE = 3.54, R² = 0.23

XGBoost Regressor

boosting modell, faalapú tanulókkal

MAE = 2.31, RMSE = 3.61, R² = 0.20

Hiperparaméter-optimalizált XGBoost

GridSearchCV + keresztvalidáció

MAE = 2.24, R² = 0.26

# Szegmentálás

Játékosok gólerősség szerinti csoportosítása (0–5, 5–10, 10–15, 15–20, 20+ gól)

A 0–5 gólos csoporton külön modell → MAE = 1.39, R² = 0.27

Eredmény: a szegmentált modellek sem hoztak jelentős előrelépést az egyszerű átlaghoz képest

# Következtetések

A múltbeli statisztikák alapján a gólszám előrejelzése korlátozott pontossággal lehetséges.

A legjobb modell (XGBoost optimalizálva) is csak mérsékelt javulást ért el az átlagbecsléshez képest.

A prediktív erő korlátait az okozza, hogy a góltermelést erősen befolyásolják külső, nehezen kvantifikálható tényezők (sérülés, taktika, csapatszerep, átigazolás, mentális állapot).

# Fejlesztési lehetőségek

Több és jobb minőségű adat integrálása (pl. poszt, taktikai szerep, lövések minősége)

Más bajnokságok adatainak bevonása

Fejlettebb modellek kipróbálása (pl. neurális hálózatok, deep learning)

Alcsoportokra bontott predikciók fejlesztése, nagyobb mintaszám mellett

# Használt technológiák

Python 3

Könyvtárak: pandas, numpy, selenium, beautifulsoup4, scikit-learn, xgboost, matplotlib
