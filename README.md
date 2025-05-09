# SQLAdventureWorksDW2022
Neste repositório iremos analisar o banco de dados AdventureWorksDW2022, utilizando SQL.

Vamos realizar um estudo no banco de dados para entendermos o que os dados nos mostram.

USE AdventureWorksDW2022;

--VAMOS COMEÇAR VENDO O TOTAL DAS VENDAS DOS REVENDEDORES, JUNTAMENTE COM O CUSTO TOTAL, E SUBTRAINDO AS VENDAS DOS CUSTOS ENCONTRAMOS O LUCRO
SELECT FORMAT(SUM(ExtendedAmount),'C0') AS VALOR_TOTAL_DE_VENDAS
	,FORMAT(SUM(TotalProductCost),'C0') AS VALOR_TOTAL_DOS_CUSTOS
	,FORMAT(SUM(ExtendedAmount) - SUM(TotalProductCost),'C0') AS LUCRO_DAS_VENDAS
FROM FactResellerSales

--AGORA VAMOS VERIFICAR O VALOR TOTAL DAS VENDAS POR PAÍS
SELECT G.EnglishCountryRegionName
	,FORMAT(SUM(RS.ExtendedAmount),'C0') AS VALOR_TOTAL_DE_VENDAS
FROM FactResellerSales RS
INNER JOIN DimReseller R ON R.ResellerKey = RS.ResellerKey
INNER JOIN DimGeography G ON G.GeographyKey = R.ResellerKey
GROUP BY G.EnglishCountryRegionName

--VAMOS VERIFICAR QUAIS SÃO OS MAIORES REVENDEDORES
SELECT R.ResellerName AS NOME_REVENDEDORES
	,SUM(RS.ExtendedAmount) AS VALOR_TOTAL_DE_VENDAS
FROM FactResellerSales RS
INNER JOIN DimReseller R ON R.ResellerKey = RS.ResellerKey
INNER JOIN DimGeography G ON G.GeographyKey = R.ResellerKey
GROUP BY R.ResellerName
ORDER BY 2 DESC

--VAMOS VERIFICAR QUAIS OS PRODUTOS MAIS VENDIDOS
SELECT P.EnglishProductName
	,SUM(RS.ExtendedAmount) AS VALOR_TOTAL_DE_VENDAS
FROM FactResellerSales RS
INNER JOIN DimProduct P ON P.ProductKey = RS.ProductKey
GROUP BY P.EnglishProductName
ORDER BY 2 DESC


-- VAMOS VERIFICAR QUAL A CATEGORIA DE PRODUTO QUE TEM MAIS VALOR
SELECT PC.EnglishProductCategoryName
	,SUM(RS.ExtendedAmount) AS VALOR_TOTAL_DE_VENDAS
FROM FactResellerSales RS
INNER JOIN DimProduct P ON P.ProductKey = RS.ProductKey
INNER JOIN DimProductSubcategory PS ON PS.ProductSubcategoryKey = P.ProductSubcategoryKey
INNER JOIN DimProductCategory PC ON PC.ProductCategoryKey = PS.ProductCategoryKey
GROUP BY PC.EnglishProductCategoryName
ORDER BY 2 DESC
