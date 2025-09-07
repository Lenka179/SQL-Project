# SQL-Project

## 1. Popis projekut (Proč se to dělá?)

Cílem projektu je zjistit dostupnost základních potravin široké veřejnosti a zodpovědět několik konkrétních otázek týkajících se průměrných mezd a cen potravin v České Republice.  
Pro získání odpovědí byly vytvořeny dvě zdrojové tabulky (primární a seknudární), ze kterých budeme čerpat konečná data pro naše odpovědi. 

Výzkumné otázky:    
[1. Rostou v průběhu let mzdy ve všech odvětvích, nebo v některých klesají?](#1-rostou-v-pr%C5%AFb%C4%9Bhu-let-mzdy-ve-v%C5%A1ech-odv%C4%9Btv%C3%ADch-nebo-v-n%C4%9Bkter%C3%BDch-klesaj%C3%AD)  

[1. Rostou v průběhu let mzdy ve všech odvětvích, nebo v některých klesají?](#1-rostou-v-prubehu-let-mzdy-ve-vsech-odvetvich-nebo-v-nekterych-klesaji)

## Obsah

- [1. Rostou v průběhu let mzdy ve všech odvětvích, nebo v některých klesají?](#1-rostou-v-prubehu-let-mzdy-ve-vsech-odvetvich-nebo-v-nekterych-klesaji)
- [2. Jak se liší vývoj mezd mezi regiony?](#2-jak-se-lisi-vyvoj-mezd-mezi-regiony)
- [3. Které profese zaznamenaly největší růst?](#3-ktere-profese-zaznamenaly-nejvetsi-rust)




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
FROM t_lenka_stankova_project_sql_primary_final;
```

### Sekundární tabulka

TODO

## 2. Odpovědi na otázky

## 1. Rostou v průběhu let mzdy ve všech odvětvích, nebo v některých klesají?  

## 1. Rostou v průběhu let mzdy ve všech odvětvích, nebo v některých klesají?
## 2. Jak se liší vývoj mezd mezi regiony?
## 3. Které profese zaznamenaly největší růst?

- Ano, obecně lze říci, že průměrné platy ve všech odvětvích **rostou**.  
- Za výjimku bychom mohli považovat rok 2013, kdy v 11 z 19 sledovaných odvětví mzdy klesly. Největší meziroční propad průměrné mzdy byl právě v roce 2013 a zasáhl oblast Peněžnicvtí a pojišťovnictví. Příčinou by mohla být ekonomická recese ČR, která souvisela s dluhovou krizí v eurozóně a vládními úspornými opatřeními.
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

**2. Kolik je možné si koupit litrů mléka a kilogramů chleba za první a poslední srovnatelné období v dostupných datech cen a mezd?**  
> - Do srovnatelného období můžeme zahrnout pouze odbdobí 2006 až 2018. Mimo toto období máme k dispozici data o průměrných mzdách, ale nemáme dostupná data o cenách potravin.
> - V roce 2006 jsme si mohli za průměrnou mzdu 20677,- Kč koupit 1283 kg chleba nebo 1432 litrů mléka.
> - V roce 2018 jsme si mohli za průměrnou mzdu 32485,- Kč koupit 1340 kg chleba nebo 1639 litrů mléka.

```
SELECT
	payroll_year,
	category_name,
	price_unit,
	round(avg(average_payroll)::NUMERIC,0) AS total_average_payroll,
	avg(average_price) AS total_average_price,
	round(avg(average_payroll) / avg(average_price)::NUMERIC,0) AS count_commodity_per_payroll
FROM t_lenka_stankova_project_sql_primary_final AS tlsp
WHERE average_payroll IS NOT NULL 
	AND average_price IS NOT NULL
	AND category_code IN ('114201','111301')
	AND payroll_year IN ('2006','2018')
GROUP BY payroll_year,
	category_name,
	price_unit,
	price_value
ORDER BY payroll_year;
```




3. Která kategorie potravin zdražuje nejpomaleji (je u ní nejnižší percentuální meziroční nárůst)?
4. Existuje rok, ve kterém byl meziroční nárůst cen potravin výrazně vyšší než růst mezd (větší než 10 %)?
5. Má výška HDP vliv na změny ve mzdách a cenách potravin? Neboli, pokud HDP vzroste výrazněji v jednom roce, projeví se to na cenách potravin či mzdách ve stejném nebo následujícím roce výraznějším růstem?
