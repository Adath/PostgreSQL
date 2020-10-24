<p align="center"><img height=300 width=300 src="Postgresql_elephant.svg"></p>

<h1 align="center">PostgreSQL</h1>

<p align="center">Repository containing the queries developed in <a href="https://www.postgresql.org">PostgreSQL</a></p>

<h3 align="center">References</h3>

<span>👹</span> <a href="https://www.w3schools.com">w3schools</a>

<span>👹</span> <a href="https://www.postgresqltutorial.com">https://www.postgresqltutorial.com</a>

<span>👹</span> <a href="https://www.edivaldobrito.com.br/pgadmin4-no-ubuntu">Como instalar o pgAdmin4 no Ubuntu e derivados</a>

<span>👹</span> <a href="https://www.edivaldobrito.com.br/postgresql-no-debian">Como instalar o mais recente SGBD PostgreSQL no Debian e derivados</a>

<span>👹</span> <a href="https://www.tutorialspoint.com/postgresql/postgresql_c_cpp.htm">PostgreSQL - C/C++ Interface</a>

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

<h2 align="center">Commands</h2>

<a href="http://www.postgresqltutorial.com/psql-commands">17 Practical psql Commands That You Don’t Want To Miss</a>

<a href="https://www.postgresql.org/docs/9.1/functions-string.html">9.4. String Functions and Operators</a>

<a href="https://www.gagno.com/14-manipulacoes-com-string-no-postgres">14 manipulações com String no Postgres</a>

<h4 align="center">LISTAR TODOS OS BANCOS DE DADOS PRESENTES</h4>

```sql
  /list
```

```sql
  /l
```

<h4 align="center">ACESSAR UM BANCO DE DADOS</h4>

```sql
  \c <nome_do_banco>
```

<h4 align="center">SAIR DO SERVIDOR</h4>

```sql
  \q
```
<h4 align="center">MOSTRA AJUDA SOBRE TODOS OS COMANDOS</h4>

```sql
  \h
```

```sql
  \h <command>
```

<h4 align="center">AJUDA GERAL DOS COMANDOS DO PSQL</h4>

```sql
  \?
```

<h4 align="center">MOSTRAR VERSÃO ATUAL DO POSTGRES</h4>

```sql
  SELECT version();
```

<h4 align="center">DESCREVER UMA TABELA</h4>

```sql
  \d <schema>.<table>;
```

<h4 align="center">SELECIONAR TODAS AS CONEXÕES ATIVAS</h4>

```sql
  SELECT * FROM pg_stat_activity;
```

<h4 align="center">FINALIZAR UM PROCESSO PELO PID</h4>

```sql
SELECT pg_terminate_backend(<NUMERO_DO_PROCESSO>);
```

```sql
  SELECT pg_terminate_backend(PID) FROM pg_stat_activity WHERE username = "some-role";
```

<h2 align="center">CTE</h2>

<h3 align="center">Table creation</h3>

```
  CREATE TABLE tree (
    id serial PRIMARY KEY,
    parent_id INT NOT NULL,
    child_id INT NOT NULL,
    created_At TIMESTAMP WITH TIME ZONE NOT NULL DEFAULT CURRENT_TIMESTAMP
  );
```

```
  -----------------------------------------------------------------------------------------------------------
  --                                                                                                       --
  --                                           (1)                                                         --
  --                                            -                                                          --
  --                    ----------------------------------------------------------                         --
  --                    -                                         -              -                         --
  --                   (2)                                       (3)             -------                   --
  --                    -                                         -                     -                  --
  --            -----------------                                 ---------             ----------         --
  --            -               -                                         -                      -         --
  --           (4)             (5)                                       (6)                     ------    --
  --            -                                                         -                           -    --
  --    ---------                                           -----------------------------             -    --
  --    -                                                   -             -             -             -    --
  --  (11)                                                 (7)           (8)           (9)          (10)   --
  --                                                                                                       --
  -----------------------------------------------------------------------------------------------------------
```

```sql
  INSERT INTO tree (parent_id, child_id, created_at) VALUES (1,  2, now());
  INSERT INTO tree (parent_id, child_id, created_at) VALUES (2,  4, now());
  INSERT INTO tree (parent_id, child_id, created_at) VALUES (2,  5, now());
  INSERT INTO tree (parent_id, child_id, created_at) VALUES (3,  6, now());
  INSERT INTO tree (parent_id, child_id, created_at) VALUES (6,  7, now());
  INSERT INTO tree (parent_id, child_id, created_at) VALUES (6,  8, now());
  INSERT INTO tree (parent_id, child_id, created_at) VALUES (6,  9, now());
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

<h2 align="center">CASE WHEN</h2>

```sql
  CREATE TABLE colors (
    id SERIAL PRIMARY KEY,
    color VARCHAR(32) NOT NULL,
    created_at TIMESTAMP WITHOUT TIME ZONE DEFAULT CURRENT_TIMESTAMP
  );
```

```sql
  INSERT INTO colors (color) VALUES ('red');
  INSERT INTO colors (color) VALUES ('green');
  INSERT INTO colors (color) VALUES ('orange');
  INSERT INTO colors (color) VALUES ('blue');
  INSERT INTO colors (color) VALUES ('gray');
  INSERT INTO colors (color) VALUES ('yellow');
  INSERT INTO colors (color) VALUES ('white');
  INSERT INTO colors (color) VALUES ('pink');
