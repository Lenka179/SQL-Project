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

Sekundární tabuka propojuje agregované údaje o cenách a mzdách podle kalendářního roku s některými makroekonomickými ukazateli. Slouží jako datový základ pro zodpovězení závěrečné výzkumné otázky.  
Pro účely případného rozšíření analýzy byla v tabulce ponechána i další ekonomická data (např. GINI ukazatel, populace apod.)  
Agregace byla provedena pouze v nezbytném rozsahu, aby byla zachována možnost další manipulace s daty dle specifického zadání. 
Tabulka byla vytvořena pomocí operace INNER JOIN mezi hlavními datovými zdroji, abychom zachovali pouze data za stejné období jako primární přehled za Českou Republiku.  

*Příkaz k vytvoření tabulky:*
```
CREATE TABLE t_lenka_stankova_project_sql_secondary_final AS 
WITH price_prepared AS ( 
	SELECT
		category_code,
		date_part('YEAR', date_from) AS price_year,
		avg(value) AS average_price,
		cpc2.name AS category_name
	FROM czechia_price
	LEFT JOIN czechia_price_category AS cpc2 
		ON category_code = cpc2.code
	GROUP BY 
		category_code,
		date_part('YEAR', date_from),
		cpc2.name
),
payroll_prepared AS ( 
	SELECT
		cp.value_type_code,
		cp.unit_code,
		cp.calculation_code,
		cp.industry_branch_code,
		cp.payroll_year,
		cpib.name AS industry_name,
		cpu.name AS unit_name,
		cpvt.name AS value_type_name,
		cpc.name AS calculation_name,
		avg(cp.value) AS average_payroll
	FROM czechia_payroll AS cp
	LEFT JOIN czechia_payroll_unit AS cpu
		ON unit_code = cpu.code
	LEFT JOIN czechia_payroll_value_type AS cpvt 
		ON cp.value_type_code = cpvt.code 
	LEFT JOIN czechia_payroll_calculation AS cpc 
		ON cp.calculation_code = cpc.code 
	LEFT JOIN czechia_payroll_industry_branch AS cpib 
		ON cp.industry_branch_code = cpib.code 
	WHERE value_type_code = '5958'
	GROUP BY 
		value_type_code,
		unit_code,
		calculation_code,
		industry_branch_code,
		payroll_year,
		industry_name,
		unit_name,
		value_type_name,
		calculation_name
	)
SELECT
	pp.category_code,
	pp.average_price,
	pp.category_name,
	pp2.value_type_code,
	pp2.unit_code,
	pp2.calculation_code,
	pp2.industry_branch_code,
	pp2.average_payroll,
	pp2.unit_name,
	pp2.industry_name,
	pp2.value_type_name,
	pp2.calculation_name,
	e.country,
	e.YEAR,
	e.gdp,
	e.population,
	e.gini,
	e.taxes,
	e.fertility,
	e.mortaliy_under5 AS mortality_under5
	FROM economies AS e
INNER JOIN price_prepared AS pp
	ON e.YEAR = pp.price_year  
INNER JOIN payroll_prepared AS pp2 
	ON e.YEAR = pp2.payroll_year
WHERE country IN (
  'Albania', 'Andorra', 'Armenia', 'Austria', 'Belarus', 'Belgium',
  'Bosnia and Herzegovina', 'Bulgaria', 'Channel Islands', 'Croatia',
  'Cyprus', 'Czech Republic', 'Denmark', 'Estonia', 'Faroe Islands',
  'Finland', 'France', 'Georgia', 'Germany', 'Gibraltar', 'Greece',
  'Hungary', 'Iceland', 'Ireland', 'Italy', 'Kosovo', 'Latvia',
  'Liechtenstein', 'Lithuania', 'Luxembourg', 'Malta', 'Moldova',
  'Monaco', 'Montenegro', 'Netherlands', 'North Macedonia', 'Norway',
  'Poland', 'Portugal', 'Romania', 'San Marino', 'Serbia', 'Slovakia',
  'Slovenia', 'Spain', 'Sweden', 'Switzerland', 'Ukraine', 'United Kingdom'
)
;
```

## 3. Odpovědi na otázky

## 3.1 Rostou v průběhu let mzdy ve všech odvětvích, nebo v některých klesají?  

