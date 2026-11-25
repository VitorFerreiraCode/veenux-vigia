# Vigia do Veenux

Workflow do GitHub Actions que confere o `/api/health` do Veenux a cada 10 minutos, **fora do Railway**.
Serve para o caso em que o serviço inteiro cai (ex.: suspenso por falta de pagamento) e o bot não consegue avisar.

- Caiu (3 tentativas falharam): abre a issue "Veenux fora do ar" (o GitHub manda e-mail) e posta no seu canal privado do Discord.
- Voltou: fecha a issue e posta quanto tempo ficou fora. O próprio bot, ao subir, trata carrinhos e pagamentos e manda o resumo da queda.
- Só avisa o dono. Manutenção planejada não dispara: o `/api/health` continua respondendo 200.

## Como instalar

1. Crie um repositório **público** só com esta pasta (`.github/workflows/vigia.yml` e este README). Público porque o
   Actions é ilimitado nele; num privado, rodar a cada 10 min passa da cota grátis. Nada sensível fica no código.
2. No Discord, no seu canal privado: Editar canal → Integrações → Webhooks → Novo webhook → Copiar URL.
3. No repositório: Settings → Secrets and variables → Actions → New repository secret:
   - `VEENUX_HEALTH_URL`: `https://SEU-DOMINIO/api/health`
   - `DISCORD_WEBHOOK_URL`: a URL do passo 2
   - `DISCORD_MENTION_ID` (opcional): seu ID de usuário do Discord, para ser mencionado
4. Actions → Vigia Veenux → Run workflow, para testar.

## Limites

- O agendamento do GitHub pode atrasar alguns minutos em horários de pico.
- O GitHub desativa workflows agendados após 60 dias sem atividade no repositório; o último passo reativa o
  workflow a cada rodada. Se mesmo assim ele parar, há um aviso por e-mail do GitHub e basta reativar em Actions.
