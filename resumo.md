SQL RESUMO

- Resumo para a prova de SQL com todos os comandos e exemplos necessários para a prova. FEITO POR MARCELO RABELLO

<!-- table -->
| Comando | Descrição | Exemplo |
| --- | --- | --- |
| `SELECT` | Seleciona dados de uma tabela | `SELECT * FROM tabela` |
| `WHERE` | Filtra registros de uma tabela | `SELECT * FROM tabela WHERE coluna = valor` |
| `ORDER BY` | Ordena os registros de uma tabela | `SELECT * FROM tabela ORDER BY coluna` |
| `GROUP BY` | Agrupa registros de uma tabela | `SELECT coluna, COUNT(*) FROM tabela GROUP BY coluna` |
| `JOIN` | Combina registros de duas ou mais tabelas | `SELECT * FROM tabela1 JOIN tabela2 ON tabela1.coluna = tabela2.coluna` |
| `HAVING` | Filtra grupos de registros depois do `GROUP BY` | `SELECT coluna, COUNT(*) FROM tabela GROUP BY coluna HAVING COUNT(*) > 1` |
| `UNION` | Combina o resultado de duas ou mais consultas | `SELECT coluna FROM tabela1 UNION SELECT coluna FROM tabela2` |
| `INSERT INTO` | Insere novos registros em uma tabela | `INSERT INTO tabela (coluna1, coluna2) VALUES (valor1, valor2)` |
| `UPDATE` | Atualiza registros de uma tabela | `UPDATE tabela SET coluna = valor WHERE coluna = valor` |
| `DELETE` | Exclui registros de uma tabela | `DELETE FROM tabela WHERE coluna = valor` |
| `ALTER TABLE` | Modifica a estrutura de uma tabela | `ALTER TABLE tabela ADD coluna tipo` |
| `CREATE TABLE` | Cria uma nova tabela | `CREATE TABLE tabela (coluna1 tipo1, coluna2 tipo2)` |
| `DROP TABLE` | Exclui uma tabela | `DROP TABLE tabela` |
| `AVG()` | Retorna a média dos valores de uma coluna | `SELECT AVG(coluna) FROM tabela` |
| `COUNT()` | Retorna o número de registros de uma tabela | `SELECT COUNT(*) FROM tabela` |
| `MAX()` | Retorna o maior valor de uma coluna | `SELECT MAX(coluna) FROM tabela` |
| `MIN()` | Retorna o menor valor de uma coluna | `SELECT MIN(coluna) FROM tabela` |
| `SUM()` | Retorna a soma dos valores de uma coluna | `SELECT SUM(coluna) FROM tabela` |
| `COALESCE()` | Retorna o primeiro valor não nulo de uma lista | `SELECT COALESCE(coluna1, coluna2, valor) FROM tabela` |
| `CONCAT()` | Concatena duas ou mais strings | `SELECT CONCAT(coluna1, coluna2) FROM tabela` |
| `LIKE` | Filtra registros com base em um padrão | `SELECT * FROM tabela WHERE coluna LIKE 'padrão'` |
| `IN` | Filtra registros com base em uma lista de valores | `SELECT * FROM tabela WHERE coluna IN (valor1, valor2)` |
| `BETWEEN` | Filtra registros com base em um intervalo de valores | `SELECT * FROM tabela WHERE coluna BETWEEN valor1 AND valor2` |
| `IS NULL` | Filtra registros com valores nulos | `SELECT * FROM tabela WHERE coluna IS NULL` |
| `IS NOT NULL` | Filtra registros com valores não nulos | `SELECT * FROM tabela WHERE coluna IS NOT NULL` |
| `DISTINCT` | Retorna valores únicos em uma coluna | `SELECT DISTINCT coluna FROM tabela` |
| `AS` | Renomeia uma coluna ou tabela | `SELECT coluna AS novo_nome FROM tabela` |
| `LIMIT` | Limita o número de registros retornados | `SELECT * FROM tabela LIMIT 10` |
| `OFFSET` | Ignora um número específico de registros | `SELECT * FROM tabela LIMIT 10 OFFSET 5` |
| `CASE` | Cria uma condição condicional | `SELECT coluna, CASE WHEN condicao THEN valor ELSE outro_valor END FROM tabela` |

<!-- sql -->
```sql
-- Cria a tabela de exemplo
CREATE TABLE exemplo (
    id INT PRIMARY KEY AUTO_INCREMENT,
    nome VARCHAR(50),
    idade INT
);

-- Insere registros na tabela
INSERT INTO exemplo (id, nome, idade) VALUES (1, 'Fulano', 30);
INSERT INTO exemplo (id, nome, idade) VALUES (2, 'Ciclano', 25);
INSERT INTO exemplo (id, nome, idade) VALUES (3, 'Beltrano', 35);

-- Seleciona todos os registros da tabela
SELECT * FROM exemplo;

-- return
| id | nome     | idade |
| -- | -------- | ----- |
| 1  | Fulano   | 30    |
| 2  | Ciclano  | 25    |
| 3  | Beltrano | 35    |


-- Atualiza o registro com id 2
UPDATE exemplo SET nome = 'Ciclano Silva' WHERE id = 2;

-- Exclui o registro com id 3
DELETE FROM exemplo WHERE id = 3;

-- return
| id | nome          | idade |
| -- | ------------- | ----- |
| 1  | Fulano        | 30    |
| 2  | Ciclano Silva | 25    |


-- Exclui a tabela de exemplo
DROP TABLE exemplo;
```



