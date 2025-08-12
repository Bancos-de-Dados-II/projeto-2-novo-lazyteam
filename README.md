# Sistema de Gestão de Propriedades Rurais

Este projeto é uma aplicação web completa para o CRUD (Create, Read, Update, Delete) de propriedades rurais, com foco em dados espaciais e visualização interativa. Ele permite o cadastro, visualização, edição e exclusão de propriedades, integrando funcionalidades de geolocalização e um dashboard analítico com MongoDB Charts.

## Funcionalidades Principais

- **CRUD Completo**: Gerenciamento de propriedades rurais (cadastro, listagem, atualização e exclusão).
- **Geolocalização Avançada**: Utilização do Leaflet para exibição de mapas interativos e Nominatim para busca de localidades.
- **Dados Espaciais com PostGIS**: Armazenamento de informações geográficas utilizando objetos espaciais no MongoDB (simulando PostGIS para fins de dados GeoJSON).
- **Dashboard Analítico**: Integração com MongoDB Charts para visualização de dados em tempo real, incluindo distribuição por cultura, mapa geográfico e maiores propriedades.
- **Interface Intuitiva**: Desenvolvido com HTML, CSS e JavaScript para uma experiência de usuário fluida e responsiva.
- **Busca por Texto (Full-Text Search)**: A funcionalidade de busca utiliza um índice de texto dedicado no MongoDB. Essa indexação é configurada diretamente no `PropriedadeSchema`. Para garantir a relevância dos resultados, a configuração do índice define `pesos` (weights) para cada campo. Uma palavra-chave encontrada no nome da propriedade tem um peso maior do que no campo de descrição, garantindo que os resultados mais precisos sejam priorizados. No back-end, a busca é executada através de um operador de consulta otimizado que utiliza este índice para retornar rapidamente os documentos já ranqueados por relevância.
- 
## Tecnologias Utilizadas

### Frontend
- **HTML5**: Estrutura da aplicação web.
- **CSS3**: Estilização e design responsivo.
- **JavaScript (ES6+)**: Lógica de frontend, interatividade e comunicação com a API.
- **Leaflet.js**: Biblioteca JavaScript para mapas interativos.
- **Leaflet Control Geocoder**: Plugin para geocodificação (busca de endereços) no mapa.
- **MongoDB Charts Embed SDK**: Para integrar os gráficos analíticos do MongoDB Charts.

### Backend
- **Node.js**: Ambiente de execução JavaScript no servidor.
- **Express.js**: Framework web para construção da API RESTful.
- **Mongoose**: ODM (Object Data Modeling) para Node.js, facilitando a interação com o MongoDB.
- **MongoDB Atlas**: Banco de dados NoSQL baseado em documentos, hospedado na nuvem, utilizado para armazenar os dados das propriedades, incluindo os dados espaciais (GeoJSON).
- **CORS**: Middleware para habilitar requisições de diferentes origens.
- **Dotenv**: Para gerenciar variáveis de ambiente.

## Como Funciona a Geolocalização

A aplicação utiliza a biblioteca Leaflet.js para renderizar o mapa interativo. A geolocalização é implementada da seguinte forma:

1.  **Localização Inicial**: Ao carregar a página, a aplicação tenta obter a localização atual do navegador do usuário. Se bem-sucedida, o mapa é centralizado nessa posição. Caso contrário, o mapa é centralizado em uma localização padrão (ex: Brasil).
2.  **Seleção no Mapa**: O usuário pode clicar em qualquer ponto do mapa para definir a latitude e longitude de uma propriedade. Essas coordenadas são automaticamente preenchidas nos campos do formulário.
3.  **Busca por Localidade (Geocodificação)**: Através do campo "Buscar Localização" e do botão "Buscar no Mapa", o usuário pode digitar um endereço ou nome de local. A aplicação utiliza o serviço Nominatim (OpenStreetMap) para converter o texto em coordenadas geográficas. O mapa é então centralizado na localização encontrada e os campos de latitude/longitude são preenchidos.
4.  **Armazenamento Espacial**: As coordenadas de localização são salvas no MongoDB Atlas como objetos GeoJSON do tipo `Point`. Isso permite consultas espaciais avançadas diretamente no banco de dados.

## Integração com MongoDB Charts

O dashboard analítico é integrado diretamente no frontend utilizando o MongoDB Charts Embed SDK. Os gráficos são carregados dinamicamente e exibem dados em tempo real das propriedades cadastradas. Os charts implementados são:

-   **Distribuição por Cultura Principal**: Um gráfico de donut que mostra a proporção de propriedades para cada tipo de cultura cultivada.
-   **Distribuição Geográfica das Propriedades**: Um mapa que visualiza a localização das propriedades, permitindo uma análise espacial da distribuição.
-   **Maiores Propriedades**: Uma tabela que lista as propriedades com as maiores áreas, fornecendo um ranking rápido.

Esses gráficos são configurados no MongoDB Atlas e seus IDs são referenciados no código JavaScript do frontend, garantindo que os dados exibidos estejam sempre atualizados com o banco de dados.
