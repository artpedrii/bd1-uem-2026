# Atividade 4 — Funções de Agregação, GROUP BY e HAVING (Resolução)

Usa `categoria` e `produto` da Atividade 1, mais as categorias vazias **Brinquedos** e **Jardinagem** criadas na Atividade 3.

## Dados de referência (Atividade 1)

| Categoria | Produtos (preço × estoque) |
|---|---|
| Informática | Notebook 3500×5, Mouse sem fio 89,90×0, Teclado 150×10, Hub USB 250×5, Webcam 300×2, Monitor 1200×10 |
| Escritório | Cadeira de escritório 650×8, Grampeador 25,90×40 |
| Acessórios | Fone de ouvido 120×15, Headset 450×5 |
| Eletrônicos | Smart TV 2200×6 |
| Games | Controle de videogame 350×12 |
| Móveis | Mesa de escritório 800×4 |
| Papelaria | Caderno universitário 18,50×100 |
| Livros | Livro de SQL 95×20 |
| Brinquedos, Jardinagem | (sem produtos) |

Os resultados esperados abaixo foram calculados com esses dados.

---

# Nível 1 — Funções básicas

## 1. Contar todos os produtos
```sql
SELECT COUNT(*) AS total_produtos
FROM produto;
```
Esperado: **15**.

## 2. Contar as categorias distintas
```sql
SELECT COUNT(DISTINCT id_categoria) AS categorias_com_produtos
FROM produto;
```
Esperado: **8**. Conta as categorias que têm produtos; as vazias não entram, pois só olhamos `produto`.

## 3. Total de unidades em estoque
```sql
SELECT SUM(estoque) AS unidades_em_estoque
FROM produto;
```
Esperado: **242**.

## 4. Valor financeiro total do estoque
```sql
SELECT SUM(preco * estoque) AS valor_total_estoque
FROM produto;
```
Esperado: **67.486,00**. Multiplicamos linha a linha (preço × estoque) antes de somar.

## 5. Preço médio
```sql
SELECT ROUND(AVG(preco), 2) AS preco_medio
FROM produto;
```
Esperado: **679,95**.

## 6. Menor e maior preço
```sql
SELECT MIN(preco) AS menor_preco,
       MAX(preco) AS maior_preco
FROM produto;
```
Esperado: **18,50** e **3500,00**.

## 7. Todos os produtos empatados no menor preço
```sql
SELECT *
FROM produto
WHERE preco = (SELECT MIN(preco) FROM produto);
```
Usar `ORDER BY preco LIMIT 1` retornaria só **uma** linha e ignoraria empates. A subconsulta traz o menor preço, e o `WHERE` devolve todos os produtos com esse valor. Neste conjunto de dados: Caderno universitário (18,50).

---

# Nível 2 — GROUP BY

## 8. Produtos por categoria
```sql
SELECT id_categoria, COUNT(*) AS qtd_produtos
FROM produto
GROUP BY id_categoria
ORDER BY id_categoria;
```

## 9. Estoque total por categoria
```sql
SELECT id_categoria, SUM(estoque) AS estoque_total
FROM produto
GROUP BY id_categoria;
```

## 10. Preço médio, mínimo e máximo por categoria
```sql
SELECT id_categoria,
       ROUND(AVG(preco), 2) AS preco_medio,
       MIN(preco)           AS preco_minimo,
       MAX(preco)           AS preco_maximo
FROM produto
GROUP BY id_categoria;
```

## 11. Valor financeiro do estoque por categoria
```sql
SELECT id_categoria,
       SUM(preco * estoque) AS valor_estoque
FROM produto
GROUP BY id_categoria;
```

## 12. Produtos de cada categoria em uma string, em ordem alfabética
```sql
SELECT id_categoria,
       STRING_AGG(nome, ', ' ORDER BY nome) AS produtos
FROM produto
GROUP BY id_categoria;
```
`STRING_AGG(coluna, separador ORDER BY ...)` concatena os valores do grupo em um texto.

