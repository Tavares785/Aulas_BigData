# Constrói a imagem
docker build -t meu-ambiente-pyspark .

# Executa o script PySpark
docker run --rm meu-ambiente-pyspark

# Para usar Jupyter (opcional)
docker run -it --rm -p 8888:8888 meu-ambiente-pyspark jupyter lab --ip=0.0.0.0 --allow-root