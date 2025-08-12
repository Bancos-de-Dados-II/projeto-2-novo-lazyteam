# Sistema de Gestão de Propriedades Rurais

Este projeto é uma aplicação web completa para o CRUD (Create, Read, Update, Delete) de propriedades rurais, com foco em dados espaciais e visualização interativa. Ele permite o cadastro, visualização, edição e exclusão de propriedades, integrando funcionalidades de geolocalização e um dashboard analítico com MongoDB Charts.

## Funcionalidades Principais

- *CRUD Completo*: Gerenciamento de propriedades rurais (cadastro, listagem, atualização e exclusão).
- *Geolocalização Avançada*: Utilização do Leaflet para exibição de mapas interativos e Nominatim para busca de localidades.
- *Dados Espaciais com PostGIS*: Armazenamento de informações geográficas utilizando objetos espaciais no MongoDB (simulando PostGIS para fins de dados GeoJSON).
- *Dashboard Analítico*: Integração com MongoDB Charts para visualização de dados em tempo real, incluindo distribuição por cultura, mapa geográfico e maiores propriedades.
- *Interface Intuitiva*: Desenvolvido com HTML, CSS e JavaScript para uma experiência de usuário fluida e responsiva.

## Tecnologias Utilizadas

### Frontend
- *HTML5*: Estrutura da aplicação web.
- *CSS3*: Estilização e design responsivo.
- *JavaScript (ES6+)*: Lógica de frontend, interatividade e comunicação com a API.
- *Leaflet.js*: Biblioteca JavaScript para mapas interativos.
- *Leaflet Control Geocoder*: Plugin para geocodificação (busca de endereços) no mapa.
- *MongoDB Charts Embed SDK*: Para integrar os gráficos analíticos do MongoDB Charts.

### Backend
- *Node.js*: Ambiente de execução JavaScript no servidor.
- *Express.js*: Framework web para construção da API RESTful.
- *Mongoose*: ODM (Object Data Modeling) para Node.js, facilitando a interação com o MongoDB.
- *MongoDB Atlas*: Banco de dados NoSQL baseado em documentos, hospedado na nuvem, utilizado para armazenar os dados das propriedades, incluindo os dados espaciais (GeoJSON).
- *CORS*: Middleware para habilitar requisições de diferentes origens.
- *Dotenv*: Para gerenciar variáveis de ambiente.

## Como Funciona a Geolocalização

A aplicação utiliza a biblioteca Leaflet.js para renderizar o mapa interativo. A geolocalização é implementada da seguinte forma:

1.  *Localização Inicial*: Ao carregar a página, a aplicação tenta obter a localização atual do navegador do usuário. Se bem-sucedida, o mapa é centralizado nessa posição. Caso contrário, o mapa é centralizado em uma localização padrão (ex: Brasil).
2.  *Seleção no Mapa*: O usuário pode clicar em qualquer ponto do mapa para definir a latitude e longitude de uma propriedade. Essas coordenadas são automaticamente preenchidas nos campos do formulário.
3.  *Busca por Localidade (Geocodificação)*: Através do campo "Buscar Localização" e do botão "Buscar no Mapa", o usuário pode digitar um endereço ou nome de local. A aplicação utiliza o serviço Nominatim (OpenStreetMap) para converter o texto em coordenadas geográficas. O mapa é então centralizado na localização encontrada e os campos de latitude/longitude são preenchidos.
4.  *Armazenamento Espacial*: As coordenadas de localização são salvas no MongoDB Atlas como objetos GeoJSON do tipo Point. Isso permite consultas espaciais avançadas diretamente no banco de dados.

## Integração com MongoDB Charts

O dashboard analítico é integrado diretamente no frontend utilizando o MongoDB Charts Embed SDK. Os gráficos são carregados dinamicamente e exibem dados em tempo real das propriedades cadastradas. Os charts implementados são:

-   *Distribuição por Cultura Principal*: Um gráfico de donut que mostra a proporção de propriedades para cada tipo de cultura cultivada.
-   *Distribuição Geográfica das Propriedades*: Um mapa que visualiza a localização das propriedades, permitindo uma análise espacial da distribuição.
-   *Maiores Propriedades*: Uma tabela que lista as propriedades com as maiores áreas, fornecendo um ranking rápido.

