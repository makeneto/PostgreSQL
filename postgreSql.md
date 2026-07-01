# SQL JOINs (PostgreSQL)

Os `JOINs` permitem **combinar dados de duas ou mais tabelas** através de um relacionamento entre elas.

Imagine as seguintes tabelas:

### `users`

| id | username |
|----|----------|
| 1 | Makene |
| 2 | Ana |
| 3 | João |

### `posts`

| id | title | user_id |
|----|-------|---------|
| 1 | SQL | 1 |
| 2 | React | 1 |
| 3 | Docker | 2 |
| 4 | Linux | 4 |

> **Observação:** O post `Linux` possui `user_id = 4`, mas esse usuário não existe na tabela `users`.

---

# A cláusula `ON`

Antes de aprender os tipos de `JOIN`, entenda o papel do `ON`.

```sql
SELECT *
FROM users
JOIN posts
ON users.id = posts.user_id;
```

O `ON` define **como as tabelas devem ser relacionadas**.

Neste caso:

```sql
users.id = posts.user_id
```

Significa:

> Junte um usuário ao seu respectivo post quando o `id` do usuário for igual ao `user_id` do post.

Sem o `ON`, o banco de dados não sabe quais registros devem ser combinados.

---

# 1. INNER JOIN

Retorna **apenas os registros que possuem correspondência nas duas tabelas**.

```sql
SELECT *
FROM users
INNER JOIN posts
ON users.id = posts.user_id;
```

Resultado:

| username | title |
|----------|-------|
| Makene | SQL |
| Makene | React |
| Ana | Docker |

**Não aparecem:**

- João (não possui posts)
- Linux (não possui usuário)

### Quando usar?

Sempre que você quiser apenas registros relacionados.

Exemplos:

- Usuários e seus posts
- Clientes e seus pedidos
- Produtos e suas categorias

---

# 2. LEFT JOIN

Retorna **todos os registros da tabela da esquerda** (`FROM`).

Se não existir correspondência, os campos da outra tabela serão `NULL`.

```sql
SELECT *
FROM users
LEFT JOIN posts
ON users.id = posts.user_id;
```

Resultado:

| username | title |
|----------|-------|
| Makene | SQL |
| Makene | React |
| Ana | Docker |
| João | NULL |

### Quando usar?

Quando a tabela da esquerda é a principal e você não quer perder nenhum registro.

Exemplos:

- Todos os usuários, mesmo sem posts.
- Todos os clientes, mesmo sem pedidos.

---

# 3. RIGHT JOIN

Retorna **todos os registros da tabela da direita** (`JOIN`).

```sql
SELECT *
FROM users
RIGHT JOIN posts
ON users.id = posts.user_id;
```

Resultado:

| username | title |
|----------|-------|
| Makene | SQL |
| Makene | React |
| Ana | Docker |
| NULL | Linux |

### Quando usar?

Na prática, raramente.

Normalmente basta inverter a ordem das tabelas e utilizar um `LEFT JOIN`.

---

# 4. FULL JOIN

Retorna **todos os registros de ambas as tabelas**.

Quando não existir correspondência, o banco preencherá os campos com `NULL`.

```sql
SELECT *
FROM users
FULL JOIN posts
ON users.id = posts.user_id;
```

Resultado:

| username | title |
|----------|-------|
| Makene | SQL |
| Makene | React |
| Ana | Docker |
| João | NULL |
| NULL | Linux |

### Quando usar?

Ideal para:

- Auditorias
- Comparação entre tabelas
- Sincronização de dados
- Encontrar registros sem correspondência

---

# A ordem das tabelas importa?

**Sim.**

A tabela escrita após o `FROM` é a **tabela principal**.

## Exemplo 1

```sql
FROM users
LEFT JOIN posts
```

Leitura:

> Quero **todos os usuários** e, se existirem, seus posts.

---

## Exemplo 2

```sql
FROM posts
LEFT JOIN users
```

Leitura:

> Quero **todos os posts** e, se existirem, seus respectivos usuários.

Perceba que apenas trocar a ordem muda completamente o resultado.

---

# A ordem importa para todos os JOINs?

| JOIN | A ordem altera o resultado? |
|------|------------------------------|
| INNER JOIN | ❌ Não |
| LEFT JOIN | ✅ Sim |
| RIGHT JOIN | ✅ Sim |
| FULL JOIN | ❌ Não (apenas a ordem das colunas) |

---

# Como decorar

Sempre pense assim:

```sql
FROM tabela_principal
LEFT JOIN outra_tabela
```

Leitura:

> Quero **todos os registros da tabela principal**.

A tabela principal é sempre aquela escrita depois do `FROM`.

---

# Resumo

| JOIN | Retorna | Uso mais comum |
|------|----------|----------------|
| `INNER JOIN` | Apenas registros relacionados | Buscar dados relacionados |
| `LEFT JOIN` | Todos da tabela da esquerda | Manter todos os registros da tabela principal |
| `RIGHT JOIN` | Todos da tabela da direita | Pouco utilizado |
| `FULL JOIN` | Todos os registros das duas tabelas | Comparações e auditorias |

---

# Regra de ouro

Antes de escrever um `JOIN`, faça duas perguntas:

1. **Qual é a minha tabela principal?**
2. **Quero manter todos os registros dela ou apenas os que possuem relacionamento?**

Se responder essas duas perguntas, ficará fácil escolher o `JOIN` correto.