# Estudo para a prova de SQL

tabela que vamos usar para todos os exemplos

```sql

CREATE TABLE employee (
  emp_id INT PRIMARY KEY,
  first_name VARCHAR(40),
  last_name VARCHAR(40),
  birth_day DATE,
  sex VARCHAR(1),
  salary INT,
  super_id INT,
  branch_id INT
);

CREATE TABLE branch (
  branch_id INT PRIMARY KEY,
  branch_name VARCHAR(40),
  mgr_id INT,
  mgr_start_date DATE,
  FOREIGN KEY(mgr_id) REFERENCES employee(emp_id) ON DELETE SET NULL
);

ALTER TABLE employee
ADD FOREIGN KEY(branch_id)
REFERENCES branch(branch_id)
ON DELETE SET NULL;

ALTER TABLE employee
ADD FOREIGN KEY(super_id)
REFERENCES employee(emp_id)
ON DELETE SET NULL;

CREATE TABLE client (
  client_id INT PRIMARY KEY,
  client_name VARCHAR(40),
  branch_id INT,
  FOREIGN KEY(branch_id) REFERENCES branch(branch_id) ON DELETE SET NULL
);

CREATE TABLE works_with (
  emp_id INT,
  client_id INT,
  total_sales INT,
  PRIMARY KEY(emp_id, client_id),
  FOREIGN KEY(emp_id) REFERENCES employee(emp_id) ON DELETE CASCADE,
  FOREIGN KEY(client_id) REFERENCES client(client_id) ON DELETE CASCADE
);

CREATE TABLE branch_supplier (
  branch_id INT,
  supplier_name VARCHAR(40),
  supply_type VARCHAR(40),
  PRIMARY KEY(branch_id, supplier_name),
  FOREIGN KEY(branch_id) REFERENCES branch(branch_id) ON DELETE CASCADE
);


-- -----------------------------------------------------------------------------

-- Corporate
INSERT INTO employee VALUES(100, 'David', 'Wallace', '1967-11-17', 'M', 250000, NULL, NULL);

INSERT INTO branch VALUES(1, 'Corporate', 100, '2006-02-09');

UPDATE employee
SET branch_id = 1
WHERE emp_id = 100;

INSERT INTO employee VALUES(101, 'Jan', 'Levinson', '1961-05-11', 'F', 110000, 100, 1);

-- Scranton
INSERT INTO employee VALUES(102, 'Michael', 'Scott', '1964-03-15', 'M', 75000, 100, NULL);

INSERT INTO branch VALUES(2, 'Scranton', 102, '1992-04-06');

UPDATE employee
SET branch_id = 2
WHERE emp_id = 102;

INSERT INTO employee VALUES(103, 'Angela', 'Martin', '1971-06-25', 'F', 63000, 102, 2);
INSERT INTO employee VALUES(104, 'Kelly', 'Kapoor', '1980-02-05', 'F', 55000, 102, 2);
INSERT INTO employee VALUES(105, 'Stanley', 'Hudson', '1958-02-19', 'M', 69000, 102, 2);

-- Stamford
INSERT INTO employee VALUES(106, 'Josh', 'Porter', '1969-09-05', 'M', 78000, 100, NULL);

INSERT INTO branch VALUES(3, 'Stamford', 106, '1998-02-13');

UPDATE employee
SET branch_id = 3
WHERE emp_id = 106;

INSERT INTO employee VALUES(107, 'Andy', 'Bernard', '1973-07-22', 'M', 65000, 106, 3);
INSERT INTO employee VALUES(108, 'Jim', 'Halpert', '1978-10-01', 'M', 71000, 106, 3);


-- BRANCH SUPPLIER
INSERT INTO branch_supplier VALUES(2, 'Hammer Mill', 'Paper');
INSERT INTO branch_supplier VALUES(2, 'Uni-ball', 'Writing Utensils');
INSERT INTO branch_supplier VALUES(3, 'Patriot Paper', 'Paper');
INSERT INTO branch_supplier VALUES(2, 'J.T. Forms & Labels', 'Custom Forms');
INSERT INTO branch_supplier VALUES(3, 'Uni-ball', 'Writing Utensils');
INSERT INTO branch_supplier VALUES(3, 'Hammer Mill', 'Paper');
INSERT INTO branch_supplier VALUES(3, 'Stamford Lables', 'Custom Forms');

-- CLIENT
INSERT INTO client VALUES(400, 'Dunmore Highschool', 2);
INSERT INTO client VALUES(401, 'Lackawana Country', 2);
INSERT INTO client VALUES(402, 'FedEx', 3);
INSERT INTO client VALUES(403, 'John Daly Law, LLC', 3);
INSERT INTO client VALUES(404, 'Scranton Whitepages', 2);
INSERT INTO client VALUES(405, 'Times Newspaper', 3);
INSERT INTO client VALUES(406, 'FedEx', 2);

-- WORKS_WITH
INSERT INTO works_with VALUES(105, 400, 55000);
INSERT INTO works_with VALUES(102, 401, 267000);
INSERT INTO works_with VALUES(108, 402, 22500);
INSERT INTO works_with VALUES(107, 403, 5000);
INSERT INTO works_with VALUES(108, 403, 12000);
INSERT INTO works_with VALUES(105, 404, 33000);
INSERT INTO works_with VALUES(107, 405, 26000);
INSERT INTO works_with VALUES(102, 406, 15000);
INSERT INTO works_with VALUES(105, 406, 130000);
```

