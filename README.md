# SQL-Projekt

Obsah  
- [1. Popis projektu](https://github.com/Lenka179/SQL-Project/blob/main/README.md#1-popis-projektu "Popis projektu")  
- [2. Popis primární a sekundární tabulky](https://github.com/Lenka179/SQL-Project/blob/main/README.md#2-popis-prim%C3%A1rn%C3%AD-a-sekund%C3%A1rn%C3%AD-tabulky "Primární a sekundární tabulka")  
	- [2.1 Primární tabulka](https://github.com/Lenka179/SQL-Project/blob/main/README.md#21-prim%C3%A1rn%C3%AD-tabulka "Primární tabulka")  
	- [2.2 Sekundární tabulka](https://github.com/Lenka179/SQL-Project/blob/main/README.md#22-sekund%C3%A1rn%C3%AD-tabulka "Sekundární tabulka")  
- [3. Odpovědi na otázky](https://github.com/Lenka179/SQL-Project/blob/main/README.md#3-odpov%C4%9Bdi-na-ot%C3%A1zky "Odpodovědi na otázky")  
	- [3.1 Rostou v průběhu let mzdy ve všech odvětvích, nebo v některých klesají?](https://github.com/Lenka179/SQL-Project/blob/main/README.md#31-rostou-v-pr%C5%AFb%C4%9Bhu-let-mzdy-ve-v%C5%A1ech-odv%C4%9Btv%C3%ADch-nebo-v-n%C4%9Bkter%C3%BDch-klesaj%C3%AD "Růst mezd")
 	- [3.2 Kolik je možné si koupit litrů mléka a kilogramů chleba za první a poslední srovnatelné období v dostupných datech cen a mezd?](https://github.com/Lenka179/SQL-Project/blob/main/README.md#32-kolik-je-mo%C5%BEn%C3%A9-si-koupit-litr%C5%AF-ml%C3%A9ka-a-kilogram%C5%AF-chleba-za-prvn%C3%AD-a-posledn%C3%AD-srovnateln%C3%A9-obdob%C3%AD-v-dostupn%C3%BDch-datech-cen-a-mezd "Chléb a mléko")  
	- [3.3 Která kategorie potravin zdražuje nejpomaleji (je u ní nejnižší percentuální meziroční nárůst)?](https://github.com/Lenka179/SQL-Project/blob/main/README.md#33-kter%C3%A1-kategorie-potravin-zdra%C5%BEuje-nejpomaleji-je-u-n%C3%AD-nejni%C5%BE%C5%A1%C3%AD-percentu%C3%A1ln%C3%AD-meziro%C4%8Dn%C3%AD-n%C3%A1r%C5%AFst "Nejpomalejší zdražování")  
	- [3.4 Existuje rok, ve kterém byl meziroční nárůst cen potravin výrazně vyšší než růst mezd (větší než 10 %)?](https://github.com/Lenka179/SQL-Project/blob/main/README.md#34-existuje-rok-ve-kter%C3%A9m-byl-meziro%C4%8Dn%C3%AD-n%C3%A1r%C5%AFst-cen-potravin-v%C3%BDrazn%C4%9B-vy%C5%A1%C5%A1%C3%AD-ne%C5%BE-r%C5%AFst-mezd-v%C4%9Bt%C5%A1%C3%AD-ne%C5%BE-10- "Růst cen a mezd")  
	- [3.5 Má výška HDP vliv na změny ve mzdách a cenách potravin? Neboli, pokud HDP vzroste výrazněji v jednom roce, projeví se to na cenách potravin či mzdách ve stejném nebo následujícím roce výraznějším růstem?](https://github.com/Lenka179/SQL-Project/blob/main/README.md#35-m%C3%A1-v%C3%BD%C5%A1ka-hdp-vliv-na-zm%C4%9Bny-ve-mzd%C3%A1ch-a-cen%C3%A1ch-potravin-neboli-pokud-hdp-vzroste-v%C3%BDrazn%C4%9Bji-v-jednom-roce-projev%C3%AD-se-to-na-cen%C3%A1ch-potravin-%C4%8Di-mzd%C3%A1ch-ve-stejn%C3%A9m-nebo-n%C3%A1sleduj%C3%ADc%C3%ADm-roce-v%C3%BDrazn%C4%9Bj%C5%A1%C3%ADm-r%C5%AFstem "Porovnání HDP cen a mezd")
   		- [Korelace](https://github.com/Lenka179/SQL-Project/blob/main/README.md#korelace "Korelace")
       	- [Regresní analýza](https://github.com/Lenka179/SQL-Project/blob/main/README.md#regresn%C3%AD-anal%C3%BDza "Regresní analýza")
   
## 1. Popis projektu 

Cílem projektu je zjistit dostupnost základních potravin pro širokou veřejnost. K dosažení cíle bylo stanoveno 5 výzkumných otázek týkajících se průměrných mezd, cen potravin a ekonomiky České Republiky.  
Pro získání odpovědí byly vytvořeny dvě zdrojové datové tabulky (primární a seknudární), ze kterých budeme čerpat konečná data pro zodpovězení otázek.  
Jako dodatečný materiál pro případný další výzkum byly v sekundární tabulce ponechány makroekonomické informace o dalších evropských státech ve stejném období jako primární přehled pro Českou Republiku.


## 2. Popis primární a sekundární tabulky

### 2.1 Primární tabulka

Primární tabulka ***t_lenka_stankova_project_sql_primary_final*** vznikla spojením tabulky o průměrných mzdách s tabulkou průměrných cen potravin. Finální tabulka byla také následně propojena s dimenzionálními tabulkami, které s ní souvisí. Data v ní nám následně pomohou zodpovědět otázky 1 až 4.  
Aby nedošlo ke ztrátě dat na žádné straně spojení mezi hlavními datovými zdroji, zvolila jsem FULL OUTER JOIN. Data o mzdách máme totiž k dispozici od roku 2000 do 2021, ale data o cenách potravin pouze mezi roky 2006 a 2018.

*Příkaz k vytvoření tabulky:*
```
CREATE TABLE  t_lenka_stankova_project_sql_primary_final AS 
WITH cp_prepared AS ( 
	SELECT 
		payroll_year,
		unit_code,
		calculation_code,
		industry_branch_code,
		avg(value) AS average_payroll
	FROM czechia_payroll
	WHERE value_type_code = 5958
	GROUP BY
		payroll_year,
		unit_code,
		calculation_code,
		industry_branch_code
),
cp2_prepared AS (
	SELECT 
		date_part('YEAR', date_from) AS year,
		category_code,
		region_code,
		avg(value) AS average_price
	FROM czechia_price
	GROUP BY 		
		date_part('YEAR', date_from),
		category_code,
		region_code
)	
SELECT 
    cp.payroll_year,
    cp.unit_code,
    cp.calculation_code,
    cp.industry_branch_code,
    cp.average_payroll,
    cp2.category_code AS category_code,
    cp2.region_code,
    cp2.average_price,
    cpc.price_unit AS price_unit,
    cpc.name AS category_name,
    cpc.price_value,
    cpib.name AS branch_name
FROM cp_prepared AS cp
FULL OUTER JOIN cp2_prepared AS cp2 
    ON cp2.year = cp.payroll_year
LEFT JOIN czechia_price_category AS cpc 
    ON cp2.category_code = cpc.code 
LEFT JOIN czechia_payroll_industry_branch AS cpib
    ON cp.industry_branch_code = cpib.code 
;
```

### 2.2 Sekundární tabulka

Sekundární tabuka propojuje údaje o cenách a mzdách z primární tabulky podle kalendářního roku s některými makroekonomickými ukazateli. Slouží jako datový základ pro zodpovězení závěrečné výzkumné otázky.  
Výběr evropských států je proveden přes propojení s tabulkou všech států světa a časové rozpětí je stejné jako primární přehled pro Českou Republiku.  
Pro účely případného rozšíření analýzy byla v tabulce ponechána i další ekonomická data (např. GINI ukazatel, populace apod.)  

*Příkaz k vytvoření tabulky:*
```
CREATE TABLE  t_lenka_stankova_project_sql_secondary_final  AS
WITH europe_countries AS ( 
	SELECT DISTINCT 
		country
	FROM countries
	WHERE continent  = 'Europe'
)
SELECT 
	e.country,
	e.YEAR,
	e.gdp,
	e.population,
	e.gini,
	e.taxes,
	e.fertility,
	e.mortaliy_under5,
	tlsp.unit_code,
	tlsp.calculation_code,
	tlsp.industry_branch_code,
	tlsp.category_code,
	tlsp.region_code,
	tlsp.price_unit,
	tlsp.category_name,
	tlsp.price_value,
	tlsp.branch_name,
	tlsp.average_payroll,
	tlsp.average_price
FROM economies AS e 
INNER JOIN europe_countries 
	ON e.country = europe_countries.country
LEFT JOIN t_lenka_stankova_project_sql_primary_final AS tlsp
	ON e.YEAR = tlsp.payroll_year AND e.country = 'Czech Republic'
WHERE e.YEAR BETWEEN 2000 AND 2021
;
```

## 3. Odpovědi na otázky

## 3.1 Rostou v průběhu let mzdy ve všech odvětvích, nebo v některých klesají?  

- Ano, obecně lze říci, že průměrné mzdy ve všech odvětvích **rostou**.
- Přičemž odvětví ***Ubytování, stravování a pohostinství*** eviduje nejnižší průměrný celkový mzdový nárůst a naopak nejvyšší celkový mzdový nárůst za sledované období vidíme v oblasti ***Informační a komunikační činnosti***.
  
	> Změníme-li pořadí řazení v dotazu, získáme další zajímavé informace: 
	> + Za výjimku bychom mohli považovat rok 2013, kdy v 11 z 19 sledovaných odvětví mzdy klesly.
 	> + Největší meziroční propad průměrné mzdy zasáhl oblast Peněžnicvtí a pojišťovnictví. Příčinou by mohla být ekonomická recese ČR, která souvisela s dluhovou krizí v eurozóně a vládními úspornými opatřeními.   
	> + Naopak nejvýraznější meziroční nárůst mezd byl zaznamenán v roce 2021 v sektoru Zdravotní a sociální péče. Tento skokový nárůst lze vysvětlit mimořádnými odměnami, které vláda České Republiky schválila jako poděkování lékařům a zdravotnickému personálu za jejich péči o pacienty s onemocněním Covid-19.
 
```
WITH prev_average_payroll AS ( 
	SELECT 	
		payroll_year,
		industry_branch_code,
		avg(average_payroll) AS previous_avg_payroll
	FROM t_lenka_stankova_project_sql_primary_final
	GROUP BY 
		payroll_year,
		industry_branch_code
),
current_payroll AS ( 
	SELECT 
		payroll_year,
		industry_branch_code,
		branch_name,
		avg(average_payroll) AS current_avg_payroll
	FROM t_lenka_stankova_project_sql_primary_final
	GROUP BY 
		payroll_year,
		industry_branch_code,
		branch_name
),
payroll_comparison AS ( 
	SELECT 
		cp.payroll_year,
		cp.industry_branch_code,
		cp.branch_name,
		cp.current_avg_payroll,
		pap.previous_avg_payroll,
		cp.current_avg_payroll - pap.previous_avg_payroll AS payroll_diff,
		CASE 
			WHEN cp.current_avg_payroll > pap.previous_avg_payroll THEN 'higher'
			WHEN cp.current_avg_payroll < pap.previous_avg_payroll THEN 'lower'
			WHEN pap.previous_avg_payroll IS NULL THEN 'null'
			ELSE 'equal'	
		END AS annual_payroll_trend	
	FROM current_payroll AS cp
	LEFT JOIN prev_average_payroll AS pap
		ON cp.industry_branch_code = pap.industry_branch_code 
		AND cp.payroll_year = pap.payroll_year +1
),
payroll_sum AS ( 
	SELECT 
		cp.industry_branch_code,
		sum(cp.current_avg_payroll - pap.previous_avg_payroll) AS overall_payroll_trend
	FROM current_payroll AS cp 
	LEFT JOIN prev_average_payroll AS pap 
		ON cp.industry_branch_code = pap.industry_branch_code
		AND cp.payroll_year = pap.payroll_year +1
	GROUP BY cp.industry_branch_code
)
SELECT
	pc.payroll_year,
	pc.industry_branch_code,
	pc.branch_name,
	pc.current_avg_payroll,
	pc.previous_avg_payroll,
	pc.payroll_diff,
	pc.annual_payroll_trend,
	ps.overall_payroll_trend
FROM payroll_comparison AS pc 
LEFT JOIN payroll_sum AS ps
	ON  pc.industry_branch_code = ps.industry_branch_code			
ORDER BY 
	ps.overall_payroll_trend, 
	pc.industry_branch_code, 
	pc.payroll_year,
	pc.annual_payroll_trend,
	pc.payroll_diff
;
```

## 3.2 Kolik je možné si koupit litrů mléka a kilogramů chleba za první a poslední srovnatelné období v dostupných datech cen a mezd?  

- V otázce není definované v jakém podílu má být rozdělena mzda mezi komodity, proto ji rozdělíme v poměru 50 : 50.  
- Do srovnatelného období můžeme zahrnout pouze odbdobí 2006 až 2018. Mimo toto období máme k dispozici data o průměrných mzdách, ale nemáme dostupná data o cenách potravin.
- V roce **2006** jsme si mohli za průměrnou mzdu 20 678 Kč koupit **`641` kilogramů chleba a `716` litrů mléka**.
- V roce **2018** jsme si mohli za průměrnou mzdu 32 486 Kč koupit **`670` kilogramů chleba a `820` litrů mléka**.

```
SELECT 
	payroll_year,
	category_name,
	price_unit,
	round(avg(average_payroll) / 2, 0) AS total_average_payroll,
	round((avg(average_payroll) / 2) / avg(average_price)::NUMERIC, 0) AS count_comodity_per_payroll
FROM t_lenka_stankova_project_sql_primary_final
WHERE average_payroll  IS NOT NULL 
	AND average_price  IS NOT NULL 
	AND category_code IN (114201,111301)
	AND payroll_year IN (2006,2018)
GROUP BY
	payroll_year,
	category_name,
	price_unit,
	price_value 
ORDER BY payroll_year 
;
```

## 3.3 Která kategorie potravin zdražuje nejpomaleji (je u ní nejnižší percentuální meziroční nárůst)?

- Chceme-li zjistit, která kategorie potravin zdražuje nejpomaleji, nestačí vyhodnotit nejnižší percentuální meziroční nárůst ceny, protože tato hodnota nezohledňuje kontext měřeného období (vývoj průměrných cen v průběhu let).
- Proto jsem zvolila vhodnější variantu výpočtu a to **percentuální relativní růst ceny vůči průměrné ceně kategorie**. 
- Nejpomaleji zdražuje kategorie s názvem **Banány žluté**. Relativní percentuální nárůst ceny za celé období je `0,56 %`.
- Z dostupných dat je dokonce zřejmé, že kategorie *Rajská jablka červená kulatá* a *Cukr krystalový* zaznamenávají relativní propad ceny (`-2,73` a `-2,43 %`).

```
WITH price_diffs AS (
    SELECT 
        tlsp.category_code,
        tlsp.category_name,
        tlsp.payroll_year,
        avg(tlsp.average_price) AS avg_price,
        LAG(avg(tlsp.average_price)) OVER (
            PARTITION BY tlsp.category_code 
            ORDER BY tlsp.payroll_year
        ) AS previous_avg_price
    FROM t_lenka_stankova_project_sql_primary_final AS tlsp
    WHERE tlsp.category_name IS NOT NULL
    GROUP BY
		tlsp.category_code,
		tlsp.category_name,
		tlsp.payroll_year
)
SELECT 
    category_code,
    category_name,
    round(avg(avg_price)::NUMERIC, 2) AS overall_avg_price,
    round(avg(avg_price - previous_avg_price)::NUMERIC, 2) AS avg_price_change,
    CASE 
        WHEN avg(avg_price) != 0 THEN 
            round((avg(avg_price - previous_avg_price)::NUMERIC) / avg(avg_price::NUMERIC) * 100, 2)
        ELSE NULL
    END AS relative_growth_percent
FROM price_diffs
GROUP BY
	category_code,
	category_name
ORDER BY relative_growth_percent ASC
; 
```

## 3.4 Existuje rok, ve kterém byl meziroční nárůst cen potravin výrazně vyšší než růst mezd (větší než 10 %)?

- Z dostupných dat vyplývá, že pro analýzu lze použít pouze období 2006–2018, kdy máme dostupná data o mzdách i cenách.  
- **V žádném ze sledovaných období nebyl meziroční nárůst cen potravin vyšší než 10 % oproti růstu mezd.**
- Nejvyšší meziroční zvýšení cen potravin bylo v roce 2017 o `9,63 %`, ale v tomtéž roce došlo také k nárůstu průměrných mezd o `6,31 %`.

```
SELECT 
	payroll_year,
    round(((avg(average_price) - 
           LAG(avg(average_price)) OVER (ORDER BY payroll_year)) / 
           NULLIF(LAG(avg(average_price)) OVER (ORDER BY payroll_year), 0) * 100) ::NUMERIC, 2) AS price_percentage_change,
    round((avg(average_payroll) - 
           LAG(avg(average_payroll)) OVER (ORDER BY payroll_year)) / 
           NULLIF(LAG(avg(average_payroll)) OVER (ORDER BY payroll_year), 0) * 100, 2) AS payroll_percentage_change
FROM t_lenka_stankova_project_sql_primary_final
WHERE payroll_year BETWEEN 2006 AND 2018
GROUP BY payroll_year
ORDER BY price_percentage_change DESC
;
```

## 3.5 Má výška HDP vliv na změny ve mzdách a cenách potravin? Neboli, pokud HDP vzroste výrazněji v jednom roce, projeví se to na cenách potravin či mzdách ve stejném nebo následujícím roce výraznějším růstem?

Na první pohled se může zdát, že výše HDP nemá přímý dopad na růst mezd ani cen potravin. Použitím vybraných analytických metod však docházíme k opačnému závěru.  
Pomocí dotazu jsem omezila data pouze na Českou Republiku a roky 2006 až 2018, protože pouze v tomto rozmezí máme dostupná data o mzdách i cenách potravin. 

V rámci analýzy jsem využila dvě metody: **Pearsonovu korelaci** a **lineární regresi**, včetně jejich **lagovaných variant**, které zohledňují časové zpoždění mezi proměnnými.

- **Pearsonova korelace** slouží k identifikaci statisticky významného vztahu mezi HDP a průměrnými mzdami či cenami potravin.
- **Lineární regrese** kvantifikuje sílu tohoto vztahu – tedy o kolik se změní závislá proměnná (mzdy nebo ceny), pokud se HDP zvýší o jednotku.
- **Lagované varianty** zkoumají, zda se vliv HDP projeví s časovým odstupem.

Pro lepší interpretaci výsledků jsem HDP převedla na miliardy korun.

### Korelace

- **Pearsonova korelace** mezi HDP a průměrnými mzdami dosahuje hodnoty `0.92`, což značí velmi silnou souvislost. Korelace mezi HDP a cenami potravin je `0.89`, což naznačuje, že ekonomický růst může ovlivňovat cenovou hladinu.
- **Lagovaná korelace** potvrzuje meziroční vliv HDP. Se mzdami dostahuje korelační koeficient hodnoty `0.95` a s cenami potravin hodnoty `0.83`.
- Je však důležité zdůraznit, že korelace sama o sobě neprokazuje kauzalitu – výsledky mohou být ovlivněny i dalšími faktory.

### Regresní analýza

- Pokud HDP vzroste o **1 miliardu Kč**, průměrná mzda se ve stejném roce zvýší o `177,24 Kč` a průměrná cena potravin o `0.3 Kč`.
- V případě **lagované regrese** se ukazuje, že HDP z předchozího roku ovlivňuje průměrnou mzdu v následujícím roce o `201,68 Kč` a průměrnou cenu potravin opět o `0.32 Kč`.

### Interpretace korelačního koeficientu

| Hodnota korelace | Interpretace       |
|------------------|--------------------|
| 0.00 – ±0.19     | velmi slabá        |
| ±0.20 – ±0.39    | slabá              |
| ±0.40 – ±0.59    | střední            |
| ±0.60 – ±0.79    | silná              |
| ±0.80 – ±1.00    | velmi silná        |

### Definice použitých metod

- **Pearsonova korelace** je statistická metoda, která měří sílu a směr lineárního vztahu mezi dvěma číselnými proměnnými. Hodnota korelačního koeficientu se pohybuje v intervalu od -1 do 1. Hodnoty blízké ±1 značí silnou lineární závislost, zatímco hodnoty blízké nule ukazují na slabý nebo žádný lineární vztah.

- **Lagovaná korelace** zkoumá statistickou souvislost mezi hodnotou jedné proměnné v daném období a hodnotou druhé proměnné v následujícím období.

- **Lineární regrese** je statistická a analytická metoda, která ve své nejjednodušší podobě zkoumá vztah mezi dvěma proměnnými: nezávislou proměnnou, která má ovlivňovat závislou proměnnou. Tuto metodu lze využít i k predikci budoucích hodnot závislé proměnné.

- **Lagovaná regrese** je rozšířená forma lineární regrese, která zohledňuje časové zpoždění mezi proměnnými – tedy vliv, který se neprojeví okamžitě, ale až v následujícím období.


```
WITH aggregated AS ( 
	SELECT 
		year,
		country,
		gdp AS avg_gdp,
		avg(average_payroll) AS avg_payroll,
		avg(average_price) AS avg_price
	FROM t_lenka_stankova_project_sql_secondary_final
	WHERE year BETWEEN 2006 AND 2018 AND country = 'Czech Republic'
	GROUP BY YEAR, country, gdp
),
lagged AS ( 
	SELECT 
		YEAR,
		avg_gdp,
		LEAD(avg_payroll) OVER (ORDER BY year) AS payroll_next_year,
		LEAD(avg_price) OVER (ORDER BY year) AS price_next_year
	FROM aggregated
),
regression AS ( 
	SELECT 
		round(REGR_SLOPE(avg_payroll, avg_gdp / 1000000000)::NUMERIC, 2) AS reg_payroll_vs_gdp,
		round(REGR_SLOPE(avg_price, avg_gdp / 1000000000)::NUMERIC, 2) AS reg_price_vs_gdp,
		round(CORR(avg_payroll, avg_gdp / 1000000000)::NUMERIC, 2) AS corr_payroll_vs_gdp,
		round(CORR(avg_price, avg_gdp / 1000000000)::NUMERIC, 2) AS corr_price_vs_gdp
FROM aggregated 
),
reg_corr_lagged AS ( 
	SELECT
		round(REGR_SLOPE(payroll_next_year, avg_gdp / 1000000000)::NUMERIC, 2) AS reg_payroll_lagged,
		round(REGR_SLOPE(price_next_year, avg_gdp / 1000000000)::NUMERIC, 2) AS reg_price_lagged,
		round(CORR(payroll_next_year, avg_gdp / 1000000000)::NUMERIC, 2) AS corr_payroll_lagged,
		round(CORR(price_next_year, avg_gdp / 1000000000)::NUMERIC, 2) AS corr_price_lagged
	FROM lagged 
	WHERE payroll_next_year IS NOT NULL AND price_next_year IS NOT NULL 
)
SELECT 
	rs.corr_payroll_vs_gdp,
	rs.corr_price_vs_gdp,
	rl.corr_payroll_lagged,
	rl.corr_price_lagged,
	rs.reg_payroll_vs_gdp,
	rs.reg_price_vs_gdp,
	rl.reg_payroll_lagged,
	rl.reg_price_lagged
FROM regression AS rs
CROSS JOIN reg_corr_lagged AS rl
;
```

---
***Dokument vytvořila Lenka Staňková, září 2025.***

