### Valida se o serviço esta online :

```systemctl status crond.service```

```crontab -l``` - Lista as tarefas agendadas

```crontab -e``` - Abre o editor para criar/editar/deletar tarefas

```crontab -r``` - remove as tarefas agendadas CUIDADO

Minutos(0-59)   -   Horas(0-23)  -   Dia do mês(1-31)   -  mês(1-12)   -  dia da semana(0-7)   -   comando

Nesse comando ele vai ser executado a cada duas horas nos minutos 10/20/30 todos os dias

10/20/30 */2 * * * 

Aqui sera toda segunda as 9:37 da manha : 

37 9 * * 1

Aqui executa todas as terças das 9 as 17 

0 9-17 * * * 2

Dois exemplos de execuçao, um a cada minuto e outro a cada 2 minutos
 * * * * * echo $(date) >> /home/rafael/crontTeste.txt
          
*/2 * * * * echo $(date) >> /home/rafael/crontTeste2.txt



```

# For details see man 4 crontabs

# Example of job definition:
# .---------------- minute (0 - 59)
# |  .------------- hour (0 - 23)
# |  |  .---------- day of month (1 - 31)
# |  |  |  .------- month (1 - 12) OR jan,feb,mar,apr ...
# |  |  |  |  .---- day of week (0 - 6) (Sunday=0 or 7) OR sun,mon,tue,wed,thu,fri,sat
# |  |  |  |  |
# *  *  *  *  * user-name  command to be executed

```
