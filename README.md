# Interface de usuário encurtador de URL's.

Interface feita em **Vue.Js** e **Javascript**.

# Instalação e executação do projeto

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

### Notificações

- Como forma de melhorar a experiência do usuário em resposta a eventos disparados na aplicação, estou usando a biblioteca "toastify" para criar notificações simples e diretas.