imagem da tabela:

![tabela](Db.png)

## Exemplos

```sql
-- 1. Selecione todos os funcionários
SELECT * FROM employee;

-- 2. Selecione todos os clientes
SELECT * FROM client;

-- 3. Selecione todos os funcionários que trabalham com o cliente 'FedEx'

SELECT e.first_name, e.last_name
FROM employee e
JOIN works_with w ON e.emp_id = w.emp_id
JOIN client c ON w.client_id = c.client_id
WHERE c.client_name = 'FedEx';

-- 4. Selecione todos os funcionários ordenado pelo sexo e depois pelo nome

SELECT * FROM employee
ORDER BY sex, first_name, last_name;

-- 5. Selecione os 5 primeiros funcionários que nasceram depois de 1970

SELECT * FROM employee
WHERE birth_day > '1970-01-01'
LIMIT 5;
```

- Mais exemplos um pouco mais complexos

```sql
-- 6. Selecione o nome do funcionário, nome do cliente e total de vendas para os funcionários que vendem mais de 30.000

SELECT e.first_name, e.last_name, c.client_name, w.total_sales
FROM employee e
JOIN works_with w ON e.emp_id = w.emp_id
JOIN client c ON w.client_id = c.client_id
WHERE w.total_sales > 30000;

-- 7. Selecione o nome do funcionário que trabalhou com alguma escola (Tem a palavra 'school' no nome do cliente) e o nome do cliente

SELECT e.first_name, e.last_name, c.client_name
FROM employee e
JOIN works_with w ON e.emp_id = w.emp_id
JOIN client c ON w.client_id = c.client_id
WHERE c.client_name LIKE '%school%';

-- 8. Selecione o nome do funcionário e a media de vendas dos funcionários que vendem mais de 30.000

SELECT e.first_name, e.last_name, AVG(w.total_sales) AS media_vendas
FROM employee e
JOIN works_with w ON e.emp_id = w.emp_id
GROUP BY e.emp_id
HAVING AVG(w.total_sales) > 30000;

-- 9. Selecione o nome do funcionário e o nome do cliente que ele vendeu mais (total_sales) e o total de vendas

SELECT e.first_name, e.last_name, c.client_name, MAX(w.total_sales) AS total_vendas
FROM employee e
JOIN works_with w ON e.emp_id = w.emp_id
JOIN client c ON w.client_id = c.client_id
GROUP BY e.emp_id;

-- 10. Selecione o nome do funcionário e o nome do cliente que ele vendeu mais (total_sales) e o total de vendas, ordenado pelo total de vendas decrescente

SELECT e.first_name, e.last_name, c.client_name, MAX(w.total_sales) AS total_vendas
FROM employee e
JOIN works_with w ON e.emp_id = w.emp_id
JOIN client c ON w.client_id = c.client_id
GROUP BY e.emp_id
ORDER BY total_vendas DESC;

-- 11. Selecione o nome do funcionario, o nome do cliente, a quantidade de vendas que o funcionario fez para o cliente e a media de vendas dos funcionarios que trabalham com o cliente

SELECT e.first_name, e.last_name, c.client_name, w.total_sales, AVG(w.total_sales) AS media_vendas
FROM employee e
JOIN works_with w ON e.emp_id = w.emp_id
JOIN client c ON w.client_id = c.client_id
GROUP BY e.emp_id, c.client_id;
```

- Exemplos com subquery

```sql
-- 12. Selecione o nome do funcionário e o nome do cliente que ele vendeu mais (total_sales) e o total de vendas, ordenado pelo total de vendas decrescente

SELECT e.first_name, e.last_name, c.client_name, w.total_sales
FROM employee e
JOIN works_with w ON e.emp_id = w.emp_id
JOIN client c ON w.client_id = c.client_id
WHERE w.total_sales = (
    SELECT MAX(total_sales)
    FROM works_with
    WHERE emp_id = e.emp_id
);

-- Select all employees who are not working with the client 'FedEx'
SELECT e.first_name, e.last_name
FROM employee e
WHERE e.emp_id NOT IN (
    SELECT w.emp_id
    FROM works_with w
    JOIN client c ON w.client_id = c.client_id
    WHERE c.client_name LIKE 'FedEx'
);

```


---

# Resumo PySpark

- Resumo para a prova de PySpark com todos os comandos e exemplos necessários para a prova. FEITO POR MARCELO RABELLO