---

# Nível 3 — WHERE, HAVING e FILTER

> **`WHERE`** filtra **linhas antes** de agrupar. **`HAVING`** filtra **grupos depois** de agregar.

## 13. Contar por categoria somente produtos com preço acima de R$ 100
```sql
SELECT id_categoria, COUNT(*) AS qtd_produtos
FROM produto
WHERE preco > 100
GROUP BY id_categoria;
```

## 14. Categorias com pelo menos três produtos
```sql
SELECT id_categoria, COUNT(*) AS qtd_produtos
FROM produto
GROUP BY id_categoria
HAVING COUNT(*) >= 3;
```
Esperado: apenas **Informática** (6 produtos).

## 15. Categorias com preço médio acima de R$ 500
```sql
SELECT id_categoria, ROUND(AVG(preco), 2) AS preco_medio
FROM produto
GROUP BY id_categoria
HAVING AVG(preco) > 500;
```
Esperado: **Informática** (≈ 914,98), **Eletrônicos** (2200,00) e **Móveis** (800,00).

## 16. Em uma linha: total, preço acima de R$ 1000 e estoque abaixo de 10
```sql
SELECT COUNT(*)                                AS total,
       COUNT(*) FILTER (WHERE preco > 1000)    AS caros,
       COUNT(*) FILTER (WHERE estoque < 10)    AS estoque_baixo
FROM produto;
```
Esperado: **15**, **3** (Notebook, Monitor, Smart TV) e **8**.

## 17. Categorias com valor total de estoque acima de R$ 10.000
```sql
SELECT id_categoria,
       SUM(preco * estoque) AS valor_estoque
FROM produto
GROUP BY id_categoria
HAVING SUM(preco * estoque) > 10000;
```
Esperado: **Informática** (32.850,00) e **Eletrônicos** (13.200,00).

---

# Nível 4 — JOIN e agregação

## 18. Produtos por categoria, incluindo categorias vazias
```sql
SELECT c.id_categoria,
       c.nome AS categoria,
       COUNT(p.id_produto) AS qtd_produtos
FROM categoria c
LEFT JOIN produto p
       ON p.id_categoria = c.id_categoria
GROUP BY c.id_categoria, c.nome
ORDER BY c.nome;
```
Brinquedos e Jardinagem aparecem com **0**. Para isso usamos `COUNT(p.id_produto)`, e não `COUNT(*)` (ver exercício 21).

## 19. Somente categorias vazias
```sql
SELECT c.id_categoria, c.nome AS categoria
FROM categoria c
LEFT JOIN produto p
       ON p.id_categoria = c.id_categoria
GROUP BY c.id_categoria, c.nome
HAVING COUNT(p.id_produto) = 0;
```
Alternativa sem agregação:
```sql
SELECT c.id_categoria, c.nome
FROM categoria c
LEFT JOIN produto p ON p.id_categoria = c.id_categoria
WHERE p.id_produto IS NULL;
```

## 20. Quantidade, média e valor do estoque por categoria
```sql
SELECT c.nome AS categoria,
       COUNT(p.id_produto)                  AS qtd_produtos,
       ROUND(AVG(p.preco), 2)               AS preco_medio,
       COALESCE(SUM(p.preco * p.estoque), 0) AS valor_estoque
FROM categoria c
LEFT JOIN produto p
       ON p.id_categoria = c.id_categoria
GROUP BY c.id_categoria, c.nome
ORDER BY c.nome;
```
Em categorias vazias, `AVG` retorna `NULL` e `SUM` retorna `NULL`. Por isso usamos `COALESCE(..., 0)` no valor do estoque.

## 21. Por que `COUNT(*)` pode dar resultado incorreto em um `LEFT JOIN`

