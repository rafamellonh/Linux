```
#! /bin/bash

PROG=$(basename $0)
SRC=rafael@192.168.2.61:/dados/
DST=/dados/
LOCKFILE=/tmp/$PROG.lock
LOGFILE=/var/log/$PROG.log

# Redirect all output to log file
exec >>"$LOGFILE" 2>&1

echo $(date +%Y-%m-%d\ %T) START $PROG

if [ -f "$LOCKFILE" ]
then
        echo "*** LOCKFILE $LOCKFILE FOUND - ABORTING ***"
        exit 1
else
        touch "$LOCKFILE"
fi

/usr/bin/rsync -rltDvau --delete --exclude=php/twig/ --exclude=css/ --exclude=js/ --exclude=js/optimized/ --exclude=synonyms/ --exclude=styles/ --exclude=404_pages/ --exclude=403_pages/ "$SRC/" "$DST/"

rm -f "$LOCKFILE"

echo $(date +%Y-%m-%d\ %T) END   $PROG
exit 0
```


```
-r : recurse into directories
l : opy symlinks as symlinks
t : preserve modification times
D : same as --devices --specials
v : verbose 
a : preserve almost everything
u : skip files that are newer on the receiver
```


```

Este script é um script de shell em Bash que realiza uma operação de sincronização de arquivos usando o comando rsync. Vamos passar por cada parte do script:

#!/bin/bash: Indica que o script deve ser interpretado pelo Bash.

PROG=$(basename $0): Define uma variável chamada PROG que armazena o nome do script sem o caminho do diretório.

SRC=rafael@192.168.2.61:/dados/: Define o caminho de origem dos arquivos a serem sincronizados.

DST=/dados: Define o destino para onde os arquivos serão sincronizados.

LOCKFILE=/tmp/$PROG.lock: Define o caminho para um arquivo de trava que é usado para evitar que o script seja executado simultaneamente por várias instâncias.

LOGFILE=/var/log/$PROG.log: Define o caminho para o arquivo de log onde todas as saídas do script serão redirecionadas.

exec >>"$LOGFILE" 2>&1: Redireciona a saída padrão (stdout) e a saída de erro padrão (stderr) para o arquivo de log especificado.

echo $(date +%Y-%m-%d\ %T) START $PROG: Registra no log a data e hora de início do script.

if [ -f "$LOCKFILE" ]: Verifica se o arquivo de trava já existe.

Se o arquivo de trava existir, o script exibe uma mensagem de erro no log indicando que o arquivo de trava foi encontrado e encerra a execução com o código de saída 1.

Se o arquivo de trava não existir, o script cria o arquivo de trava usando touch "$LOCKFILE".

/usr/bin/rsync -rltDvau --delete --exclude=php/twig/ --exclude=css/ --exclude=js/ --exclude=js/optimized/ --exclude=synonyms/ --exclude=styles/ --exclude=404_pages/ --exclude=403_pages/ "$SRC/" "$DST/": Executa o comando rsync para sincronizar os arquivos da origem para o destino. O comando rsync realiza a sincronização de forma eficiente, transferindo apenas os arquivos que foram modificados.

-rltDvau: Opções do rsync para especificar o modo de transferência, preservar links simbólicos, preservar a hora da última modificação, excluir arquivos no destino que não existem na origem, excluir arquivos no destino que estão excluídos na origem, e mostrar saída detalhada.

--delete: Remove arquivos no destino que não existem na origem.

--exclude=...: Exclui determinados diretórios da sincronização.

"$SRC/" "$DST/": Especifica a origem e o destino da sincronização.

rm -f "$LOCKFILE": Remove o arquivo de trava após a conclusão da sincronização.

echo $(date +%Y-%m-%d\ %T) END $PROG: Registra no log a data e hora de término do script.

exit 0: Indica que o script foi concluído com sucesso, e o código de saída é 0.
```



