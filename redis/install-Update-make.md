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

5. Substituir o Redis 5 pela versão 6.2.7
A versão instalada do Redis pode substituir a versão anterior se o caminho de instalação for o mesmo. Caso tenha uma versão antiga do Redis rodando, pare o serviço:
```
sudo systemctl stop redis
sudo cp src/redis-server /usr/local/bin/
sudo cp src/redis-cli /usr/local/bin/
```

6. Verificar a versão instalada
Para garantir que a versão correta foi instalada, verifique a versão do Redis:
```
redis-server --version
```

7. Reiniciar o serviço do Redis
Se você estiver usando o Redis como um serviço, reinicie-o:
```
sudo systemctl start redis
```

8. Verificar funcionamento
Você pode verificar se o Redis está funcionando corretamente conectando-se a ele com o comando redis-cli e executando um comando como PING:
```
redis-cli
127.0.0.1:6379> PING
PONG

```

























