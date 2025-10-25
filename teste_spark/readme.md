# Ambiente PySpark com Docker
## Este repositório contém um exemplo simples de como configurar um ambiente PySpark usando Docker. O objetivo é facilitar o desenvolvimento e a execução de scripts PySpark sem a necessidade de instalar o Apache Spark localmente.

### Execicio para TF:
1. Faça um fork do repositorio e ajuste o codigo do script_pyspark.py para corrigir o erro.
2. Depois faça um pull request para o repositorio original.



## HELPS:
### Constrói a imagem (certifique-se de que codigo spark e Dockerfile estão na pasta)
docker build -t meu-ambiente-pyspark .

### Mapeia a porta (-p 8888:8888) e monta um volume para persistência dos dados
docker run -it --rm -p 8888:8888 -v ~/folder_data:/home/projetos/pyspark/data meu-ambiente-pyspark