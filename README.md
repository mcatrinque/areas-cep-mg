# AGRUPAMENTO DE UNIDADES GEORREFERENCIADAS PARA A DELIMITAÇÃO DE ÁREAS POSTAIS 

## Introdução
<p> A representação espacial dos limites urbanos, tem propiciado a observação dos fenômenos geogŕaficos, políticos e sociais, graças a extração de informações relevantes provenientes da manipulação e análise dos bancos de dados georeferênciados. </p>

<p> No que tange a postagem, o código de endereçamento postal (CEP), é um dos principais fatores que contribuem para a implementação da logística dos Correios. Através desse meio de endereçamento, é possivel setorizar a distribuição de correspondências e encomendas. </p>

<p> Este trabalho resultou em uma base de dados contendo a representação espacial das áreas CEP pertencente ao estado de Minas Gerais. A delimitação de área, dar-se-á por meio de um agrupamento geográfico advindo das unidades contidas nas bases alvo de exploração.

<p> A partir do modelo gerado, pretende-se obter visualizações da área correspondente a cada CEP, provendo uma base que facilite consultas relacionadas ao códigos postais. </p>

<p> Para permitir o acesso gratuito às informações, a base de dados gerada será disponibilizada nesse repositório aberto, para uso gratuito. </p>
  
**Palavras-chave:** Aplicação Urbana de SIG, Endereçamento Urbano, Banco de Dados Espaciais, Geoprocessamento
  
 ## Metodologia
 <p> O processo de desenvolvimento do projeto é focado em 3 etapas que consistem na coleta, modelagem e tratamento dos dados. As análises e parte do processo realizado foram explicitados, ainda que resumidamente, em um notebook, que se encontra no link: </p>
 <li> https://github.com/mcatrinque/areas-cep-mg/blob/main/analise_area_cep_mg.ipynb</li> 
 
 ### Coleta de dados
