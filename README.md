#### Query em Sql Server para o sistema Protheus 12.

* A Query serve para caso o seu sistema tenha problemas em atualizar a base de atendimento se o tipo de retono D1_TIPO for ihual a B ou beneficiamento, a Query auxilia em mostrar qual são os dados exatos que devem estár na tabela de base de atendimento
****
```SQL
/*
	@Author	: Emerson Pereira
	@Data	: 27/01/2022
	@Sys	: Protheus 12
	@Descri	: Retorna a base de atendimento correta com base nas NFs de saida e Entrada
*/

SELECT TOP 1 /*Top 1 para retonrar apenas um resultado exato*/
CASE
	WHEN D2_EMISSAO = D1_EMISSAO THEN 'A DATA DE SAIDA E ENTRADA SÃO IGUAIS(ANALISAR)'
	WHEN D2_EMISSAO > D1_EMISSAO THEN 'O ITEM ESTA FORA'
	WHEN D1_EMISSAO > D2_EMISSAO THEN 'O ITEM ESTA NA JH'
END AS 'STATUS',
AA3_CODPRO AS 'PRODUTO',
CASE
	WHEN D2_EMISSAO > D1_EMISSAO THEN D2_CLIENTE
	WHEN D2_EMISSAO < D1_EMISSAO THEN '%Codigo da Empesa'
END AS 'CLIENTE_CORRETO',

CASE
	WHEN D2_EMISSAO > D1_EMISSAO THEN D2_LOJA
	WHEN D2_EMISSAO < D1_EMISSAO THEN '%Loja da Empresa'
END AS 'LOJA_CORRETA',

CASE
	WHEN D2_EMISSAO > D1_EMISSAO THEN ABS_LOCAL
	WHEN D2_EMISSAO < D1_EMISSAO THEN '%Codigo do Local da Empresa'
END AS 'LOCAL_CORRETO',

CASE
	WHEN AA3_MSBLQL = '1' THEN 'BASE INATIVA'
	WHEN AA3_MSBLQL = '2' THEN 'BASE ATIVA'
END AS 'STATUS_BASE',

CASE
	WHEN D2_EMISSAO > D1_EMISSAO THEN D2_DOC+'  |  '+ 'NF DE SAIDA'
	WHEN D2_EMISSAO < D1_EMISSAO THEN D1_DOC+'  |  '+ 'NF DE ENTRADA'
END AS 'NOTA_FISCAL',

CASE
	WHEN D2_EMISSAO > D1_EMISSAO THEN D2_TIPO

	WHEN D2_EMISSAO < D1_EMISSAO AND D1_TIPO = 'B' THEN 'BENEFICIAMENTO'
	WHEN D2_EMISSAO < D1_EMISSAO AND D1_TIPO = 'N' THEN 'NORMAL' 
	WHEN D2_EMISSAO < D1_EMISSAO AND D1_TIPO = 'C' THEN 'NORMAL' 
	WHEN D2_EMISSAO < D1_EMISSAO AND D1_TIPO = 'I' THEN 'COMPLE. ICMS' 
	WHEN D2_EMISSAO < D1_EMISSAO AND D1_TIPO = 'D' THEN 'DEVOLUÇÃO' 
	WHEN D2_EMISSAO < D1_EMISSAO AND D1_TIPO = 'P' THEN 'COMPLE. IPI' 
END AS 'TIPO_DE_RETORNO'

FROM AA3010 --Tabela Base de Atendimento
LEFT JOIN SD1010 D1 (NOLOCK) ON D1_COD = AA3_CODPRO -- Tabela NF de Entrda
LEFT JOIN SD2010 D2 (NOLOCK) ON D2_COD = AA3_CODPRO	-- Tabela Nf de Saida
LEFT JOIN ABS010 BS (NOLOCK) ON ABS_CODIGO+ABS_LOJA = D2_CLIENTE+D2_LOJA -- Tabela Local de Atendimento
WHERE AA3_CODPRO = 'PRODUTO'

--Aqui  é Ordernado pela ultima dtada de emissão das Nfs de saida e entrada	
ORDER BY D1_EMISSAO DESC , D2_EMISSAO DESC
```

****
### Contatos
[![Linkedin](https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/emerson-santos-9358041b7/)
[![Whatsapp](https://img.shields.io/badge/WhatsApp-25D366?style=for-the-badge&logo=whatsapp&logoColor=white)](https://api.whatsapp.com/send?phone=5511999467109&text=Ol%C3%A1%20Sou%20o%20Emerson%20Pereira)
[![Gmail](https://img.shields.io/badge/Gmail-D14836?style=for-the-badge&logo=gmail&logoColor=white)](mailto:emersonrox8@gmail.com)