`COUNT(*)` conta **linhas**, e `COUNT(coluna)` conta **valores não nulos**. No `LEFT JOIN`, uma categoria sem produtos continua gerando **uma linha** (com as colunas de `produto` em `NULL`).

```sql
SELECT c.nome, COUNT(*) AS errado
FROM categoria c
LEFT JOIN produto p ON p.id_categoria = c.id_categoria
GROUP BY c.nome;
```
Aqui "Brinquedos" aparece com **1**, embora não tenha nenhum produto. Com `COUNT(p.id_produto)`, o `NULL` é ignorado e o resultado correto é **0**.

---

# Desafio

## 22. Modelagem de `cliente`, `pedido` e `item_pedido`

Para não sobrescrever as tabelas da Atividade 3, usamos o sufixo `_v2`. O `item_pedido_v2` referencia a tabela `produto` já existente.

```sql
CREATE TABLE cliente_v2 (
    id_cliente   BIGINT GENERATED BY DEFAULT AS IDENTITY PRIMARY KEY,
    nome_cliente VARCHAR(100) NOT NULL
);

CREATE TABLE pedido_v2 (
    id_pedido   BIGINT GENERATED BY DEFAULT AS IDENTITY PRIMARY KEY,
    data_pedido DATE NOT NULL,
    id_cliente  BIGINT NOT NULL REFERENCES cliente_v2 (id_cliente)
);

CREATE TABLE item_pedido_v2 (
    id_pedido      BIGINT  NOT NULL REFERENCES pedido_v2 (id_pedido),
    id_produto     BIGINT  NOT NULL REFERENCES produto (id_produto),
    quantidade     INTEGER NOT NULL CHECK (quantidade > 0),
    preco_unitario NUMERIC(10,2) NOT NULL CHECK (preco_unitario >= 0),
    PRIMARY KEY (id_pedido, id_produto)
);
```

### Dados de exemplo

```sql
INSERT INTO cliente_v2 (nome_cliente)
VALUES ('Ana'), ('Bruno'), ('Carla'), ('Daniel');   -- Carla não terá pedidos

INSERT INTO pedido_v2 (data_pedido, id_cliente)
VALUES
    ('2026-11-01', 1),   -- pedido 1: Ana
    ('2026-11-08', 1),   -- pedido 2: Ana
    ('2026-11-05', 2),   -- pedido 3: Bruno
    ('2026-11-10', 4);   -- pedido 4: Daniel
```

Como os `id_produto` podem ter "buracos" (falhas na Atividade 1 consomem a sequência), os itens buscam o id pelo nome:

```sql
INSERT INTO item_pedido_v2 (id_pedido, id_produto, quantidade, preco_unitario)
VALUES
 (1, (SELECT id_produto FROM produto WHERE nome = 'Notebook'),              1, 3500.00),
 (1, (SELECT id_produto FROM produto WHERE nome = 'Mouse sem fio'),         2,   89.90),
 (2, (SELECT id_produto FROM produto WHERE nome = 'Livro de SQL'),          1,   95.00),
 (2, (SELECT id_produto FROM produto WHERE nome = 'Caderno universitário'), 3,   18.50),
 (3, (SELECT id_produto FROM produto WHERE nome = 'Smart TV'),              1, 2200.00),
 (3, (SELECT id_produto FROM produto WHERE nome = 'Mouse sem fio'),         1,   89.90),
 (4, (SELECT id_produto FROM produto WHERE nome = 'Mouse sem fio'),         4,   89.90),
 (4, (SELECT id_produto FROM produto WHERE nome = 'Grampeador'),            2,   25.90);
```

Totais dos pedidos: 1 = 3.679,80 | 2 = 150,50 | 3 = 2.289,90 | 4 = 411,40.