Foi realizada uma busca pelas bases georeferênciadas necessárias para a modelagem proposta e por elementos que auxiliem no processo de visualização e identificaçãodo comportamento espacial dos dados. Como o objetivo do projeto também envolve a disponibilização de informações ao público, todos os dados coletados para o projeto são provenientes de repositórios abertos, conforme descrição abaixo: 
 <li> Utilização de base georeferênciada contendo aproximadamente, 9 milhões de logradouros em Minas Gerais, disponível em:
  <p> [OpenAddresses](https://batch.openaddresses.io/data/)<p> </li>
 <li> Utilização de base georeferênciada contendo aproximadamente, 750 mil logradouros em Belo Horizonte, fornecidos pelo portal:
  <p> [BHMap](http://bhmap.pbh.gov.br/v2/mapa/idebhgeo)<p> </li>
 <li> Aplicação de shapefiles contendo os limites das unidades federativas brasileiras e dos municípios de Minas Gerais, fornecidos pelo portal de mapas do IBGE, que serão utlizados nesse projeto auxiliar na visualização e, em caso de cidades com apenas um registro de CEP, na delimitação do polígono para a sua representação. </li>
 
 ### Modelagem
O processo de desenvolvimento do banco de dados espaciais e reorganização dos seus registros para uma estrutura que favoreça a representação da área espacial correspondente a cada CEP no estado de Minas Gerais, dar-se-á da seguinte maneira: 
<li> Instalação de sistema gerenciador de banco de dados PostgresSQL e de sua extensão PostGIS para a manipulação de dados espaciais. </li>
<li> Criação de um banco de dados SQL, que irá conter as informações necessáriaspara determinação das áreas de CEP em Minas Gerais. </li>
<li> Importação dos dados e dos limites municipais, citados na etapa de coleta, para o banco de dados iniciado. </li>
<li> Seleção e remoção de registros contidos no banco e que não correspondem aopropósito do modelo e possam interferir na qualidade do sistema, como os dadoscom o campo CEP vazio ou não associado ao estado de Minas Gerais, ou seja, não iniciados pelo algarismo 3. </li>
<li> Associação dos limites municipais provenientes do IBGE aos municipios com CEP único encontrados a partir de consulta à basedo OpenAddresses. </li>
<li> Construção de um cluster geográfico a partir dos logradouros oriundos da base coletada, agrupando as unidades que possuem o mesmo CEP em comum. </li>
<li> Junção dos registros georeferenciados obtidos através das bases </li>

### Tratamento dos dados
Nessa etapa é realizada a execução de técnicas de refinamento e abordagens para delimitação do contorno das áreas de CEP a partir do agrupamento das unidades residenciais e comerciais oriundas das bases de dados exploradas: 
<li> Seleção apenas das unidades de composição do agrupamento baseada em uma estimativa a partir da média e do limite de dispersão tolerável, sob um intervalo de confiança de 9/5%. </li>
<li> Os n pontos cuja a distância para o centróide do cluster a qual pertencem está a uma dispersão dentro do intervalo, são considerados integrantes do agrupamento de CEP listado em seu campo descritor de código postal. </li>
<li> Geração das envoltórias convexas para cada cluster de CEP, a partir de uma função para estimar os limites mínimos de geografia para esse tipo de fecho, resultando a partir dessa operação, na criação de polígonos que representarão a área de cada CEP no território de Minas Gerais. </li>
<li> Junção dos polígonos obtidos por meio do cálculo de fecho convexo advindos do passo anterior e dos limites de área dos CEPs que cobrem todos os municípios, em busca de obter uma melhor representação aproximada das áreas. </li>
<li> Aplicação de um contorno baseado em envoltório côncavo para a área que corresponde à Belo Horizonte, devido a sua alta concentração de CEPs </li>
### Visualizações e análise
Para validar as operações realizadas, será necessário a observação tanto do primeiro estado dos dados antes e após a modelagem, a partir de sua divisão por logradouros, quanto na organização proposta de agrupamentos por CEP, gerada após o processode tratamento. O processo de análise ocorrerá como sugerido a seguir:
<li> Utilização de ferramentas como o QGIS5, para auxiliar na visualização dos registros georeferenciados coletados. </li>
<li> A figura 3.5, é uma visualização gerada com o intuito de observar a concentração dos logradouros dentro do estado de Minas Gerais, como mapa de calor e pontos. </li>
<li> Criação de visualizações para observar as diferentes representações por polígonos, exploradas nas etapas de modelagem e tratamento. </li>

Os arquivos no formato shapefile, contendo as geometrias exploradas e geradas encontram-se em:
<li> https://drive.google.com/drive/folders/13VjXPGV-O8Eorm1qKM6DubVV4fq4dMj7?usp=sharing <\li>

## Resultados e Discussão
<p> Ao final deste projeto, foi possível obter um banco de dados espacial com os clusters geográficos de CEPS disponíveis a partir dos pontos georeferenciados fornecidos pelo OpenAddresses e pelo Portal da Prefeitura de Belo Horizonte, e ainda, como elemento resultante principal, um arquivo do tipo shapefile, que contém a representação da área que abrange cada um dos CEPs do estadode Minas Gerais. </p>
<p> A nova base de dados é produto da junção entre o uso dos limites municipais das cidades com CEP único como melhor representação de área postal, a estimativa de geometrias baseadas em envoltória côncava para as regiões de alta concentração de CEPs, e o agrupamentos de unidades em função de um envoltório convexo, para os CEPs restantes. </p>
<p> Em suma, após a modelagem e tratamento dos dados, foi possível obter uma base dados contendo a representação de área dos CEPs de Minas Gerais. </p>
 <p> O arquivo gerado, foi disponibilizado no seguinte repositório aberto: </p>
 <li> https://github.com/mcatrinque/areas-cep-mg/blob/main/areas_cep_mg.csv </li>
