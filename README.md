# Aplicação Web - React + SpringBoot

  Este projeto foi desenvolvido durante a Semana Spring React dos dias (14/11/22 - 18/11/22) elaborado pelo professor Nélio Alves 

  Construimos uma aplicação Web de consulta de vendas, com a opção de filtrar por mês/ano e notificar um SMS ao clicar no botão ao final da lista com os dados da venda selecionada
  
 <br>

## Tecnologias utilizadas

  ### Front-end

  - HTML/CSS + JS/TS com ReactJS
  
### Back-end
  
  - Java + SQL com SpringBoot
  - H2 como DB

### API SMS

  - Twilio
 
### Implantação na Cloud

  - Heroku
  - Netlify
  
 <br>

## Instalação

  ### Front-end

  - É necessário ter instalado NodeJS

  Para instalar as depêndencias do projeto, digite no terminal da pasta "frontend":
  
    npm install


  <br>
        
  ### Back-end

  - É necessário ter instalado instalado STS (Spring Tool) ou alguma outra IDE Java

        -> Abrir a pasta que contém o projeto
        -> Selecionar a opção "Import projects"
        -> "Import Maven Projects"
        -> Selecionar a pasta do projeto como Root Directory
        -> Selecionar o arquivo "pom.xml"
        
  - Caso não apareça os arquivos do projeto no lado esquerdo da IDE, procure na lupa por "Project Explorer"
        
 <br>
        
  ### Configurar Variáveis de Ambiente
  
    -> Botão direito no projeto -> Properties
    -> Run/Debug Settings
    -> Duplo clique no backend
    -> Aba Environment -> Add
             
  - Você precisa criar uma conta na Twilio para ter acesso aos valores das variáveis
  
     - TWILIO_KEY = chave única do usuário
     - TWILIO_PHONE_FROM = número do Twilio
     - TWILIO_PHONE_TO = seu número de telefone
     - TWILIO_SID = chave do Twilio
     
     
<br>

## Rodar a aplicação

   - Na pasta "frontend", digite no terminal
   
          npm run dev
   
   - No terminal será disponibilizado o link do front-end, ctrl + botão direito para abrir
   
 <br>
     
   - No SpringBoot, abra o Boot Dashboard. 
   
   - Em "local", botão direito no "backend" e clique em (Re)start para iniciar o backend
   
   - O SpringBoot tem como padrão a porta :8080, portanto suas rotas:
     
                -> './sales' = mostra as 20 primeiras vendas de maior valor

                -> './sales/{id}/notification = manda o SMS com os dados da venda do "id"

                -> './h2-console' = para acessar o banco de dados
                   -> JDBC URL = jdbc:h2:mem:testdb
                   -> username = "sa"
                   -> password = ""

## Aplicação pronta 😁👍