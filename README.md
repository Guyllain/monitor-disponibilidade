\# Monitor de Disponibilidade com Alerta no Telegram (n8n)

Automação em n8n que testa a cada 5 minutos se o serviços de rede estão no ar e avisa se houver alguma falha

\## 1. Sobre o Projeto e Problema Resolvido

Descobrir tarde que um serviço caiu gera perda de tempo. Este projeto verifica tais serviços automaticamente e envia um alerta para o celular através do Telegram, sem precisar ficar checando manualmente.

\## 2. Arquitetura e Fluxo de Funcionamento

```
Schedule Trigger (5 min)
    → Edit Fields (lista JSON de alvos)
    → Split Out (1 item por alvo)
    → HTTP Request (timeout 10s, Never Error, Saída de Erro Ativa)
    → IF (status 200-299)
        ├─ falso > Telegram (alerta)
        └─ verdadeiro > FIM
HTTP Request (saída de erro: timeout/host fora) > Telegram (alerta)

```

Gatilho > Processamento (JSON, filtragem com IF) > Saída (mensagem no Telegram).

!\[Fluxo no n8n](prints/fluxo-n8n.png)

\## 3. Tecnologias e APIs Utilizadas

n8n (self-hosted), Docker, Telegram Bot API, HTTP/HTTPS, JSON.

\## 4. Pré-requisitos e Como Executar.

1. Instale Docker e Git.
2. Crie um bot no Telegram com o @BotFather e guarde o token.
3. Clone o repositório e entre na pasta.
4. Copie '.env.example' para '.env'.
5. Execute 'Docker composse up -d' e abra 'http://localhost:5678'.
6. No n8n, crie a credencial \*\*Telegram API\*\* com o token do bot.
7. Importe 'monitor-disponibilidade.json'
8. No nó Telegram, selecione a credencial criada.
9. Ative o workflow.

Os alvos monitorados ficam no nó \*\*Edit Fields\*\*

\## 5. Demonstração

!\[Sinal de Alerta no n8n](prints/alerta-n8n.png)

!\[Alerta de Para no Telegram](prints/alerta-telegram.png)

\## 6. Melhorias Futuras (Roadmap)

* Medir latência e alertar acima de X ms.
* Avisar também quando o serviço voltar.
* Gravar histórico de disponibilidade em planilha.

\## Segurança

O token do bot fica no cofre de credenciais do n8n (não vai no JSON exportado). O chat id fica no '.env', que está no '.gitignore'.

