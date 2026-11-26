# Respostas — Avaliação Prática II de Banco de Dados I

**Acadêmico(a): Arthur Pedrinho de Paula**  
**RA: 145100**  
**Data: 26/11/2026**

## Orientações

Preencha os blocos SQL com as consultas finais, testadas no Supabase. Utilize os nomes completos das tabelas (`avaliacao_bd2.produto` e `avaliacao_bd2.categoria`). Mantenha os títulos e a numeração dos itens. Não inclua o script de preparação, resultados das consultas ou capturas de tela. No item 2.c, escreva sua explicação em texto.

## Questão 1 — Consultas SQL

### 1.a

```
SELECT nome, preco, estoque 
FROM avaliacao_bd2.produto 
WHERE preco > 100.0 AND estoque >= 5
ORDER BY preco DESC 

```

### 1.b

```
SELECT *
FROM avaliacao_bd2.produto
WHERE preco BETWEEN 50.0 AND 1000.0
ORDER BY nome 

```

## Questão 2 — Relacionamentos

### 2.a

```
SELECT p.nome AS avaliacao_bd2.produto, p.preco, c.nome AS avaliacao_bd2.categoria
FROM avaliacao_bd2.produto p
INNER JOIN categoria c ON c.id_categoria = p.id_categoria 
```

### 2.b

```
SELECT c.nome AS categoria, p.nome AS produto
FROM avaliacao_bd2.categoria c 
LEFT JOIN avaliacao_bd2.produto p ON p.id_categoria = c.id_categoria 
```

### 2.c

**Resposta: Porque na questão a) o INNER JOIN mostra todas as tabelas que possuem correspondência, limpeza não possui produto correspondente, logo fica de fora. 
O LEFT JOIN mostra a tabela da esquerda (neste caso categoria) e suas correspondências (independente se existem ou não), Limpeza está em categoria e, mesmo sem correspondente ela aparece.
O LEFT JOIN não exige que HAJA correspondência, apenas exibe o que ta na esquerda e seus correspondentes (NULL ou não).**

## Questão 3 — Agregações

### 3.a

```
SELECT COUNT (*) AS quantidade_produtos_cadastrados, SUM(estoque) AS quantidade_total_estoque, AVG(preco) AS media, MIN(preco) AS menor_valor, MAX(preco) AS maior_valor
FROM avaliacao_bd2.produto
```

### 3.b

```
SELECT sum(preco * estoque) AS valor_financeiro_estoque
FROM avaliacao_bd2.produto
```

### 3.c

```
SELECT produto.id_categoria, estoque AS quantidade 
FROM avaliacao_bd2.produto
WHERE estoque > 2
```

## Questão 4 — Relacionamentos e agregações

```sql

```

