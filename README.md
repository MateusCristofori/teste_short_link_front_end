# Interface de usuário encurtador de URL's.

Interface feita em **Vue.Js** e **Javascript**.

# Instalação e execução do projeto

## Dependências globais

Clone o repositório e rode o comando `npm install` na pasta raiz do projeto.

## Execução da aplicação

Após a instalar todas as dependências, basta rodar o comando `npm run dev` para iniciar a aplicação.
A aplicação está setada para rodar no endereço padrão: `http://localhost:5173`

Observações:

- Para finalizar a execução da aplicação, basta utilizar as telcas `CTRL+C` dentro da pasta raiz do projeto.

## Templates da aplicação

1. Home

- No projeto iremos ter dois templates simples. O primeiro template, a 'Home', é responsável por retornar todos as URL's com suas respectivas URL's encurtadas. Cada URL terá seu card. Nesse card será exibido a URL original, URL encurtada e a data a qual essa URL encurtada foi criada.

- Os registros de URL estão sendo páginados em 10 registros por página. Na última página, aparecerá um botão que permitirá retornar para a primeira página.

2. Encurtador de URL

- O segundo template é justamente o formulário para encurtar a URL passada.

- A validação da URL está sendo delegada para o construtor do objeto "URL". Caso a instanciação desse objeto não ocorra, significa que a URL é inválida, um erro será retornado e o input da URL receberá "focus" para o usuário digitar novamente a URL.

Notas:

- Para a funcionalidade de contagem de click's em cada link, foram criados um objeto e uma função. Esse objeto irá guardar o valor da hash juntamente com sua quantidade de clicks. Na função "getAllLinksFromAPI", iremos rodar um forEach para checar se cada hash retornado pela resposta da requisição existe no objeto "clicks". Quando o componente for montado pela primeira vez, nenhum hash irá existir lá dentro. Então será sempre "0".

- Sempre quando o usuário clicar no link encurtado, esse "hash" (link encurtado) será enviada para função "redirect". Essa função redirect recebe esse hash, faz o redirecionamento do usuário para o site da URL original e envia o valor do hash para a função "countShortedLinkClicks". Essa última função irá fazer uma requisição para a API, onde terá seu valor incrementado em 1.

- No template, vamos sempre referenciar a esse objeto "clicks" passando o "hash" dos links. Como estamos rodando um forEach para cada link que retornou da resposta da requisição, podemos usar esse hash para "pesquisar" se esse o mesmo existe dentro do objeto "clicks".
  Ex: this.clicks[link.hash]

### Notificações

- Como forma de melhorar a experiência do usuário em resposta a eventos disparados na aplicação, estou usando a biblioteca "toastify" para criar notificações simples e diretas.