- Ano, obecně lze říci, že průměrné mzdy ve všech odvětvích **rostou**.
- Přičemž odvětví ***Ubytování, stravování a pohostinství*** eviduje nejnižší průměrný celkový mzdový nárůst a naopak nejvyšší celkový mzdový nárůst za sledované období vidíme v oblasti ***Informační a komunikační činnosti***.  
- Za výjimku bychom mohli považovat rok 2013, kdy v 11 z 19 sledovaných odvětví mzdy klesly. Největší meziroční propad průměrné mzdy zasáhl oblast Peněžnicvtí a pojišťovnictví. Příčinou by mohla být ekonomická recese ČR, která souvisela s dluhovou krizí v eurozóně a vládními úspornými opatřeními.
- Naopak nejvýraznější meziroční nárůst mezd byl zaznamenán v roce 2021 v sektoru Zdravotní a sociální péče. Tento skokový nárůst lze vysvětlit mimořádnými odměnami, které vláda ČR schválila jako poděkování lékařům a zdravotnickému personálu za jejich péči o pacienty s onemocněním Covid-19.

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
	pc.*,
	ps.overall_payroll_trend
FROM payroll_comparison AS pc 
LEFT JOIN payroll_sum AS ps
	ON  pc.industry_branch_code = ps.industry_branch_code			
ORDER BY 
	overall_payroll_trend, 
	industry_branch_code, 
	payroll_year
;
```

## 3.2 Kolik je možné si koupit litrů mléka a kilogramů chleba za první a poslední srovnatelné období v dostupných datech cen a mezd?  

- V otázce není definované v jakém podílu má být rozdělena mzda mezi komodity, proto ji rozdělíme v poměru 50 : 50.  
- Do srovnatelného období můžeme zahrnout pouze odbdobí 2006 až 2018. Mimo toto období máme k dispozici data o průměrných mzdách, ale nemáme dostupná data o cenách potravin.
- V roce **2006** jsme si mohli za průměrnou mzdu 20678 Kč koupit **641 kilogramů chleba a 716 litrů mléka**.
- V roce **2018** jsme si mohli za průměrnou mzdu 32486 Kč koupit **670 kilogramů chleba a 820 litrů mléka**.

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
	AND category_code IN ('114201','111301')
	AND payroll_year IN ('2006','2018')
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
- Nejpomaleji zdražuje kategorie s názvem **Banány žluté**. Relativní percentuální nárůst ceny za celé období je **0,56 %**.
- Z dostupných dat je dokonce zřejmé, že kategorie *Rajská jablka červená kulatá* a *Cukr krystalový* zaznamenávají relativní propad ceny (-2,73 resp. -2,43 %).

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
- **V žádném ze sledovaných období nebyl meziroční nárůst cen potravin vyšší alespoň o 10 % oproti růstu mezd.**
- Nejvyšší meziroční zvýšení cen potravin bylo v roce 2017 o 9,63 %, ale v tomtéž roce došlo také k nárůstu průměrných mezd o 6,31 %.

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

- Na první pohled se může zdát, že výše HDP nemá přímý vliv na růst mezd ani cen potravin. Pokud však použijeme analytický nástroj  ***Pearsonovu korelaci***, zjistíme, že **HDP a průměrné mzdy vykazují silnou pozitivní korelaci** (0,92). To naznačuje, že růst HDP je spojen s růstem mezd.  
- Podobně i korelace mezi **HDP a průměrnými cenami** (0,89) potravin ukazuje, že ekonomický růst **může ovlivňovat cenovou hladinu**.
- Je však třeba mít na paměti, že korelace neprokazuje kauzalitu (příčinný vztah) — roli zde mohou hrát i další faktrory.
  
  	> **Pearsonova korelace** měří sílu lineárního vztahu mezi dvěma proměnnými v celé datové sadě. Hodnota korelačního koeficientu leží v intervalu -1 do 1. Krajní hodnoty blízké -1 nebo 1 značí silnou lineární korelaci, zatímco hodnoty blízké nule poukazují na velmi slabou nebo žádnou lineární závislost. Obecně lze korelaci interpretovat následovně:
  > 
  > + 0,00 - ±0,19 velmi slabá  
  > + ±0,20 - ±0,39 slabá  
  > + ±0,40 - ±0,59 střední  
  > + ±0,60 - ±0,79 silná  
  > + ±0,80 - ±1,00 velmi silná  

```
WITH averaged_data AS (
  SELECT
    YEAR,
    country,
    avg(average_price) AS overall_avg_price,
    avg(average_payroll) AS overall_avg_payroll,
    avg(gdp) AS gdp
  FROM t_lenka_stankova_project_sql_secondary_final
  WHERE country = 'Czech Republic'
  GROUP BY YEAR, country
)
SELECT
  round(corr(gdp, overall_avg_price)::NUMERIC, 2) AS corr_gdp_price,
  round(corr(gdp, overall_avg_payroll)::NUMERIC, 2) AS corr_gdp_payroll
FROM averaged_data;
```

---
***Dokument vytvořila Lenka Staňková, září 2025.***

