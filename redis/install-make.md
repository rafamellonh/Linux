## Compile Redis server


1. Baixar o Redis 6.2.7
Primeiro, baixe o código-fonte da versão desejada (6.2.7) diretamente do site oficial do Redis ou do GitHub.
```
cd /tmp
curl -O http://download.redis.io/releases/redis-6.2.7.tar.gz
```

2. Extrair o arquivo
Extraia o arquivo tar.gz baixado.

```
tar xzf redis-6.2.7.tar.gz
cd redis-6.2.7
```

3. Instalar dependências
Certifique-se de ter as dependências necessárias instaladas. Para sistemas baseados em Debian/Ubuntu, você pode usar o seguinte comando:
```
sudo apt-get update
sudo apt-get install build-essential tcl
```

4. Compilar e instalar
Agora, compile e instale o Redis.
```
make
sudo make install
```
