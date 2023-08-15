# Aula 03 (Integração e Além)

   -> Integrar back-end e front-end
   -> Implantar o front-end



## Anotações

   ### Primeira requisição com Axios e useEffect
      -> `npm install axios@0.27.2`
      
      -> useEffect = ReactHook (programar de uma forma atrelada ao ciclo de vida do componente)
         -> Executa alguma coisa quando o componente é montado, ou quando algum dado que você informar alterar
            -> Controlar, "quando mudar este dado, execute esta função"
         
         -> Recebe uma função e uma lista como parâmetro
            -> 

   

   ### Listagem de vendas
      -> Definição da BASE_URL:
         -> Criar arquivo "request.ts" na pasta "utils"
         -> `export const BASE_URL = import.meta.env.VITE_BACKEND_URL ?? "http://localhost:8080";`
         -> Pega o valor da variavel de ambiente, se não existir variavel de ambiente com este nome, por padrão utilize o localhost:8080
         -> Ou seja, estamos atribuindo o endpoint a uma variável, para sempre que precisarmos mandar uma requisição pro back-end, 
            só precisamos usar a variável, e não o endereço inteiro
         

      -> Armazenar a lista de vendas no useState
         -> Exportar os tipos de dados no arquivo "sales.ts" na pasta "models"
            -> export type Sale = { id: 23, sellerName: Igor, ...}

         -> [sales, setSales] recebe o useState (nome do dado, nome da função que altera o dado)
         
         -> Precisamos tipar o useState
            -> <Sale[]> (lista de vendas) + ([]) valor inicial (lista vazia)

         -> Quando buscar do back-end as vendas, chamamos a função setSales para atualizar o useState com o valor da API
         
         -> No final:
            -> `useEffect(() => { axios.get(`${BASE_URL}/sales`).then(response => {setSales(response.data.content);});  }, []);`
      

      -> Renderizar a lista de vendas pelo React
         -> `{sales.map((sale) => { return( "código html" )}`
         -> Utilizamos a função .map na lista "sales" que foi preenchida pelo useState
         -> Passamos como parâmetro a venda "sale"
         -> Retorna todo o código html que tinhamos feito antes, porém agora com os dados dinâmicos do back-end

         -> No final:
         ```tsx
         {sales.map((sale) => {
            return (
               <tr key={sale.id}>
               <td className="show992">{sale.id}</td>
               <td className="show576">{new Date(sale.date).toLocaleDateString()}</td>
               <td>{sale.sellerName}</td>
               <td className="show992">{sale.visited}</td>
               <td className="show992">{sale.deals}</td>
               <td>R$ {sale.amount.toFixed(2)}</td>
               <td>
                  <div className="dsmeta-red-btn-container">
                     <NotificationButton />
                  </div>
               </td>
               </tr>
            )
         })}
         ```

         -> Passamos entre "{}" os dados do objeto sale | sale.id, sale.date, ...

   

   ### Passando as datas como argumento
      -> Criar variáveis dmin e dmax para recortar as datas devolvidas pelo back-end

      -> Igualar minDate e maxDate com as variáveis dmin e dmax como argumento na URL, para deixar dinâmico a aplicação
      
      -> Colocar minDate e maxDate na lista de dependências do useEffect
         -> Para que sempre que o minDate ou maxDate mudar, executar novamente o useEffect


   
   ### Enviar notificação
      -> Ao clicar no botão de uma venda, mande uma mensagem com os dados daquela venda

      -> Criar um type no componente do botão que recebe o id da venda
         -> Passar como parâmetro no botão, o type "Props"
      
      -> Criar função "handleClick"que chama o endpoint que manda a notificação
         -> Usar biblioteca "axios" para chamar a API

      -> Criar um evento onClick na div do botão
         -> Passar a função "handleClick" no evento
         -> Logo quando clicarmos, ele irá executar a função que chama a API de mandar SMS

      -> Por fim passar a Prop também nos arquivos onde o componente do botão está sendo utilizado

   

   ### Mensagem Toast de confirmação
      -> Instalar biblioteca toastify
         -> `npm install react-toastify@9.0.5`

      -> Importar:
         -> `import { ToastContainer } from 'react-toastify';` | `import 'react-toastify/dist/ReactToastify.css';`
         -> Criar um componente `<ToastContainer />` no "App.tsx"
         -> Com isso já podemos utilizar em qualquer componente a biblioteca toastify

         -> No componente do botão de notificação, colocamos como resposta da requisição da função que pega API do back-end, a mensagem Toast
            -> `toast.info("mensagem");`

   

   ### Deploy do front-end no Netlify
      -> Acrescentar `window.React = React` no "main.tsx"
      
      -> Base directory = frontend
      -> Build command = npm run build
      -> Publish directory = frontend/dist

      -> Variáveis de ambiente 
         -> key = VITE_BACKEND_URL
         -> value = link do back-end do Heroku ("https://semana-spring-react-igor.herokuapp.com")

      -> 