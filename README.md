A) Elabore uma consulta que retorne os códigos de barras dos produtos cadastrados pela primeira vez no banco nos últimos 10 dias.

SELECT "Código de barras"

FROM Produto

WHERE "Data cadastro" >= SYSDATE - 10;


B) Elabore uma consulta que retorne os nomes das origens que tiveram algum produto cadastrado ou atualizado no mês de março de 2020.

SELECT DISTINCT Origem.Nome

FROM Produtoinfo

JOIN Origem ON Produtoinfo.IDOrigem = Origem.ID

WHERE TO_CHAR("Data cadastro", 'YYYY-MM') = '2020-03' OR TO_CHAR("Data Atualização", 'YYYY-MM') = '2020-03';



C) Houve um erro na atualização da origem "DISTRIBUIDORA TESTE" em que todos os produtos do fabricante "JOAO" foram enviados com nome "MARIA". Faça um update que troque os produtos da "DISTRIBUIDORA TESTE" com fabricante "MARIA" para o fabricante "JOAO".

UPDATE Produtoinfo

SET IDFabricante = (SELECT ID FROM Fabricante WHERE Nome = 'JOAO')

WHERE IDOrigem = (SELECT ID FROM Origem WHERE Nome = 'DISTRIBUIDORA TESTE')

AND IDFabricante = (SELECT ID FROM Fabricante WHERE Nome = 'MARIA');


d) Elabore uma consulta que retorne os códigos de barras, descrição, nome do fabricante e código interno de todos os produtos cadastrados de acordo com a ordem de preferência, onde um código de barras aparecerá apenas uma vez no resultado com as informações da origem de maior preferência disponível para ele.

WITH PreferenciaOrigem AS (

  SELECT IDProduto, IDOrigem, Preferencia,
  
         ROW_NUMBER() OVER (PARTITION BY IDProduto ORDER BY Preferencia ASC) AS preferencia_rank
         
  FROM Origem
  
)
SELECT P."Código de barras", PI.Descrição, F.Nome AS "Nome do Fabricante", PI."Código Interno"

FROM Produto P

JOIN Produtoinfo PI ON P.ID = PI.IDProduto

JOIN Fabricante F ON PI.IDFabricante = F.ID

JOIN PreferenciaOrigem PO ON PI.IDProduto = PO.IDProduto

WHERE PO.preferencia_rank = 1;


