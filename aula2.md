# Aula 02 (Back-end Completo)

   -> Implementar o back end
   -> Acesso a banco de dados
   -> Criar endpoints da API REST
   -> Integração com SMS
   -> Implantação na nuvem



## Anotações

   - Instalações
      -> Postman (API development)
      -> Heroku CLI (cloud host)

   

   - Configuração de segurança
      -> back-end vai estar hospedado no Heroku, porém o front-end vai estar hospedado em outro lugar
      -> para uma aplicação que está hospedada em um lugar, acessar outra em outra hospedagem, temos que liberar o acesso
         -> CORS (Cross-Origin Resource Sharing)


   
   - Banco de Dados
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