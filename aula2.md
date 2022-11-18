# Aula 02 (Back-end Completo)

   -> Implementar o back end
   -> Acesso a banco de dados
   -> Criar endpoints da API REST
   -> Integração com SMS
   -> Implantação na nuvem



## Anotações

   ### Instalações
      -> Postman (API development)
      -> Heroku CLI (cloud host)

   

   ### Configuração de segurança
      -> back-end vai estar hospedado no Heroku, porém o front-end vai estar hospedado em outro lugar
      -> para uma aplicação que está hospedada em um lugar, acessar outra em outra hospedagem, temos que liberar o acesso
         -> CORS (Cross-Origin Resource Sharing)


   
   ### Banco de Dados
      -> Fazer mapeamento objeto-relacional (JPA)
         -> Transformar uma classe Java em uma tabela no Banco de Dados

         -> Usamos notation
            -> Código especial que colocamos perto de uma classe, atributo, elemento da classe, para que seja feito um pré-processamento na hora de compilar o projeto
            
            -> "@" 
               -> "@Entity" = prepara uma classe para que seja equivalente a uma tabela no banco de dados 
               -> "@Table" = pode receber um atributo "name", que serve para renomear a entidade no bd
               -> "@Id" = o atributo será único (PK)
               -> "@GeneratedValue" = autoincremento do Id, para que após cada registro o Id receba +1

      
      -> Configurar dados de conexão do Banco de Dados H2

         ```java
         // endereço do banco de dados
         spring.datasource.url=jdbc:h2:mem:testdb
         // usuario
         spring.datasource.username=sa
         // senha
         spring.datasource.password=

         // aplicativo web para acessar no navegador
         spring.h2.console.enabled=true
         spring.h2.console.path=/h2-console

         // aparecer cada consulta no banco de dados no terminal em SQL
         spring.jpa.show-sql=true
         spring.jpa.properties.hibernate.format_sql=true
         ```

         -> para acessar a aplicação web do bd H2 = localhost:8080/h2-console
            -> JDBC URL = jdbc:h2:mem:testdb
            -> usernamer = sa
            -> password = ""

      
      -> Seed do Banco de Dados
         -> Criar um arquivo "import.sql" na pasta "resources"
         -> Colocar os comandos de inserção de dados em SQL no arquivo 
   


   ### Primeiro teste de endpoint da API REST

      -> Fluxo padrão
         -> Repository -> Service -> Controller
            -> Repository = CRUD
            
            -> Service = função do sistema através do Repository
            
            -> Controller = disponibiliza os dados pro front-end através do Service


      -> Criar repository
         -> Componentes que são responsáveis por manipular o banco de dados (consulta, deletar, salvar, etc)
      
         -> Criação no Spring:
            -> new interface
            -> extends JpaRepository<nomeDaEntidade, tipoDoId> | (<Sale, Long>)

         -> Com isso o Spring já cria um componente que faz um CRUD no banco de dados
      

      -> Criar service
         -> Responsável por implementar as operações de negócio (ex: buscar as vendas)
         -> Uma funcionalidade do sistema

         -> Criação no Spring:
            -> new class que recebe "@Service" notation
            -> private "nomeRepository" repository que recebe "@Autowired" notation

            -> Por querermos uma lista de vendas
               -> public List<nomeEntidade> findSales()
               -> return repository.findAll()
      

      -> Criar controller
         -> Implementa as APIs
         -> Responsável por disponibilizar os endpoints que o front-end precisa para acessar o back-end

         -> Criação no Spring:
            -> new class que recebe @RestController + @RequestMapping(value = "/endereço")

            -> private "nomeService" service que recebe @Autowired

            -> método para disponibilizar para o front-end
               -> Recebe @GetMapping (método HTTP)
               -> public List<Sale> findSales()
               -> return service.findSales()



   ### Configurar consulta por data
      -> A aplicação está retornando TODAS as vendas, por isso devemos filtrar 
      -> Trocar os tipos de retornos de "List" por um objeto especial do Spring "Page" (mostra as primeiras 20 vendas)
      -> Passar o argumento "pageable" nas funcionalidades


      -> Resultado páginado
         -> Controller
            -> Adicionar os parâmetros minDate e maxDate nas funções
            -> Adicionar a notation "@RequestParam" com parâmetros de "value = minDate" e "defaultValue = ''"

         -> Service
            -> Adicionar os parâmetros minDate e maxDate nas funções
            -> transformar os valores de String para LocalDate

         -> Repository
            -> Criar uma busca customizada que recebe como parâmetro min e max já transformados em LocalDate
               -> ("SELECT obj FROM Sale obj WHERE obj.date BETWEEN :min AND :max ORDER BY obj.amount DESC")
               -> "obj" = vendas
               -> pegue as vendas da tabela "Sale" onde a data das vendas esteja entre a data min e max, ordenados decrescente pelo número de vendas


      -> Consulta com parâmetros na URL
         -> "?" = diz que virá um parâmetro a seguir
         -> "&" = divide os parâmetros

         -> Ex: localhost:8080/sales?minDate=2022-01-01&maxDate=2022-03-31
         -> consulta por vendas entre o começo de janeiro até o fim de março
      

      -> Tratar erro de URL sem parâmetros
         -> Caso não for informado nenhum dos parâmetros, minDate = 1 ano atrás e maxDate = hoje

         -> Criar uma nova variável "today", onde recebe o dia de hoje, conforme a data do sistema
            -> LocalDate today = LocalDate.ofInstant(Instant.now(), ZoneId.systemDefault());
         
         -> Utilizamos um método do objeto LocalDate ".minusDays", onde ele subtrai dias da data inserida
            -> today.minusDays(365) = 1 ano atrás

         -> Condição terciária
            -> "?" = "=="
            -> ":" = "else"

            -> Caso os parâmetros recebam uma string vazia, minDate recebe 1 ano atrás e maxDate recebe hoje, senão roda normal
               LocalDate min = minDate.equals("") ? today.minusDays(365) : LocalDate.parse(minDate);
		         LocalDate max = maxDate.equals("") ? today: LocalDate.parse(maxDate);

   

   ### Envio de SMS
      -> Twilio
         -> Disponibiliza várias APIs
         -> Cadastrar-se e fazer a comunicação entre ele e a nossa aplicação
      
         -> Adicionar dependências do Twilio no pom.xml

         -> Definir variáveis de ambiente no application.properties
            -> Por se tratar de dados sensíveis, como ID do twilio, número, etc, criamos variáveis que serão configuradas 
               no ambiente que a aplicação estará rodando
            
         -> Criar classe SmsService que vai enviar de fato um SMS
            -> Utilizamos então as variáveis de ambiente com a notation "@Value" para receber os dados da aplicação
            -> A função de enviar o SMS foi tirada da documentação do Twilio


      -> Criar um endpoint no Controller para receber essa função de SMS
         -> Declarar a classe SmsService

         -> Criar o endpoint por meio da notation "@GetMapping" com "/notification" como parâmetro
            -> Criar a função notifySms que retorna a função que envia o SMS


      -> Definir as variáveis de ambiente na IDE, conforme foram declaradas

      
      -> Lógica de programação para enviar a mensagem do SMS
         
         ```java
         // pegamos o Id da venda
         Sale sale = saleRepository.findById(saleId).get();

         // nome do vendedor
         String name = sale.getSellerName();
         // data da venda (separado por "mês" + "/" + "ano")
         String date = sale.getDate().getMonthValue() + "/" + sale.getDate().getYear();
         // quantia da venda (formatado 2 casas após a virgula)
         String amount = String.format("%.2f", sale.getAmount());

         String msg = "O vendedor " + name + " foi destaque em " + date + " com um total de R$ " + amount; 
         ```
   


   ### Implantação no Heroku
      -> Implantar as variáveis de ambiente no Heroku
      
      -> Criar arquivo "system.properties"
         -> java.runtime.version=17

      -> Procedimento de conexão do heroku com git
         -> heroku -v
         -> heroku login
         -> heroku git:remote -a <nome-do-app>
         -> git remote -v
         -> git subtree push --prefix backend heroku main