```

```sql
  SELECT colors.* FROM public.colors;
```

```sql
  SELECT
    colors.color,
    colors.id,
    CASE
      WHEN (colors.id > 0 AND colors.id < 4) THEN 'greater than 0 and less than 4'
      WHEN (colors.id = 4) THEN 'equal 4'
      WHEN (colors.id > 4) THEN 'greater than 4'
    END
  FROM
    colors;
```

<h2 align="center">DATABASE</h2>

```sql
  CREATE DATABASE <nome_do_banco_de_dados>
```

<h4 align="center">CRIAÇÃO DE UM BANCO DE DADOS A PARTIR DE UM TEMPLATE</h4>

```sql
  CREATE DATABASE <nome_do_banco> TEMPLATE <nome_do_banco_template>
```

<h4 align="center">BANCO TEMPLATE - PROTEGER O BANCO DE DADOS CONTRA ALTERAÇÕES</h4>

```sql
  UPDATE pg_database SET datistemplate = TRUE WHERE datname = <nome_do_banco>
```

<h4 align="center">SELECIONAR O BANCO DE DADOS EM QUE ESTAMOS LOGADO</h4>

```sql
  SELECT current_database();
```

<h2 align="center">TABLES</h2>

<a href="https://www.postgresqltutorial.com/postgresql-create-table">PostgreSQL CREATE TABLE</a>

<a href="https://www.postgresql.org/docs/current/sql-createtable.html">CREATE TABLE</a>

```sql
  CREATE TABLE logs (
    id SERIAL PRIMARY KEY,
    user VARCHAR (64),
    description text,
    log_ts TIMESTAMP WITH TIME ZONE NOT NULL DEFAULT CURRENT_TIMESTAMP
  );
```

```sql
  CREATE TABLE meu_esquema.logs (
    id serial PRIMARY KEY,
    user VARCHAR (64),
    description text,
    log_ts TIMESTAMP WITH TIME ZONE NOT NULL DEFAULT CURRENT_TIMESTAMP
  );
```

```sql
  CREATE TABLE public.account(
    user_id serial PRIMARY KEY,
    username VARCHAR (64) UNIQUE NOT NULL,
    password VARCHAR (64) NOT NULL,
    email VARCHAR (355) UNIQUE NOT NULL,
    description TEXT NOT NULL, -- Allow empty values (How to prevent this behavior?)
    created_on TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP, -- How prevent to edit this field once the register was created?
    last_login TIMESTAMP
  );
```

```sql
  INSERT INTO account (username, password, email, description) VALUES ('johndoe', 'secret', 'johndoe@gmail.com', 'example');
```

```sql
  SELECT * FROM public.account;
```

<h3 align="center">TABELA-HERANÇA</h3>

```sql
  CREATE TABLE logs_2017 (PRIMARY KEY(log_id)) INHERITS (logs0);
```

```sql
  ALTER TABLE logs_2017 ADD CONSTRAINT chk_y2016 CHECK (log_ts >= '2011-1-1'::timestamptz AND log_ts < '2016-1-1'::timestamptz );
```

<a href="https://www.postgresql.org/docs/current/tutorial-inheritance.html">3.6. Inheritance</a>

<h3 align="center">TABELA-UNLOGGED</h3>

<a href="http://web.archive.org/web/20170724234315/https://www.compose.com/articles/faster-performance-with-unlogged-tables-in-postgresql">Faster Performance with Unlogged Tables in PostgreSQL</a>

```sql
  CREATE UNLOGGED TABLE sessoes_web (id_sessão text PRIMARY KEY, add_ts timestamptz, upd_ts timestamptz, estado_sessão xml);
```

<h3 align="center">TABELA-TYPEOF</h3>

```sql
  CREATE TYPE user_básico AS (user varchar(50), pwd varchar(10));
```

```sql
  CREATE TABLE super_user OF user_básico (CONSTRAINT pk_su PRIMARY KEY (user));
```

```sql
  ALTER TYPE user_básico ADD ATTRIBUTE telefone varchar(10) CASCADE;
```

<h3 align="center">TABELA-GENERATED-ALWAYS</h3>

```sql
  CREATE TABLE testing (
    id SERIAL PRIMARY KEY,
    meter INTEGER NOT NULL,
    km NUMERIC (8, 3) GENERATED ALWAYS AS (meter::INTEGER / 1000::NUMERIC(8, 3)) STORED NOT NULL
  );
```

```sql
  INSERT INTO testing (km) VALUES (10);
```

<h2 align="center">Other Topics</h2>

```sql
  CREATE TABLE t_oil (
    id SERIAL PRIMARY KEY,
    region VARCHAR(32),
    country VARCHAR(32),
    year INT,
    production INT,
    consumption INT
  );
```

```sql
  COPY t_oil (region, country, year, production, consumption) FROM PROGRAM 'curl https://www.cybertec-postgresql.com/secret/oil_ext.txt';
```

```sql
  SELECT region, avg(production) FROM t_oil GROUP BY region;
```

```sql
  SELECT region, avg(production) FROM t_oil GROUP BY ROLLUP (region);
```

```sql
  SELECT 
    region,
    avg(production),
    avg(production) FILTER (WHERE year < 1990) AS old,
    avg(production) FILTER (WHERE year >= 1990) AS new
  FROM
    t_oil
  GROUP BY region;
```