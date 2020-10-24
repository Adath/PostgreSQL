<p align="center"><img height=300 width=300 src="Postgresql_elephant.svg"></p>

<h1 align="center">PostgreSQL</h1>

<p align="center">Repository containing the queries developed in <a href="https://www.postgresql.org">PostgreSQL</a></p>

<h3 align="center">References</h3>

<span>👹</span> <a href="https://www.w3schools.com">https://www.w3schools.com</a>

<span>👹</span> <a href="https://www.postgresqltutorial.com">https://www.postgresqltutorial.com</a>

<span>👹</span> <a href="https://www.edivaldobrito.com.br/pgadmin4-no-ubuntu">https://www.edivaldobrito.com.br/pgadmin4-no-ubuntu</a>

<span>👹</span> <a href="https://www.edivaldobrito.com.br/postgresql-no-debian">https://www.edivaldobrito.com.br/postgresql-no-debian</a>

<span>👹</span> <a href="https://www.tutorialspoint.com/postgresql/postgresql_c_cpp.htm">https://www.tutorialspoint.com/postgresql/postgresql_c_cpp.htm</a>

<span>🧷</span> <span>`1. DDL (DATA DEFINITION LANGUAGE);`</span>

<span>🧷</span> <span>`2. DML (DATA MANIPULATION LANGUAGE);`</span>

<span>🧷</span> <span>`3. DCL (DATA CONTROL LANGUAGE);`</span>

<span>🧷</span> <span>`4. TCL (TRANSACTION CONTROL LANGUAGE);`</span>

<span>🧷</span> <span>`5. DQL (DATA QUERY LANGUAGE);`</span>

<h2 align="center">Definitions</h2>

<span>📀</span> <span>`SQL IS A STANDARD LANGUAGE FOR STORING, MANIPULATION AND RETRIEVING DATA IN DATABASES.`</span>

<span>📀</span> <span>`SGBD SIGNIFICA SISTEMA DE GERENCIAMENTO DE BANCO DE DADOS.`</span>

<span>📀</span> <span>`CRUD É UMA SIGLA PARA OS TERMOS CREATE, READ, UPDATE AND DELETE.`</span>

<h2 align="center">Cardinalidade</h2>

<a href="https://pt.wikipedia.org/wiki/Normaliza%C3%A7%C3%A3o_de_dados">https://pt.wikipedia.org/wiki/Normaliza%C3%A7%C3%A3o_de_dados</a>

<a href="https://www.luis.blog.br/primeira-forma-normal-1fn-normalizacao-de-dados">https://www.luis.blog.br/primeira-forma-normal-1fn-normalizacao-de-dados</a>

<h3 align="center">Primeiro Algarismo: Obrigatoriedade</h3>

🌂 `0 - NÃO OBRIGATÓRIO`

🌂 `1 - OBRIGATÓRIO`

<h3 align="center">Segundo Algarismo: Cardinalidade</h3>

🌂 `0 - MÁXIMO DE UM`

🌂 `1 - MAIS DE UM`

<h2 align="center">CTE</h2>

<h3 align="center">TABELA PARA REALIZAR A CONSULTA</h3>

```
  CREATE TABLE tree (
    id serial PRIMARY KEY,
    parent_id INT NOT NULL,
    child_id INT NOT NULL,
    created_At TIMESTAMP WITH TIME ZONE NOT NULL DEFAULT current_timestamp
  );
```

```
  ----------------------------------------------------------------------------------------------------------------
  --                                                                                                            --
  --                                            (1)                                                             --
  --                                             -                                                              --
  --                      -------------------------------------------------------------------------------       --
  --                      -                                         -                                   -       --
  --                     (2)                                       (3)                                  -       --
  --                      -                                         -                                   -       --
  --             ------------------                                 ---------                           -       --
  --             -                -                                         -                           -       --
  --            (4)              (5)                                       (6)                          -       --
  --             -                                                          -                           -       --
  --     ---------                                            -----------------------------             -       --
  --     -                                                    -            -               -            -       --
  --   (11)                                                  (7)          (8)             (9)          (10)     --
  --                                                                                                            --
  ----------------------------------------------------------------------------------------------------------------
```

```sql
  INSERT INTO tree (parent_id, child_id, created_at) VALUES (1, 2, now());
  INSERT INTO tree (parent_id, child_id, created_at) VALUES (2, 4, now());
  INSERT INTO tree (parent_id, child_id, created_at) VALUES (2, 5, now());
  INSERT INTO tree (parent_id, child_id, created_at) VALUES (3, 6, now());
  INSERT INTO tree (parent_id, child_id, created_at) VALUES (6, 7, now());
  INSERT INTO tree (parent_id, child_id, created_at) VALUES (6, 8, now());
  INSERT INTO tree (parent_id, child_id, created_at) VALUES (6, 9, now());
  INSERT INTO tree (parent_id, child_id, created_at) VALUES (1, 10, now());
  INSERT INTO tree (parent_id, child_id, created_at) VALUES (4, 11, now());
```

```sql
  WITH RECURSIVE like_tree AS (
  SELECT
      id,
      child_id,
      parent_id
  FROM
      tree
  WHERE
      ( parent_id = 4 OR parent_id IN ( SELECT parent_id FROM tree WHERE child_id = 4 ) ) -- TENTAR ARRUMAR AQUI, RETORNANDO TODOS OS PARENTS
  UNION
      SELECT
        t.id,
        t.child_id,
        t.parent_id
      FROM
        tree t
      INNER JOIN like_tree lt ON lt.child_id = t.parent_id
  ) SELECT
      *
    FROM
      like_tree;
```