<!-- table -->
| Comando | Descrição | Exemplo |
| --- | --- | --- |
| `spark.read.csv()` | Lê um arquivo CSV | `df = spark.read.csv('arquivo.csv', header=True, inferSchema=True)` |
| `df.show()` | Mostra os primeiros registros do DataFrame | `df.show()` |
| `df.printSchema()` | Mostra o schema do DataFrame | `df.printSchema()` |
| `df.columns` | Retorna as colunas do DataFrame | `df.columns` |
| `df.describe().show()` | Mostra estatísticas descritivas do DataFrame | `df.describe().show()` |
| `df.select('coluna').show()` | Seleciona uma coluna do DataFrame | `df.select('coluna').show()` |
| `df.filter(df['coluna'] > 10).show()` | Filtra registros do DataFrame | `df.filter(df['coluna'] > 10).show()` |
| `df.groupBy('coluna').count().show()` | Agrupa registros do DataFrame | `df.groupBy('coluna').count().show()` |
| `df.withColumn('nova_coluna', df['coluna'] * 2).show()` | Adiciona uma nova coluna ao DataFrame | `df.withColumn('nova_coluna', df['coluna'] * 2).show()` |
| `df.drop('coluna').show()` | Remove uma coluna do DataFrame | `df.drop('coluna').show()` |
| `df.na.drop().show()` | Remove registros com valores nulos | `df.na.drop().show()` |
| `df.na.fill(0).show()` | Preenche valores nulos com um valor específico | `df.na.fill(0).show()` |
| `df.write.csv('arquivo.csv', header=True)` | Escreve um DataFrame em um arquivo CSV | `df.write.csv('arquivo.csv', header=True)` |
| `df.write.parquet('arquivo.parquet')` | Escreve um DataFrame em um arquivo Parquet | `df.write.parquet('arquivo.parquet')` |
| `df.write.json('arquivo.json')` | Escreve um DataFrame em um arquivo JSON | `df.write.json('arquivo.json')` |
| `df.write.format('jdbc').options(url='jdbc:mysql://localhost:3306/db', dbtable='tabela', user='user', password='password').save()` | Escreve um DataFrame em um banco de dados | `df.write.format('jdbc').options(url='jdbc:mysql://localhost:3306/db', dbtable='tabela', user='user', password='password').save()` |
| `df.createOrReplaceTempView('tabela')` | Cria uma visão temporária do DataFrame | `df.createOrReplaceTempView('tabela')` |
| `spark.sql('SELECT * FROM tabela').show()` | Executa uma consulta SQL no DataFrame | `spark.sql('SELECT * FROM tabela').show()` |
| `df.rdd` | Retorna o RDD do DataFrame | `df.rdd` |
| `df.toPandas()` | Converte o DataFrame para um DataFrame Pandas | `df.toPandas()` |
| `df.cache()` | Armazena o DataFrame em cache | `df.cache()` |
| `df.unpersist()` | Remove o DataFrame do cache | `df.unpersist()` |

<!-- pyspark -->
```python
# Importa o SparkSession
from pyspark.sql import SparkSession

# Cria a sessão do Spark
spark = SparkSession.builder.appName('exemplo').getOrCreate()

# Lê um arquivo CSV
df = spark.read.csv('arquivo.csv', header=True, inferSchema=True)

# Mostra os primeiros registros do DataFrame
df.show()

# Mostra o schema do DataFrame
df.printSchema()

# Retorna as colunas do DataFrame
df.columns

# Mostra estatísticas descritivas do DataFrame
df.describe().show()

# Seleciona uma coluna do DataFrame
df.select('coluna').show()
```

Vamos supor o seguinte txt (`arquivo.txt`) que tem o seguinte conteúdo:

```txt
2020-11-04 03:15:32
2021-10-19 01:47:58
2022-08-05 11:11:47
2021-09-08 00:02:37
2021-12-13 07:20:51
2023-12-10 09:06:45
2023-09-21 01:02:10
2023-10-20 03:39:38
2022-07-11 05:33:33
2020-07-26 16:59:56
2023-12-19 08:15:43
2023-09-25 02:51:54
...
```

# Resumo de Funções do RDD no PySpark

## 1. Criação de RDDs

### `parallelize`
Cria um RDD a partir de uma coleção.
```python
data = [1, 2, 3, 4, 5]
rdd = sc.parallelize(data)
```

### `textFile`
Lê um arquivo de texto e cria um RDD.
```python
rdd = sc.textFile("timestamps.txt")
```

## 2. Transformações

### `map`
Aplica uma função a cada item do RDD e retorna um novo RDD.
```python
rdd = sc.parallelize([1, 2, 3, 4])
mapped_rdd = rdd.map(lambda x: x * 2)
# Resultado: [2, 4, 6, 8]
```

### `filter`
Retorna um novo RDD contendo apenas os elementos que satisfazem a condição.
```python
filtered_rdd = rdd.filter(lambda x: x > 2)
# Resultado: [3, 4]
```

### `flatMap`
Aplica uma função a cada item do RDD e retorna um RDD de itens individuais (não listas).
```python
rdd = sc.parallelize(["hello world", "hi there"])
flat_mapped_rdd = rdd.flatMap(lambda x: x.split(" "))
# Resultado: ["hello", "world", "hi", "there"]
```

### `reduceByKey`
Agrupa os pares de chave-valor pelo mesmo valor de chave e aplica uma função de redução.
```python
rdd = sc.parallelize([("a", 1), ("b", 1), ("a", 1)])
reduced_rdd = rdd.reduceByKey(lambda x, y: x + y)
# Resultado: [("a", 2), ("b", 1)]
```

### `groupByKey`
Agrupa os valores por chave.
```python
rdd = sc.parallelize([("a", 1), ("b", 1), ("a", 1)])
grouped_rdd = rdd.groupByKey()
# Resultado: [("a", [1, 1]), ("b", [1])]
```

