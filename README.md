# Telegram-Data-Pipeline-
Telegram data pipeline, a project from the Data Analysis course at EBAC.

## Contexto:

Chatbots como os do Telegram geram uma grande quantidade de dados, registrando cada interação dos usuários em tempo real.
Para transformar essas informações em insights, foi criada uma pipeline completa na AWS que converte dados transacionais em dados analíticos, já tratados e prontos para exploração no Athena.
Este projeto mostra exatamente como essa jornada acontece: Do Bot criado, da mensagem enviada no grupo Telegram, da captação e ingestão das mensagens até uma base estruturada para análise.

![Fluxograma Pipeline de Dados](https://raw.githubusercontent.com/nathalia-de-sa/Telegram-Data-Pipeline-/refs/heads/main/Fluxo%20de%20Processamento%20de%20Dados.png)

## Estrutura Arquitetura:

- Sistema transacional: 
  - Bot criado no Telegram, apenas para a leitura das mensagens.
  - Utilizando a API fornecida pelo Telegram para possibilitar as conexões
  - Uso do método getUpdates para capturar mensagens do grupo no notebook Python, e poder transformá-las em arquivo JSON.

- Sistema analítico (AWS):

  - Ingestão: API Gateway + Lambda recebem os dados e armazenam no S3.
 
  - ETL: uma segunda função Lambda consome esses dados brutos, transforma e salva a nova versão processada no S3.

  - Criação de automatização: Utilizando o Event Bridge da AWS para dar sequencia na ingestão dos dados diariamente de forma automatizada e armazenar no S3 de forma particionada

  - Apresentação: uso do AWS Athena para consulta SQL dos dados já modelados.

 ##  Análise Exploratória de Dados
 Dicionário de dados:

 | Campo               | Tipo    | Descrição                                                                                      |
| ------------------- | ------- | ---------------------------------------------------------------------------------------------- |
| **message_id**      | bigint  | Identificador único da mensagem dentro do chat. Permite rastrear e diferenciar cada envio.     |
| **user_id**         | bigint  | ID único do usuário que enviou a mensagem. Serve para identificar o autor e analisar padrões.  |
| **user_is_bot**     | boolean | Indica se o autor da mensagem é um bot (`true`) ou um usuário humano (`false`).                |
| **user_first_name** | string  | Primeiro nome do usuário que enviou a mensagem. Útil para análises descritivas e agrupamentos. |
| **chat_id**         | bigint  | Identificador único do chat ou grupo no Telegram. Permite diferenciar conversas ou canais.     |
| **chat_type**       | string  | Tipo de chat onde a mensagem foi enviada (ex.: `private`, `group`, `supergroup`).              |
| **text**            | string  | Conteúdo textual da mensagem. Pode incluir comandos, perguntas e interações do usuário.        |
| **date**            | bigint  | Timestamp Unix da mensagem (segundos desde 01/01/1970). Usado para análises temporais.         |


## Melhorias:

Melhoria mapeada para implantação em roadmap:
  - Ingestão de outros formatos além do texto, como imagens e aúdios compartilhados no grupo.
