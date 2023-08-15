# Aula 01 (Front-end Estático)

   -> Criação do projeto no Github
   -> Layout estático
   -> Componentes React
   -> DatePicker
   -> React Hook useState



## Aplicação

Front-end <---- requisições web (HTTP/JSON) ----> Back-end + Banco de Dados (server)



### Front-end

- Linguagens
   -> HTML/CSS + JavaScript/TypeScript

- Framework
   -> ReactJS



### Back-end

- Linguagens
   -> Java + SQL

- Framework
   -> Spring Boot

- DB
   -> H2



## Anotações

   ### Instalações
      -> Instalar STS (IDE Java SpringBoot)
         -> + Maven (baixa as dependências do projeto (npm))

      -> Usamos o framework Vite para nos ajudar a criar o projeto em React

      -> Usamos o framework Spring Initializr para nos ajudar a criar o projeto em Spring



   ### React (Components)
      -> React faz uma renderização do HTML a partir de uma ID ('root')

      -> Componente visual do React = função JavaScript

      -> Componente = pedaço de código reaproveitável
         -> Um sistema é um conjunto de componentes trabalhando em conjunto para atingir um objetivo

      -> Cada componente é armazenado em um arquivo .tsx (typescript + react)

      -> Arquivo tsx
         -> função javascript que retorna tags HTML

      -> Para por mais de uma tag HTML em um componente temos que botar elas dentro de um fragment = "<> </>"

      -> Não podemos usar "class" do HTML no React por ser também ser do JavaScript, portanto usamos "className"
      
      -> Para por uma expressão no React, devemos usar entre "{}". Ex: src={icon} 

      -> Para por uma expressão no React, devemos usar entre "{}". Ex: src={icon} 

      -> A estrutura principal da página (main) nós não transformamos em componente, botamos ela direto no "App"

      -> Para importar da mesma pasta = './'
      -> Pasta anterior '../'



   ### DatePicker
      -> Componente que renderiza um calendário no qual o usuário pode escolher uma data

      -> https://github.com/Hacker0x01/react-datepicker



   ### React Hook "useState"
      -> Hook são recursos do React que estão atrelados ao ciclo de vida do componente

      -> Serve para mantermos o estado das datas (manter a alteração do usuário)