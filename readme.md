# Constrói a imagem (certifique-se de que codigo spark e Dockerfile estão na pasta)
docker build -t meu-ambiente-pyspark .

# Mapeia a porta (-p 8888:8888) e monta um volume para persistência dos dados
docker run -it --rm -p 8888:8888 -v ~/folder_data:/home/projetos/pyspark/data meu-ambiente-pyspark

# Rodando PySpark com Docker

Este projeto contém um script PySpark para rodar dentro de um container Docker.

Passos para executar

Construa a imagem Docker:

# docker build -t meu-ambiente-pyspark .


Rode o container mapeando a pasta local e abrindo o terminal do container:

# docker run -it --rm -v C:/Users/Administrador/Desktop/Aulas_BigData/teste_spark:/home/jovyan/work meu-ambiente-pyspark bash


Dentro do container, execute o script PySpark:

