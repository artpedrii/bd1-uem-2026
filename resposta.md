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

```sql

```

### 2.b

```sql

```

### 2.c

**Resposta:**

## Questão 3 — Agregações

### 3.a

```sql

```

### 3.b

```sql

```

### 3.c

```sql

```

## Questão 4 — Relacionamentos e agregações

```sql

```

