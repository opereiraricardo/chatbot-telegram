# Weather Telegram Bot com n8n

Bot de Telegram desenvolvido utilizando **n8n** que permite consultar a
temperatura atual de uma cidade utilizando a API **OpenWeatherMap**.

O usuário envia o nome da cidade para o bot e recebe como resposta
informações climáticas atualizadas.

------------------------------------------------------------------------

# Visão Geral

O workflow implementa um fluxo automatizado que:

1.  Recebe mensagens enviadas ao bot no Telegram.
2.  Valida se o usuário informou uma cidade.
3.  Consulta a API OpenWeatherMap.
4.  Processa os dados retornados.
5.  Retorna ao usuário a temperatura atual e outras informações
    climáticas.

------------------------------------------------------------------------

# Tecnologias Utilizadas

-   **n8n** -- ferramenta de automação de workflows
-   **Telegram Bot API** -- comunicação com o usuário
-   **OpenWeatherMap API** -- obtenção de dados climáticos
-   **Docker / VPS (opcional)** -- execução do n8n

------------------------------------------------------------------------

# Estrutura do Workflow

## 1. Telegram Trigger

Responsável por capturar mensagens enviadas ao bot.

Entrada esperada:

    Nome da cidade

Exemplo:

    Belo Horizonte

------------------------------------------------------------------------

## 2. Define Parâmetros (Set Node)

Campos definidos:

  Campo             Descrição
  ----------------- -----------------------------
  message.chat.id   ID do chat do Telegram
  message.text      cidade enviada pelo usuário
  appid             chave da API OpenWeather
  lang              idioma da resposta
  units             unidade de temperatura

Também é aplicado automaticamente `,BR` ao final da cidade.

------------------------------------------------------------------------

## 3. Validação da Cidade

Node **IF** que verifica se o usuário informou uma cidade.

Condição:

    message.text not empty

Caso esteja vazio, o bot retorna uma mensagem de erro.

------------------------------------------------------------------------

## 4. HTTP Request -- OpenWeatherMap

Endpoint utilizado:

    https://api.openweathermap.org/data/2.5/weather

Parâmetros enviados:

  Parâmetro   Valor
  ----------- ------------------
  q           cidade informada
  appid       chave da API
  lang        pt_br
  units       metric

------------------------------------------------------------------------

## 5. Validação da Resposta da API

Condições verificadas:

    cod == 200
    main.temp exists

------------------------------------------------------------------------

## 6. Resposta ao Usuário

Exemplo de resposta:

    🌤️ A temperatura em Belo Horizonte é de 25°C
    ☁️ Condição: nublado
    💧 Umidade: 83%
    💨 Vento: 1.3 m/s

------------------------------------------------------------------------

## Mensagem de erro

    ❌ Cidade não encontrada. Use o formato Cidade,UF,BR (ex.: São Paulo,SP,BR).

------------------------------------------------------------------------

# Variáveis de Ambiente

    OPENWEATHER_API_KEY

Exemplo:

    OPENWEATHER_API_KEY=xxxxxxxxxxxxxxxx

------------------------------------------------------------------------

# Como Executar

1.  Criar um bot no Telegram usando **BotFather**
2.  Configurar credenciais do Telegram no n8n
3.  Criar conta em https://openweathermap.org
4.  Gerar API Key
5.  Definir variável `OPENWEATHER_API_KEY`
6.  Importar o workflow JSON no n8n
7.  Ativar o workflow

------------------------------------------------------------------------

# Exemplo de Uso

Entrada:

    Porto Alegre

Resposta:

    🌤️ A temperatura em Porto Alegre é de 24°C
    ☁️ Condição: parcialmente nublado
    💧 Umidade: 75%
    💨 Vento: 2.1 m/s

------------------------------------------------------------------------

# Melhorias Futuras

-   suporte a cidades internacionais
-   cache de requisições
-   previsão de vários dias
-   comandos no Telegram
-   emojis dinâmicos conforme clima

------------------------------------------------------------------------

# Autor

**Ricardo Pereira**\
Especialista em Dados, Automação e IA

LinkedIn:\
https://www.linkedin.com/in/ricardoopereira/
