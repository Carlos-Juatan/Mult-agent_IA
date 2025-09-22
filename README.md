# Mult-agent_IA

Eu desenvolvi esse Mult-Agent para uma loja de produtos relacionados a surf e praia ( Roupas, parafina, itens para surf, etc ) e que também oferece aulas de surf

O objetivo desse mult-agent é classificar o que o lead deseja e direciona-lo para um chat ia especializado no que o lead esta buscando

## FUNÇÕES PRINCIPAIS DO MULT-AGENT

### CLASIFICADOR
  - Agente responsável por classificar o que o lead deseja, ele não responde diretamente o usuário, somente encaminha para uma ferramenta de chat especializado usando o **MCP Server**
  - Agente especializado em Boas vindas para iniciar a conversa com novos leads
  - Agente especializado em Vendas de produtos da loja
  - Agente especializado em marcação de horários para aulas de surf ( Marcar, Desmarcar e Consultar Horários Disponíveis)
  - Agente especializado em tirar dúvidas ( RAG, FAQ, Termos E Serviços )


### WORFLOW PRINCIPAL
  - Recebe mensagens via webhook do **chatwoot** permitindo se conectar com diversos meios de comunicação ( Whatsapp, Instagram, Facebook, Telegram, etc...)
  - Separa o tipo de mensagens ( Texto, Audio, Outros )
    - Texto é enviado direto para continuar o fluxo
    - Audio é enviado para outro workflow para ser convertido em texto
    - Outros não faz nada ( Imagens, Videos, Documentos)
  - Organizador de dados e variáveis ( Seleciona os dados inportantes e defini variáveis para serem usadas durante o fluxo do workflow)
  - Função de deBounce ( Aguarda um breve momento para juntar mensagens quebradas e espaçadas do lead )
  - Função de **reset-ia** para testes internos permitindo apagar os dados do lead e chat para test
  - Agente IA Classificador que analiza os dados e usa memória compartilhada do histórico da conversão para direcionar para um agente ia especializado
  - MCP Server que separa e classifica as diferentes ferramentas que podem ser usadas pelos agentes para que o contexto seja reduzido e consuma menos tokens e processamento
  - Função de rotulação do lead que permite coletar dados do lead do histórico da conversa e classificar o posicionamento do lead e seu momento atual ( Novo, Marcou_AUla, Comprou_Algo, etc...) e armazenar esses dados no banco de dados (supabase) e rotular no chatwoot as informações coletadas ( nome, telefone, e-mail, etc... )

![main](https://github.com/Carlos-Juatan/Mult-agent_IA/blob/main/images/1-main.png?raw=true)


### Sistema de transcrever audio para texto
Recebe um link de audio e converte o audio para texto.

![audio-para-texto](https://github.com/Carlos-Juatan/Mult-agent_IA/blob/main/images/2-traslator.png?raw=true)


### MCP Server
Sistema de Servidor MCP para otimizar o uso do **contexto** dos agentes IA para diminuir gastos com **tokens**, organiza melhor o projeto centralizando os sub-workflows em um só lugar e facilita a manutenção e expansão do sistema.

![mcp-server](https://github.com/Carlos-Juatan/Mult-agent_IA/blob/main/images/3-mcp-server.png?raw=true)



### CHAT DE ATENDIMENTO INICIAL 
Sua especialização é de iniciar a conversa e direcionar o lead para o que ele busca

![chat-atendimento](https://github.com/Carlos-Juatan/Mult-agent_IA/blob/main/images/4-chat-inicial.png?raw=true)


### CHAT ESPECIALIZADO EM VENDAS
Sua especialização é de vender e informar sobre os produtos da loja (física e online) e entregas

![chat-vendas](https://github.com/Carlos-Juatan/Mult-agent_IA/blob/main/images/5-chat-vendas.png?raw=true)

#### FERRAMENTAS PARA O CHAT DE VENDAS
  - pagamentos: ( nesse workflow ainda não tem )
  - entregas: Usa o cep do lead para localizar o estado e busca esse estado no banco de dados para ter o valor do frete
  - produtos: Usa uma banco de dados misto (Vetorial e normal)
    - Vetorial para realizar a busca dos produtos no banco de dados
    - Normal para consultar os modelos, cores, preços, imagens, e quantidade no estoque ( que são dados que mudam dinâmicamente )

![chat-vendas-tools](https://github.com/Carlos-Juatan/Mult-agent_IA/blob/main/images/6-store-tools.png?raw=true)


### CHAT ESPECIALIZADO EM AGENDAMENTOS
Sua especialização é marcar um horário no calendário do google para as aulas de surf

![chat-agendamentos](https://github.com/Carlos-Juatan/Mult-agent_IA/blob/main/images/7-chat-canlendario.png?raw=true)


#### FERRAMENTAS PARA O CHAT DE AGENDAMENTOS
  - buscar horários disponíveis no calendário do google
  - marcar um horário no calendário do google e salvar no supabase também
  - cancelar um agendaemnto e remover do banco de dados do supabase

![chat-agendamentos-tools](https://github.com/Carlos-Juatan/Mult-agent_IA/blob/main/images/8-calendario-tools.png?raw=true)


### CHAT ESPECIALIZADO EM TIRAR DÚVIDAS
Sua especialização é tirar dúvidas dos lead usando um banco de dados vetorial para buscar as FAQs (perguntas frequentes), os termos e serviços registrados no rag

![chat-dúvidas](https://github.com/Carlos-Juatan/Mult-agent_IA/blob/main/images/9-chat-rag.png?raw=true)

#### FERRAMENTAS PARA O CHAT DE DÚVIDAS
  - Realiza uma busca vetorial para tirar a dúvida do lead
  - Caso não encontre uma resposta informa ao lead que a dúvida dele foi encaminhada e será respondida assim que possível, a dúvida não encontrada é registrada em um banco de dados normal no supabase para analize, assim como uma possível resposta gerada por ia que pode ou não ser aprovada pelo gestor, assim que a resposta estiver revisada e pronta para entrar no banco de dados ela será adicionado no banco vetorial

![chat-duvidas-tools](https://github.com/Carlos-Juatan/Mult-agent_IA/blob/main/images/10-rag-tools.png?raw=true)


### Banco vetorial
O banco vetorial, tanto para os FAQs como para novos Itens da loja, conta com um workflow que roda todos os dias a meia noite que verifica quais dados estão disponíveis para entrar no banco de dados vetorial

![banco-usual-rag](https://github.com/Carlos-Juatan/Mult-agent_IA/blob/main/images/11-banco-faqs.png?raw=true)
![banco-vetorial-time-rag](https://github.com/Carlos-Juatan/Mult-agent_IA/blob/main/images/12-faqs-time.png?raw=true)
![banco-vetorial-time-store](https://github.com/Carlos-Juatan/Mult-agent_IA/blob/main/images/13-store-times.png?raw=true)

> **PS**: estou usando o LLM do google por motivos de testes eu sei que ele é meio bugado e que é melhor usa outro como o chatgpt