### `distinct`
Retorna um novo RDD contendo apenas elementos distintos do original.
```python
rdd = sc.parallelize([1, 2, 2, 3, 3, 3])
distinct_rdd = rdd.distinct()
# Resultado: [1, 2, 3]
```

### `union`
Une dois RDDs.
```python
rdd1 = sc.parallelize([1, 2])
rdd2 = sc.parallelize([3, 4])
union_rdd = rdd1.union(rdd2)
# Resultado: [1, 2, 3, 4]
```

### takeOrdered
Retorna os primeiros n elementos do RDD, ordenados.
```python
rdd = sc.parallelize([4, 2, 1, 3])
ordered_rdd = rdd.takeOrdered(2)
# Resultado: [1, 2]
```

## 3. Ações

### `collect`
Retorna todos os elementos do RDD para o driver.
```python
result = rdd.collect()
# Resultado: [1, 2, 3, 4]
```

### `count`
Retorna o número de elementos no RDD.
```python
count = rdd.count()
# Resultado: 4
```

### `take`
Retorna os primeiros n elementos do RDD.
```python
taken = rdd.take(2)
# Resultado: [1, 2]
```

### `reduce`
Aplica uma função de redução aos elementos do RDD.
```python
sum = rdd.reduce(lambda x, y: x + y)
# Resultado: 10
```

### `first`
Retorna o primeiro elemento do RDD.
```python
first_element = rdd.first()
# Resultado: 1
```

### `top`
Retorna os primeiros n elementos ordenados.
```python
top_elements = rdd.top(2)
# Resultado: [4, 3]
```

## Exemplos Práticos

### Contagem de Acessos por Ano
```python
def total_acessos(rdd, ano):
    return rdd.filter(lambda x: str(ano) in x).count()

rdd = sc.textFile("timestamps.txt")
print(total_acessos(rdd, "2022"))
print(total_acessos(rdd, "2023"))
```

### Encontrar os 20 Itens Mais Frequentes
```python
def top20(rdd):
    return rdd.map(lambda x: (x[:10], 1))               .reduceByKey(lambda x, y: x + y)               .takeOrdered(20, key=lambda x: -x[1])

rdd = sc.textFile("timestamps.txt")
print(top20(rdd))
```

Este resumo cobre as principais transformações e ações que você pode realizar em RDDs usando PySpark, junto com exemplos práticos de uso. Essas operações permitem manipular e analisar grandes conjuntos de dados distribuídos de maneira eficiente.



Exercicios de exemplo:

- Encontrar a data mais recente
```python
from pyspark.sql.functions import to_timestamp

rdd = sc.textFile("arquivo.txt")

df = rdd.map(lambda x: (x, )).toDF(["data"])

df = df.withColumn("data", to_timestamp("data"))
```


---


## SQL TRIGGERS

- Triggers são procedimentos armazenados que são executados automaticamente em resposta a determinados eventos em uma tabela ou exibição.

- Os eventos que podem acionar um trigger incluem INSERT, UPDATE e DELETE.

### Exemplo de Trigger

```sql
CREATE TRIGGER nome_trigger
AFTER INSERT ON tabela
FOR EACH ROW
BEGIN
    -- Código do trigger
END;
```

### Exemplo de Trigger no MySQL

```sql
DELIMITER //

CREATE TRIGGER insere_log
AFTER INSERT ON funcionarios
FOR EACH ROW
BEGIN
    INSERT INTO logs (mensagem) VALUES ('Novo funcionário inserido');
END//

DELIMITER ;
```

### Exemplos com exercícios

- Um trigger que insere um registro na tabela `logs` sempre que um novo funcionário é inserido na tabela `funcionarios`.

```sql
DELIMITER //

CREATE TRIGGER insere_log
AFTER INSERT ON funcionarios
FOR EACH ROW
BEGIN
    INSERT INTO logs (mensagem) VALUES ('Novo funcionário inserido');
END//

DELIMITER ;
```

- Um trigger que atualiza a coluna `ultima_atualizacao` na tabela `clientes` sempre que um cliente é atualizado.

```sql
DELIMITER //

CREATE TRIGGER atualiza_data
AFTER UPDATE ON clientes
FOR EACH ROW
BEGIN
    UPDATE clientes
    SET ultima_atualizacao = NOW()
    WHERE id = NEW.id;
END//

DELIMITER ;
```

- Um trigger que exclui todos os pedidos associados a um cliente quando o cliente é excluído.

```sql
DELIMITER //

CREATE TRIGGER exclui_pedidos
AFTER DELETE ON clientes
FOR EACH ROW
BEGIN
    DELETE FROM pedidos
    WHERE cliente_id = OLD.id;
END//
```

- Um trigger que impede a exclusão de um departamento se houver funcionários associados a ele.

```sql
DELIMITER //

CREATE TRIGGER impede_exclusao
BEFORE DELETE ON departamentos
FOR EACH ROW
BEGIN
    DECLARE total INT;
    SELECT COUNT(*) INTO total
    FROM funcionarios
    WHERE departamento_id = OLD.id;

    IF total > 0 THEN
        SIGNAL SQLSTATE '45000'
        SET MESSAGE_TEXT = 'Não é possível excluir o departamento';
    END IF;
END//

DELIMITER ;
```

