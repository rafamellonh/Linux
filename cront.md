### Valida se o serviço esta online :

```systemctl status crond.service```

```crontab -l``` - Lista as tarefas agendadas

```crontab -e``` - Abre o editor para criar/editar/deletar tarefas

Minutos(0-59)   -   Horas(0-23)  -   Dia do mês(1-31)   -  mês(1-12)   -  dia da semana(0-7)   -   comando

Nesse comando ele vai ser executado a cada duas horas nos minutos 10/20/30 todos os dias

10/20/30 */2 * * * 
