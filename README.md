# SQL-Project

## 1. Popis projekut (Proč se to dělá?)

Cílem projektu je zjistit dostupnost základních potravin široké veřejnosti a zodpovědět několik konkrétních otázek týkajících se průměrných mezd a cen potravin v České Republice.  
Pro získání odpovědí byly vytvořeny dvě zdrojové tabulky (primární a seknudární), ze kterých budeme čerpat konečná data pro naše odpovědi. 

Výzkumné otázky:    
[1. Rostou v průběhu let mzdy ve všech odvětvích, nebo v některých klesají?](#1-rostou-v-pr%C5%AFb%C4%9Bhu-let-mzdy-ve-v%C5%A1ech-odv%C4%9Btv%C3%ADch-nebo-v-n%C4%9Bkter%C3%BDch-klesaj%C3%AD)  
[2. Kolik je možné si koupit litrů mléka a kilogramů chleba za první a poslední srovnatelné období v dostupných datech cen a mezd?](#2-kolik-je-mo%C5%BEn%C3%A9-si-koupit-litr%C5%AF-ml%C3%A9ka-a-kilogram%C5%AF-chleba-za-prvn%C3%AD-a-posledn%C3%AD-srovnateln%C3%A9-obdob%C3%AD-v-dostupn%C3%BDch-datech-cen-a-mezd)  
[3. Která kategorie potravin zdražuje nejpomaleji (je u ní nejnižší percentuální meziroční nárůst)?](#3-kter%C3%A1-kategorie-potravin-zdra%C5%BEuje-nejpomaleji-je-u-n%C3%AD-nejni%C5%BE%C5%A1%C3%AD-percentu%C3%A1ln%C3%AD-meziro%C4%8Dn%C3%AD-n%C3%A1r%C5%AFst)  
[4. Existuje rok, ve kterém byl meziroční nárůst cen potravin výrazně vyšší než růst mezd (větší než 10 %)?](#4-existuje-rok-ve-kter%C3%A9m-byl-meziro%C4%8Dn%C3%AD-n%C3%A1r%C5%AFst-cen-potravin-v%C3%BDrazn%C4%9B-vy%C5%A1%C5%A1%C3%AD-ne%C5%BE-r%C5%AFst-mezd-v%C4%9Bt%C5%A1%C3%AD-ne%C5%BE-10-)  
[5. Má výška HDP vliv na změny ve mzdách a cenách potravin? Neboli, pokud HDP vzroste výrazněji v jednom roce, projeví se to na cenách potravin či mzdách ve stejném nebo následujícím roce výraznějším růstem?](#5-m%C3%A1-v%C3%BD%C5%A1ka-hdp-vliv-na-zm%C4%9Bny-ve-mzd%C3%A1ch-a-cen%C3%A1ch-potravin-neboli-pokud-hdp-vzroste-v%C3%BDrazn%C4%9Bji-v-jednom-roce-projev%C3%AD-se-to-na-cen%C3%A1ch-potravin-%C4%8Di-mzd%C3%A1ch-ve-stejn%C3%A9m-nebo-n%C3%A1sleduj%C3%ADc%C3%ADm-roce-v%C3%BDrazn%C4%9Bj%C5%A1%C3%ADm-r%C5%AFstem)  

## 2. Popis primární a sekundární tabulky

### Primární tabulka

V primární tabulce jsou spojena data z přehledu o průměrných mzdách v jednotlivých odvětvích a průměrných cenách sledovaných komodit. Data v ní nám následně pomohou zodpovědět otázky 1 až 4.  
Spojení tabulek o mzdách a cenách bylo provedeno přes LEFT JOIN, protože data o mzdách máme k dispozici od roku 2000 do roku 2021, ale data pro ceny potravin zahrnují pouze období 2006 až 2018.
Zároveň jsou ve zdrojových datech informace o průměrných mzdách bez uvedení kategorie. Tyto hodnoty byly dodatečně označeny kategorií "UNCLASSIFIED".

*Příkaz k vytvoření tabulky:*
```
CREATE TABLE t_lenka_stankova_project_sql_primary_final(
	payroll_year INTEGER,
	average_payroll NUMERIC(10,0),
	industry_branch_code TEXT,
	industry_branch_name TEXT,
	category_code NUMERIC,
	average_price NUMERIC(10,2),
	category_name TEXT,
	price_unit TEXT,
	price_value NUMERIC
);
```
*Příkaz k naplnění tabulky:*
```
INSERT INTO t_lenka_stankova_project_sql_primary_final 
SELECT 
    cp.payroll_year,
    avg(cp.value)::NUMERIC  AS average_payroll,
    COALESCE (cp.industry_branch_code, 'null')  AS industry_branch_code,
    COALESCE (cpib.name, 'UNCLASSIFIED') AS industry_branch_name,
    cp2.category_code AS category_code,
    round(avg(cp2.value)::NUMERIC,2) AS average_price,
    cpc.name AS category_name,
    cpc.price_unit AS price_unit,
    cpc.price_value::NUMERIC AS price_value
FROM czechia_payroll AS cp
LEFT JOIN czechia_payroll_industry_branch AS cpib
    ON cp.industry_branch_code = cpib.code 
LEFT JOIN czechia_price AS cp2 
    ON date_part('YEAR', cp2.date_from) = cp.payroll_year
LEFT JOIN czechia_price_category AS cpc 
    ON cp2.category_code = cpc.code 
WHERE cp.value_type_code = 5958
GROUP BY cp.payroll_year,
	COALESCE (cp.industry_branch_code, 'null'),
	COALESCE (cpib.name, 'UNCLASSIFIED'),
    cp2.category_code,
    cpc.name,
    cpc.price_unit,
    cpc.price_value
;
```
*Primární tabulka:*
```
SELECT*
FROM t_lenka_stankova_project_sql_primary_final
;
```

### Sekundární tabulka

TODO

## 2. Odpovědi na otázky

## 1. Rostou v průběhu let mzdy ve všech odvětvích, nebo v některých klesají?  

- Ano, obecně lze říci, že průměrné platy ve všech odvětvích **rostou**.  
- Za výjimku bychom mohli považovat rok 2013, kdy v 11 z 19 sledovaných odvětví mzdy klesly. Největší meziroční propad průměrné mzdy zasáhl oblast Peněžnicvtí a pojišťovnictví. Příčinou by mohla být ekonomická recese ČR, která souvisela s dluhovou krizí v eurozóně a vládními úspornými opatřeními.
- Naopak nejvýraznější meziroční nárůst mezd byl zaznamenán v roce 2021 v sektoru Zdravotní a sociální péče. Tento skokový nárůst lze vysvětlit mimořádnými odměnami, které vláda ČR schválila jako výraz poděkování lékařům a zdravotnickému personálu za jejich péči o pacienty s onemocněním Covid-19.

```
SELECT
	tlsp.payroll_year,
	tlsp.industry_branch_code,
	tlsp.industry_branch_name,
	avg(tlsp.average_payroll) AS average_payroll,
	LAG(avg(tlsp.average_payroll)) OVER(
		PARTITION BY tlsp.industry_branch_code
		ORDER BY tlsp.payroll_year) AS previous_year_payroll,
	CASE 
		WHEN avg(tlsp.average_payroll) > LAG(avg(tlsp.average_payroll)) OVER (
			PARTITION BY tlsp.industry_branch_code
			ORDER BY tlsp.payroll_year) THEN 'higher'
		WHEN avg(tlsp.average_payroll) < LAG(avg(tlsp.average_payroll)) OVER (
			PARTITION BY tlsp.industry_branch_code
			ORDER BY tlsp.payroll_year) THEN 'lower'
		WHEN LAG(avg(tlsp.average_payroll)) OVER ( 
			PARTITION BY tlsp.industry_branch_code
			ORDER BY tlsp.payroll_year) IS NULL THEN 'null'
		ELSE 'equal'
	END AS payroll_trend,
	avg(tlsp.average_payroll) - 
		LAG(avg(tlsp.average_payroll)) OVER (
			PARTITION BY tlsp.industry_branch_code 
			ORDER BY tlsp.payroll_year) AS payroll_difference 
FROM t_lenka_stankova_project_sql_primary_final AS tlsp
GROUP BY tlsp.payroll_year,
	tlsp.industry_branch_code,
	tlsp.industry_branch_name
ORDER BY  industry_branch_code, payroll_year desc
;
```

## 2. Kolik je možné si koupit litrů mléka a kilogramů chleba za první a poslední srovnatelné období v dostupných datech cen a mezd?  

- V otázce není definované v jakém podílu má být rozdělena mzda mezi komodity, proto ji rozdělíme v poměru 50:50.  
- Do srovnatelného období můžeme zahrnout pouze odbdobí 2006 až 2018. Mimo toto období máme k dispozici data o průměrných mzdách, ale nemáme dostupná data o cenách potravin.
- V roce 2006 jsme si mohli za průměrnou mzdu 20678,- Kč koupit 641 kg chleba a 716 litrů mléka.
- V roce 2018 jsme si mohli za průměrnou mzdu 32486,- Kč koupit 670 kg chleba a 820 litrů mléka.

```
SELECT
	payroll_year,
	category_name,
	price_unit,
	round(avg(average_payroll) / 2 ,0) AS total_average_payroll,
	avg(average_price) AS total_average_price,
	round((avg(average_payroll) / 2) / avg(average_price),0) AS count_commodity_per_payroll
FROM t_lenka_stankova_project_sql_primary_final AS tlsp
WHERE average_payroll IS NOT NULL 
	AND average_price IS NOT NULL
	AND category_code IN ('114201','111301')
	AND payroll_year IN ('2006','2018')
GROUP BY payroll_year,
	category_name,
	price_unit,
	price_value
ORDER BY payroll_year
;
```

## 3. Která kategorie potravin zdražuje nejpomaleji (je u ní nejnižší percentuální meziroční nárůst)?

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
        AVG(tlsp.average_price) AS avg_price,
        LAG(AVG(tlsp.average_price)) OVER (
            PARTITION BY tlsp.category_code 
            ORDER BY tlsp.payroll_year
        ) AS previous_avg_price
    FROM t_lenka_stankova_project_sql_primary_final AS tlsp
    WHERE tlsp.category_name IS NOT NULL
    GROUP BY tlsp.category_code, tlsp.category_name, tlsp.payroll_year
)
SELECT 
    category_code,
    category_name,
    round(AVG(avg_price),2) AS overall_avg_price,
    round(AVG(avg_price - previous_avg_price),2) AS avg_price_change,
    CASE 
        WHEN AVG(avg_price) != 0 THEN 
            round(AVG(avg_price - previous_avg_price) / AVG(avg_price) * 100 , 2)
        ELSE NULL
    END AS relative_growth_percent
FROM price_diffs
GROUP BY category_code, category_name
ORDER BY relative_growth_percent ASC;
```

## 4. Existuje rok, ve kterém byl meziroční nárůst cen potravin výrazně vyšší než růst mezd (větší než 10 %)?

- Z dostupných dat vyplývá, že pro analýzu lze použít pouze období 2006 - 2018, kdy máme dostupná data o mzdách i cenách.  
- V žádném ze sledovaných období nebyl meziroční nárůst cen potravin výrazně vyšší než růst mezd. Nejvyšší meziroční zvýšení cen potravin bylo v roce 2017 o 9,63 %, ale tomtéž roce došlo také k nárůstu průměrných mezd o 6,31 %.

```
SELECT 
	payroll_year,
    ROUND((AVG(average_price) - 
           LAG(AVG(average_price)) OVER (ORDER BY payroll_year)) / 
           NULLIF(LAG(AVG(average_price)) OVER (ORDER BY payroll_year), 0) * 100, 2) AS price_percentage_change,
    ROUND((AVG(average_payroll) - 
           LAG(AVG(average_payroll)) OVER (ORDER BY payroll_year)) / 
           NULLIF(LAG(AVG(average_payroll)) OVER (ORDER BY payroll_year), 0) * 100, 2) AS payroll_percentage_change
FROM t_lenka_stankova_project_sql_primary_final
WHERE payroll_year BETWEEN 2006 AND 2018
GROUP BY payroll_year
ORDER BY price_percentage_change DESC;
```

## 5. Má výška HDP vliv na změny ve mzdách a cenách potravin? Neboli, pokud HDP vzroste výrazněji v jednom roce, projeví se to na cenách potravin či mzdách ve stejném nebo následujícím roce výraznějším růstem?

- Výška HDP nemá vliv na mzdy a ceny potravin. 


```
WITH gdp_data AS (
  SELECT
    e.YEAR,
    ROUND(AVG(e.gdp)::NUMERIC, 0) AS GDP,
    ROUND(AVG(cp.value), 0) AS overall_avg_payroll,
    ROUND(AVG(cp2.value)::NUMERIC ,2) AS overall_avg_price
  FROM economies AS e 
  JOIN czechia_payroll AS cp 
    ON e.YEAR = cp.payroll_year 
  JOIN czechia_price AS cp2 
    ON e.YEAR = EXTRACT(YEAR FROM cp2.date_from)
  WHERE e.country = 'Czech Republic' 
    AND e.YEAR > 1989
    AND cp.value_type_code = 5958
  GROUP BY e.YEAR
)
SELECT 
  YEAR,
  GDP,
  overall_avg_payroll,
  overall_avg_price,
  CASE 
    WHEN GDP > LAG(GDP) OVER (ORDER BY YEAR) THEN 'increase'
    WHEN GDP < LAG(GDP) OVER (ORDER BY YEAR) THEN 'decrease'
    ELSE 'no change'
  END AS gdp_trend,
    CASE 
    WHEN overall_avg_payroll > LAG(overall_avg_payroll) OVER (ORDER BY YEAR) THEN 'increase'
    WHEN overall_avg_payroll < LAG(overall_avg_payroll) OVER (ORDER BY YEAR) THEN 'decrease'
    ELSE 'no change'
  END AS payroll_trend,
      CASE 
    WHEN overall_avg_price > LAG(overall_avg_price) OVER (ORDER BY YEAR) THEN 'increase'
    WHEN overall_avg_price < LAG(overall_avg_price) OVER (ORDER BY YEAR) THEN 'decrease'
    ELSE 'no change'
  END AS price_trend
FROM gdp_data
ORDER BY YEAR DESC;
```