- Um trigger que verifica o genero de um funcionario novo e adiciona em logs a mensagem de "Novo funcionario do genero masculino" ou "Novo funcionario do genero feminino"

```sql
DELIMITER //

CREATE TRIGGER insere_log_genero
AFTER INSERT ON funcionarios
FOR EACH ROW
BEGIN
    IF NEW.genero = 'M' THEN
        INSERT INTO logs (mensagem) VALUES ('Novo funcionário do gênero masculino');
    ELSE
        INSERT INTO logs (mensagem) VALUES ('Novo funcionário do gênero feminino');
    END IF;
END//

DELIMITER ;
```

---

## Credenciais de Acesso

Aqui estão alguns exemplos de comandos de MySQL para gerenciar permissões:

### Concedendo Permissões

1. **Concedendo Todas as Permissões em um Banco de Dados**

```sql
GRANT ALL PRIVILEGES ON meu_banco_de_dados.* TO 'usuario'@'localhost';
```

2. **Concedendo Permissão SELECT em uma Tabela Específica**

```sql
GRANT SELECT ON meu_banco_de_dados.minha_tabela TO 'usuario'@'localhost';
```

3. **Concedendo Permissão INSERT em Colunas Específicas de uma Tabela**

```sql
GRANT INSERT (coluna1, coluna2) ON meu_banco_de_dados.minha_tabela TO 'usuario'@'localhost';
```

4. **Concedendo Permissões de Execução em um Procedimento Armazenado**

```sql
GRANT EXECUTE ON PROCEDURE meu_banco_de_dados.meu_procedimento TO 'usuario'@'localhost';
```

### Revogando Permissões

1. **Revogando Todas as Permissões em um Banco de Dados**

```sql
REVOKE ALL PRIVILEGES ON meu_banco_de_dados.* FROM 'usuario'@'localhost';
```

2. **Revogando Permissão SELECT em uma Tabela Específica**

```sql
REVOKE SELECT ON meu_banco_de_dados.minha_tabela FROM 'usuario'@'localhost';
```

3. **Revogando Permissão INSERT em Colunas Específicas de uma Tabela**

```sql
REVOKE INSERT (coluna1, coluna2) ON meu_banco_de_dados.minha_tabela FROM 'usuario'@'localhost';
```

4. **Revogando Permissões de Execução em um Procedimento Armazenado**

```sql
REVOKE EXECUTE ON PROCEDURE meu_banco_de_dados.meu_procedimento FROM 'usuario'@'localhost';
```

### Visualizando Permissões

1. **Visualizando Permissões de um Usuário**

```sql
SHOW GRANTS FOR 'usuario'@'localhost';
```

### Criando e Gerenciando Usuários

1. **Criando um Novo Usuário**

```sql
CREATE USER 'novo_usuario'@'localhost' IDENTIFIED BY 'senha_segura';
```

2. **Removendo um Usuário**

```sql
DROP USER 'usuario'@'localhost';
```

### Exemplos Combinados

1. **Criando um Usuário e Concedendo Permissões de Leitura em um Banco de Dados**

```sql
CREATE USER 'leitor'@'localhost' IDENTIFIED BY 'senha_leitor';
GRANT SELECT ON meu_banco_de_dados.* TO 'leitor'@'localhost';
```

2. **Criando um Usuário e Concedendo Permissões de Leitura e Escrita em uma Tabela Específica**

```sql
CREATE USER 'editor'@'localhost' IDENTIFIED BY 'senha_editor';
GRANT SELECT, INSERT, UPDATE, DELETE ON meu_banco_de_dados.minha_tabela TO 'editor'@'localhost';
```

Esses exemplos fornecem uma base para gerenciar permissões no MySQL. A configuração adequada das permissões é crucial para garantir a segurança e a funcionalidade do seu banco de dados.

---

## Diferenca formas normais

Claro! Vou dar um exemplo de uma tabela que está na Segunda Forma Normal (2FN) mas não na Terceira Forma Normal (3FN).

### Exemplo de Tabela na 2FN mas não na 3FN

Considere uma tabela que armazena informações sobre pedidos de clientes, onde cada pedido tem um cliente associado e cada cliente está associado a uma cidade específica. 

| PedidoID | ClienteID | ClienteNome | Cidade       | CEP         |
|----------|-----------|-------------|--------------|-------------|
| 1        | 100       | João Silva  | São Paulo    | 01000-000   |
| 2        | 101       | Maria Souza | Rio de Janeiro| 20000-000  |
| 3        | 100       | João Silva  | São Paulo    | 01000-000   |
| 4        | 102       | Ana Lima    | Belo Horizonte| 30000-000  |

#### Análise:

- **Chave Primária:** `PedidoID`
- **Dependência Total da Chave Primária:** Todos os atributos não-chave (ClienteID, ClienteNome, Cidade, CEP) dependem da chave primária `PedidoID`.

### Por que está na 2FN?
A tabela está na 2FN porque todos os atributos não-chave são totalmente dependentes da chave primária `PedidoID`. Não há dependências parciais, pois `PedidoID` é uma chave simples (não composta).

### Por que não está na 3FN?
A tabela não está na 3FN porque há uma dependência transitiva. `Cidade` e `CEP` dependem de `ClienteID`, que não é a chave primária.