### a) Total de pedidos por cliente, incluindo clientes sem pedidos
```sql
SELECT c.nome_cliente,
       COUNT(p.id_pedido) AS total_pedidos
FROM cliente_v2 c
LEFT JOIN pedido_v2 p ON p.id_cliente = c.id_cliente
GROUP BY c.id_cliente, c.nome_cliente
ORDER BY c.nome_cliente;
```
Esperado: Ana 2 | Bruno 1 | Carla 0 | Daniel 1.

### b) Valor total de cada pedido
```sql
SELECT i.id_pedido,
       SUM(i.quantidade * i.preco_unitario) AS valor_pedido
FROM item_pedido_v2 i
GROUP BY i.id_pedido
ORDER BY i.id_pedido;
```

### c) Total gasto por cliente
Primeiro agrega os itens por pedido (CTE) e depois agrupa por cliente:
```sql
WITH pedido_total AS (
    SELECT id_pedido,
           SUM(quantidade * preco_unitario) AS valor_pedido
    FROM item_pedido_v2
    GROUP BY id_pedido
)
SELECT c.nome_cliente,
       COALESCE(SUM(pt.valor_pedido), 0) AS total_gasto
FROM cliente_v2 c
LEFT JOIN pedido_v2 p        ON p.id_cliente = c.id_cliente
LEFT JOIN pedido_total pt    ON pt.id_pedido = p.id_pedido
GROUP BY c.id_cliente, c.nome_cliente
ORDER BY c.nome_cliente;
```
Esperado: Ana 3.830,30 | Bruno 2.289,90 | Carla 0,00 | Daniel 411,40.

### d) Ticket médio por cliente
Ticket médio = **média do valor dos pedidos** do cliente.
```sql
WITH pedido_total AS (
    SELECT id_pedido,
           SUM(quantidade * preco_unitario) AS valor_pedido
    FROM item_pedido_v2
    GROUP BY id_pedido
)
SELECT c.nome_cliente,
       ROUND(AVG(pt.valor_pedido), 2) AS ticket_medio
FROM cliente_v2 c
LEFT JOIN pedido_v2 p        ON p.id_cliente = c.id_cliente
LEFT JOIN pedido_total pt    ON pt.id_pedido = p.id_pedido
GROUP BY c.id_cliente, c.nome_cliente
ORDER BY c.nome_cliente;
```
Esperado: Ana 1.915,15 | Bruno 2.289,90 | Carla `NULL` (sem pedidos) | Daniel 411,40.

> Calcular a média direto sobre `item_pedido_v2` daria a média dos **itens**, e não dos pedidos. Por isso agregamos por pedido antes.

### e) Clientes com total gasto acima de R$ 1.000
```sql
WITH pedido_total AS (
    SELECT id_pedido,
           SUM(quantidade * preco_unitario) AS valor_pedido
    FROM item_pedido_v2
    GROUP BY id_pedido
)
SELECT c.nome_cliente,
       SUM(pt.valor_pedido) AS total_gasto
FROM cliente_v2 c
JOIN pedido_v2 p        ON p.id_cliente = c.id_cliente
JOIN pedido_total pt    ON pt.id_pedido = p.id_pedido
GROUP BY c.id_cliente, c.nome_cliente
HAVING SUM(pt.valor_pedido) > 1000;
```
Esperado: **Ana** (3.830,30) e **Bruno** (2.289,90).

### f) Produto mais vendido por quantidade, preservando empates
```sql
WITH vendas AS (
    SELECT pr.id_produto,
           pr.nome,
           SUM(i.quantidade) AS qtd_vendida
    FROM item_pedido_v2 i
    JOIN produto pr ON pr.id_produto = i.id_produto
    GROUP BY pr.id_produto, pr.nome
)
SELECT nome, qtd_vendida
FROM vendas
WHERE qtd_vendida = (SELECT MAX(qtd_vendida) FROM vendas);
```
Esperado: **Mouse sem fio** (7 unidades). Como o filtro compara com o `MAX`, se dois produtos empatarem **ambos** são retornados (um `ORDER BY ... LIMIT 1` perderia o empate).