Esses gráficos são configurados no MongoDB Atlas e seus IDs são referenciados no código JavaScript do frontend, garantindo que os dados exibidos estejam sempre atualizados com o banco de dados.

## Configuração e Execução do Projeto

Para configurar e rodar este projeto em seu ambiente local, siga os passos abaixo:

### Pré-requisitos

-   Node.js (versão 14 ou superior)
-   npm (gerenciador de pacotes do Node.js)
-   Acesso à internet para o backend se conectar ao MongoDB Atlas e para o frontend carregar as bibliotecas Leaflet e Charts SDK.

### 1. Configuração do Banco de Dados (MongoDB Atlas)

1.  *Crie um Cluster*: Se ainda não tiver, crie um cluster no MongoDB Atlas.
2.  *Crie um Usuário de Banco de Dados*: Crie um usuário com permissões de leitura e escrita para o seu banco de dados.
3.  *Configure o Network Access (IP Whitelist)*: Adicione o endereço IP da sua máquina (ou 0.0.0.0/0 para permitir acesso de qualquer IP, *não recomendado para produção*) à lista de IPs permitidos no seu cluster.
4.  *Obtenha a String de Conexão*: No seu cluster, clique em "Connect", selecione "Connect your application" e copie a string de conexão SRV (ex: mongodb+srv://<username>:<password>@cluster0.xxxxx.mongodb.net/?retryWrites=true&w=majority).
5.  *Configure os MongoDB Charts*: 
    -   Ative o MongoDB Charts no seu projeto Atlas.
    -   Crie um dashboard e os charts mencionados na seção "Integração com MongoDB Charts", utilizando a collection propriedades do database test como fonte de dados.
    -   Habilite o embedding não autenticado para cada chart e anote os Chart IDs e a Base URL.

### 2. Configuração do Backend

1.  **Navegue até a pasta backend** do projeto:
    bash
    cd backend
    
2.  **Crie um arquivo .env**: Na pasta backend, crie um arquivo chamado .env e adicione sua string de conexão do MongoDB Atlas:
    
    MONGO_URI=sua_string_de_conexao_do_mongodb_atlas
    PORT=5000
    
    Substitua sua_string_de_conexao_do_mongodb_atlas pela string que você copiou do Atlas.
3.  *Instale as dependências*: 
    bash
    npm install
    
4.  *Inicie o servidor backend*: 
    bash
    node server.js
    
    O servidor estará rodando em http://localhost:5000 (ou na porta que você definiu no .env).

### 3. Configuração do Frontend

1.  *Atualize os IDs dos Charts*: Abra o arquivo frontend/script.js e atualize os `chartId`s e baseUrl na constante CHARTS_CONFIG com os IDs e a Base URL dos seus próprios charts do MongoDB Atlas:
    javascript
    const CHARTS_CONFIG = {
        baseUrl: "https://charts.mongodb.com/charts-project-0-qgebchr", // Sua Base URL
        charts: {
            cultura: "9bb2278c-4d8e-4f29-aafc-11d59e178f83", // Seu Chart ID para Cultura
            mapa: "e4350898-03dc-4c33-91c2-ee9992bbb08f",   // Seu Chart ID para Mapa
            tabela: "736963f0-e463-49a8-a979-d77a060e00c4"    // Seu Chart ID para Tabela
        }
    };
    
2.  **Abra o arquivo index.html**: No seu navegador web, abra o arquivo frontend/index.html diretamente. Não é necessário um servidor web para o frontend, pois ele faz requisições para o backend Node.js.

## Estrutura do Projeto


. 
├── backend/               # Código do servidor Node.js (API RESTful)
│   ├── node_modules/      # Dependências do Node.js
│   ├── models/            # Definição do schema do MongoDB (propriedade.js)
│   ├── routes/            # Rotas da API (propriedades.js)
│   ├── .env.example       # Exemplo de arquivo de variáveis de ambiente
│   ├── package.json       # Metadados e dependências do backend
│   ├── server.js          # Ponto de entrada do servidor
├── frontend/              # Código da interface do usuário (HTML, CSS, JS)
│   ├── index.html         # Página principal da aplicação
│   ├── script.js          # Lógica JavaScript do frontend
│   ├── styles.css         # Estilos CSS da aplicação
├── README.md              # Este arquivo
└── .gitignore             # Arquivos e diretórios a serem ignorados pelo Git