#### Dependência Transitiva:
- `PedidoID` → `ClienteID`
- `ClienteID` → `Cidade`, `CEP`

Para colocar esta tabela na 3FN, precisamos eliminar a dependência transitiva, separando os dados de clientes e suas cidades em tabelas diferentes.

### Normalizando para a 3FN

**Tabela de Pedidos:**

| PedidoID | ClienteID |
|----------|-----------|
| 1        | 100       |
| 2        | 101       |
| 3        | 100       |
| 4        | 102       |

**Tabela de Clientes:**

| ClienteID | ClienteNome | Cidade          | CEP         |
|-----------|-------------|-----------------|-------------|
| 100       | João Silva  | São Paulo       | 01000-000   |
| 101       | Maria Souza | Rio de Janeiro  | 20000-000   |
| 102       | Ana Lima    | Belo Horizonte  | 30000-000   |

### Resumo:
- A tabela inicial estava na 2FN porque todos os atributos não-chave dependiam totalmente da chave primária `PedidoID`.
- A tabela inicial não estava na 3FN porque `Cidade` e `CEP` dependiam de `ClienteID`, criando uma dependência transitiva.
- Ao separar em tabelas de Pedidos e Clientes, eliminamos a dependência transitiva, colocando os dados na 3FN.

---

## Formas normais revisao

As formas normais são regras usadas na modelagem de bancos de dados relacionais para minimizar a redundância e evitar anomalias na manipulação dos dados. As três primeiras formas normais (1FN, 2FN e 3FN) são as mais fundamentais e frequentemente utilizadas. Vou explicar cada uma delas com exemplos:

### Primeira Forma Normal (1FN)
A 1FN estabelece que uma tabela deve ter colunas atômicas (ou seja, cada coluna deve conter valores indivisíveis) e cada coluna deve conter apenas um tipo de dado.

**Exemplo:**
Suponha que temos uma tabela de estudantes com a seguinte estrutura:
| ID | Nome  | Telefones         |
|----|-------|-------------------|
| 1  | Ana   | 12345, 67890      |
| 2  | Bruno | 54321, 98765, 11223|

Essa tabela não está na 1FN porque a coluna "Telefones" contém múltiplos valores em uma única célula. Para colocá-la na 1FN, devemos separar esses valores em linhas diferentes:

| ID | Nome  | Telefone |
|----|-------|----------|
| 1  | Ana   | 12345    |
| 1  | Ana   | 67890    |
| 2  | Bruno | 54321    |
| 2  | Bruno | 98765    |
| 2  | Bruno | 11223    |

### Segunda Forma Normal (2FN)
A 2FN exige que a tabela já esteja na 1FN e que todos os atributos não-chave sejam totalmente dependentes da chave primária. Ou seja, não pode haver dependência parcial de qualquer parte da chave primária.

**Exemplo:**
Considere uma tabela de cursos com a seguinte estrutura (assumindo que a chave primária é composta por "CursoID" e "ProfessorID"):
| CursoID | ProfessorID | NomeCurso | NomeProfessor | Departamento |
|---------|-------------|-----------|---------------|--------------|
| 1       | 10          | Matemática| Dr. Silva     | Ciências     |
| 2       | 11          | Física    | Dr. Sousa     | Ciências     |

Aqui, "NomeCurso" depende apenas de "CursoID" e "NomeProfessor" e "Departamento" dependem apenas de "ProfessorID", caracterizando dependências parciais.

Para colocar na 2FN, devemos criar duas tabelas separadas:
**Tabela de Cursos:**
| CursoID | NomeCurso   |
|---------|-------------|
| 1       | Matemática  |
| 2       | Física      |

**Tabela de Professores:**
| ProfessorID | NomeProfessor | Departamento |
|-------------|---------------|--------------|
| 10          | Dr. Silva     | Ciências     |
| 11          | Dr. Sousa     | Ciências     |

E uma tabela de relacionamento:
| CursoID | ProfessorID |
|---------|-------------|
| 1       | 10          |
| 2       | 11          |

### Terceira Forma Normal (3FN)
A 3FN exige que a tabela já esteja na 2FN e que todos os atributos não-chave sejam mutuamente independentes, ou seja, um atributo não-chave não pode depender de outro atributo não-chave.

**Exemplo:**
Considere a tabela de Professores do exemplo anterior, mas com uma modificação:
| ProfessorID | NomeProfessor | Departamento | ChefeDepartamento |
|-------------|---------------|--------------|-------------------|
| 10          | Dr. Silva     | Ciências     | Dr. Costa         |
| 11          | Dr. Sousa     | Ciências     | Dr. Costa         |

Aqui, "ChefeDepartamento" depende de "Departamento", não diretamente de "ProfessorID". Para normalizar na 3FN, devemos separar essas informações:

**Tabela de Professores:**
| ProfessorID | NomeProfessor | Departamento |
|-------------|---------------|--------------|
| 10          | Dr. Silva     | Ciências     |
| 11          | Dr. Sousa     | Ciências     |

**Tabela de Departamentos:**
| Departamento | ChefeDepartamento |
|--------------|-------------------|
| Ciências     | Dr. Costa         |

Dessa forma, removemos a dependência transitiva e garantimos que todos os atributos não-chave dependem diretamente da chave primária.

