# 🚀 Ambiente Big Data com Spark, Hive e HDFS

Este ambiente Docker Compose levanta um **mini cluster de Big Data** com os seguintes componentes:

- **HDFS (Hadoop Distributed File System)**: Armazenamento distribuído de dados.
- **Spark**: Motor de processamento distribuído (modo standalone).
  - Spark Master + 1 Worker.
- **Hive**: Data Warehouse SQL sobre o HDFS.
  - Inclui Hive Metastore e HiveServer2.
- **PostgreSQL**: Banco de dados do metastore do Hive.

O ambiente é modular — você pode adicionar outros serviços facilmente (Kafka, Airflow, Zeppelin, etc.).

## 🧩 Estrutura do projeto

```
ambiente_bigdata/
├── docker-compose.yml
└── README.md
```

Principais volumes criados:
- `pgdata`: dados do metastore do Hive (Postgres)
- `hdfs-namenode` e `hdfs-datanode`: dados do HDFS
- `hive-warehouse`: diretório `/user/hive/warehouse`

---

## ⚙️ Como subir o ambiente

### 1. Subir os containers
```bash
docker compose up -d
```

### 2. Verificar status
```bash
docker compose ps
```

### 3. Acompanhar logs
```bash
docker compose logs -f
```

### 4. Parar o ambiente
```bash
docker compose down
```

### Para limpar volumes e dados persistidos:
```bash
docker compose down -v
```

---

## 🌐 Portas e interfaces

| Serviço        | Porta | Descrição |
|----------------|-------|------------|
| Spark Master UI | 8080 | Interface Web do Spark Master |
| Spark Worker UI | 8081 | Interface Web do Spark Worker |
| Hadoop NameNode UI | 9870 | Monitoramento do HDFS |
| HiveServer2 (Thrift) | 10000 | Conexão JDBC/Beeline |
| Hive Metastore Thrift | 9083 | Comunicação interna do Hive |
| PostgreSQL | 5432 | Banco de dados do metastore |

---

## 🧠 Testando o Hive

### Acessar o Hive via Beeline
```bash
docker exec -it hive /opt/hive/bin/beeline -u jdbc:hive2://localhost:10000 -n hive
```

### Exemplo de comando HiveQL
```sql
CREATE DATABASE teste;
USE teste;
CREATE TABLE exemplo (id INT, nome STRING);
INSERT INTO exemplo VALUES (1, 'SparkHive');
SELECT * FROM exemplo;
```

---

## ⚡ Testando o Spark

### Acessar o container do Spark Master
```bash
docker exec -it spark-master /bin/bash
```

### Rodar o shell do Spark
```bash
spark-shell --master spark://spark-master:7077
```

### Teste rápido no shell Scala
```scala
val data = spark.range(10)
data.show()
```


## 🗂️ Resumo do docker-compose.yml

|------------------|-------------------------|-------------------------------|
| Serviço          |      Imagem base        |            Função             |
|------------------|-------------------------|-------------------------------|
| **postgres**     | postgres:13             | Armazena o metastore do Hive  |
| **namenode**     | bde2020/hadoop-namenode | Gerencia metadados do HDFS    |
| **datanode**     | bde2020/hadoop-datanode | Armazena blocos de dados      |
| **spark-master** | bde2020/spark-master    | Coordena o cluster Spark      |
| **spark-worker** | bde2020/spark-worker    | Executa tarefas distribuídas  |
| **hive**         | bde2020/hive            | Data Warehouse SQL sobre HDFS |
|------------------|-------------------------|-------------------------------|


## 📚 Referências

- [Apache Spark Documentation](https://spark.apache.org/docs/latest/)
- [Apache Hive Documentation](https://cwiki.apache.org/confluence/display/Hive/)
- [Big Data Europe Docker Images](https://github.com/big-data-europe)
- [Hadoop HDFS Overview](https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html)

## Dicas
Dentro de um notebook Python (PySpark):

```python
from pyspark.sql import SparkSession

spark = SparkSession.builder \
    .appName("JupyterSparkHive") \
    .master("spark://spark-master:7077") \
    .config("spark.sql.catalogImplementation", "hive") \
    .config("hive.metastore.uris", "thrift://hive:9083") \
    .enableHiveSupport() \
    .getOrCreate()

spark.sql("SHOW DATABASES").show()
```