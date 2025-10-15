# Ambiente PySpark com Jupyter

Este projeto contém um exemplo de processamento de dados usando PySpark.

## Instruções

1. Construa a imagem Docker:
```bash
docker build -t meu-ambiente-pyspark .
```

2. Execute o script PySpark:
```bash
docker run -it --rm meu-ambiente-pyspark
```

3. Para executar em modo Jupyter Notebook (opcional):
```bash
docker run -it --rm -p 8888:8888 meu-ambiente-pyspark jupyter notebook --ip 0.0.0.0 --no-browser --allow-root
```