Essas são as três primeiras formas normais que ajudam a organizar os dados em um banco de dados relacional, garantindo a minimização da redundância e a integridade dos dados.

---

## RAID-resumo

Entender como os discos rígidos são divididos em diferentes configurações RAID é essencial para saber como cada tipo de RAID otimiza o desempenho e a redundância. Aqui está uma explicação detalhada de como os dados são divididos em RAID 0, RAID 1 e RAID 5:

### RAID 0 (Striping)
- **Divisão dos Dados**: No RAID 0, os dados são divididos em blocos (stripes) e distribuídos uniformemente entre todos os discos do array.
- **Exemplo**: Se você tiver dois discos (Disco A e Disco B) e um arquivo de 1000 MB, metade do arquivo (500 MB) será gravada no Disco A e a outra metade (500 MB) será gravada no Disco B. 
- **Visualização**:
  ```
  Disco A: [Bloco 1, Bloco 3, Bloco 5, ...]
  Disco B: [Bloco 2, Bloco 4, Bloco 6, ...]
  ```

### RAID 1 (Mirroring)
- **Divisão dos Dados**: No RAID 1, cada disco contém uma cópia idêntica dos dados, o que significa que todos os dados são duplicados em cada disco do array.
- **Exemplo**: Se você tiver dois discos (Disco A e Disco B) e um arquivo de 1000 MB, uma cópia completa do arquivo será gravada no Disco A e outra cópia completa será gravada no Disco B.
- **Visualização**:
  ```
  Disco A: [Bloco 1, Bloco 2, Bloco 3, ...]
  Disco B: [Bloco 1, Bloco 2, Bloco 3, ...]
  ```

### RAID 5 (Striping with Parity)
- **Divisão dos Dados**: No RAID 5, os dados e a paridade são distribuídos entre todos os discos do array. A paridade é usada para recuperar dados em caso de falha de um disco.
- **Exemplo**: Se você tiver três discos (Disco A, Disco B, Disco C), os dados e a paridade serão distribuídos assim:
  - Bloco 1 no Disco A, Bloco 2 no Disco B, Paridade 1+2 no Disco C
  - Bloco 3 no Disco B, Bloco 4 no Disco C, Paridade 3+4 no Disco A
  - Bloco 5 no Disco C, Bloco 6 no Disco A, Paridade 5+6 no Disco B
- **Visualização**:
  ```
  Disco A: [Bloco 1, Paridade, Bloco 6, ...]
  Disco B: [Bloco 2, Bloco 3, Paridade, ...]
  Disco C: [Paridade, Bloco 4, Bloco 5, ...]
  ```

### Visualização Geral:

#### RAID 0:
- **Disco 1**: D1-1, D1-2, D1-3, D1-4
- **Disco 2**: D2-1, D2-2, D2-3, D2-4

#### RAID 1:
- **Disco 1**: D1-1, D1-2, D1-3, D1-4
- **Disco 2**: D1-1, D1-2, D1-3, D1-4

#### RAID 5:
- **Disco 1**: D1-1, D1-2, Paridade 3+4, D1-5
- **Disco 2**: D2-1, Paridade 1+2, D2-3, D2-4
- **Disco 3**: Paridade 1+2, D3-1, D3-2, Paridade 5+6


Esses esquemas ajudam a entender como os dados são organizados e protegidos em diferentes configurações RAID. RAID 0 maximiza o desempenho ao distribuir os dados, RAID 1 maximiza a redundância ao duplicar os dados, e RAID 5 equilibra desempenho e redundância distribuindo dados e paridade entre vários discos.

-------

Os níveis de isolamento de transações em bancos de dados relacionais são:

### Read Uncommitted:

Permite que uma transação leia dados que ainda não foram confirmados (committed) por outras transações.
Pode resultar em Dirty Reads, onde uma transação lê dados que podem ser revertidos.
Menor nível de isolamento, com o melhor desempenho, mas menos seguro em termos de consistência.
### Read Committed:

Garante que uma transação só leia dados confirmados (committed) por outras transações.
Evita Dirty Reads, mas permite Non-repeatable Reads e Phantom Reads.
Oferece um bom equilíbrio entre consistência e desempenho.
### Repeatable Read:

Garante que se uma transação lê um dado, ela será capaz de ler o mesmo valor posteriormente, sem que outras transações possam modificar esse dado.
Evita Dirty Reads e Non-repeatable Reads, mas ainda permite Phantom Reads, onde novas linhas podem ser adicionadas por outras transações entre leituras.
Mais consistente que Read Committed, mas com maior overhead.
### Serializable:

O nível mais alto de isolamento, onde a execução de transações ocorre de forma que parece que elas foram executadas uma após a outra, de maneira serial.
Evita Dirty Reads, Non-repeatable Reads e Phantom Reads.
Oferece a maior consistência, mas com o maior custo em termos de desempenho devido ao aumento de bloqueios e contenção.
Cada nível de isolamento fornece um trade-off diferente entre consistência dos dados e desempenho do sistema:

### Read Uncommitted: Máxima performance, mínima consistência.
### Read Committed: Boa performance, consistência razoável.
### Repeatable Read: Maior consistência, desempenho médio.
### Serializable: Máxima consistência, menor desempenho.
A escolha do nível de isolamento depende dos requisitos específicos de consistência e performance da aplicação.
