# Agrupamento de Unidades Georreferenciadas para  a Delimitação de  Áreas Postais
<p> A representação espacial dos limites urbanos, tem propiciado a observação dos fenômenos geogŕaficos, políticos e sociais, graças a extração de informações relevantes provenientes da manipulação e análise dos bancos de dados georeferênciados. </p>

<p> No que tange a postagem, o código de endereçamento postal (CEP), é um dos principais fatores que contribuem para a implementação da logística dos Correios. Através desse meio de endereçamento, é possivel setorizar a distribuição de correspondências e encomendas. </p>

<p> Este trabalho resultou em um sistema de informação geográfica que priorize a representação geoespacial do perímetro de cada CEP pertencente ao estado de Minas Gerais. A delimitação de área, dar-se-á por meio de um agrupamento geográfico advindo das unidades contidas na base alvo de exploração, disponibilizada no OpenAddresses e da exploração dos limites municipais de coletados no portal de mapas do IBGE. </p>

<p> A partir do modelo gerado, pretende-se obter visualizações da área correspondente a cada CEP,e também prover uma ferramenta que propicie consultas como as que visam estimar apartir das coordenadas geográficas, a qual CEP um determinado imóvel no espaço urbano, pertence. </p>

<p> Para permitir o acesso gratuito às informações, o arquivo em formato shapefile gerado será disponibilizado em um repositório aberto, para uso gratuito. </p>
  
**Palavras-chave:** Aplicação Urbana de SIG, Endereçamento Urbano, Banco de Dados Espaciais, Geoprocessamento
  
  ## Metodologia
  O sistema proposto no projeto, pode ser observado em quatro etapas de implementação, sendo que, em alto nível, cada componente do fluxo de trabalho pode ser definido como nas seções a seguir.
  
 ## Coleta de dados
Foi realizada uma busca pelas bases georeferênciadas necessárias para a modelagem proposta e por elementos que auxiliem no processo de visualização e identificaçãodo comportamento espacial dos dados. Como o objetivo do projeto também envolve a disponibilização de informações ao público, todos os dados coletados para o projeto são provenientes de repositórios abertos, conforme descrição abaixo: 
 <li> Utilização de base georeferênciada contendo aproximadamente, 9 milhões de logradouros em Minas Gerais, disponível em:
  <p> [OpenAddresses](https://batch.openaddresses.io/data/)<p> </li>
 <li> Utilização de base georeferênciada contendo aproximadamente, 750 mil logradouros em Belo Horizonte, fornecidos pelo portal:
  <p> [BHMap](http://bhmap.pbh.gov.br/v2/mapa/idebhgeo)<p> </li>
 <li> Aplicação de shapefiles contendo os limites das unidades federativas brasileiras e dos municípios de Minas Gerais, fornecidos pelo portal de mapas do IBGE, que serão utlizados nesse projeto auxiliar na visualização e, em caso de cidades com apenas um registro de CEP, na delimitação do polígono para a sua representação. </li>
 
 ## Modelagem
O processo de desenvolvimento do banco de dados espaciais e reorganização dosseus registros para uma estrutura que favoreça a representação da área espacial corre pondente a cada CEP no estado de Minas Gerais, dar-se-á da seguinte maneira: 
<li> Instalação de sistema gerenciador de banco de dados PostgresSQL3e de suaextensão PostGIS para a manipulação de dados espaciais. </li>
<li> Criação de um banco de dados SQL, que irá conter as informações necessáriaspara determinação das áreas de CEP em Minas Gerais. </li>
<li> Importação dos dados e dos limites municipais, citados na etapa de coleta, parao banco de dados iniciado. </li>
<li> Seleção e remoção de registros contidos no banco e que não correspondem aopropósito do modelo e possam interferir na qualidade do sistema, como os dadoscom o campo CEP vazio ou não associado ao estado de Minas Gerais, ou seja,não iniciados em três. </li>
<li> Associação dos limites municipais provenientes doIBGE, aos municipios com CEP único encontrados a partir de consulta à basedo OpenAddresses. A operação se faz necessária, uma vez que esses limites sãoa melhor representação de área possível para os registros apontado. </li>
<li> Construção de um cluster geográfico a partir dos logradouros oriundos da base coletada, agrupando as unidades que possuem o mesmo CEP em comum. </li>
<li> Criação de um envoltória convexa a partir dos pontos extremos dentro doclusterde cada CEP. </li>
<li> Junção do polígonos gerados a partir dos pontos georeferenciados obtidos através das bases do OpenAddresses e do BHMap </li>

### Tratamento dos Dados
Nessa etapa, é feita uma apuração dos resultados obtidos após a modelagem, embusca de estratégias para refinar a qualidade da informação explorada afim de obter resultados com maior exatidão, que são: 
<li> Seleção apenas das unidades de composição do agrupamento baseada em umaestimativa a partir da média e do limite de dispersão tolerável, sob um intervalode confiança de 9/5%. </li>
<li> Os n pontos cuja a distância para o centróide do cluster a qual pertencem está a uma dispersão dentro do intervalo, são considerados integrantes do agrupamento de CEP listado em seu campo descritor de código postal. </li>
<li> Geração das envoltórias convexas para cada cluster de CEP, a partir de uma função para estimar os limites mínimos de geografia para esse tipo de fecho, resultando a partir dessa operação, na criação de polígonos que representarão a área de cada CEP no território de Minas Gerais. </li>
<li> Na busca por obter uma melhor representação aproximada das áreas, foi feita uma junção dos polígonos obtidos por meio do cálculo de fecho convexo advindos do passo anterior e dos limites de área dos CEPs que cobrem todos os municípios, resultando em um arquivo no formato shapefile, a ser disponibilizado. </li>

## Visualizações e análise
Para validar as operações realizadas, será necessário a observação tanto do primeiro estado dos dados antes e após a modelagem, a partir de sua divisão por logradouros, quanto na organização proposta de agrupamentos por CEP, gerada após o processode tratamento. O processo de análise ocorrerá como sugerido a seguir:
<li> Utilização de ferramentas como o QGIS5, para auxiliar na visualização dos registros georeferenciados coletados. </li>
<li> A figura 3.5, é uma visualização gerada com o intuito de observar a concentração dos logradouros dentro do estado de Minas Gerais, como mapa de calor e pontos. </li>
<li> Criação de visualizações para observar as diferentes representações por polígonos,exploradas nas etapas de modelagem e tratamento. </li>

## Resultados e Discussão
<p> Ao final deste projeto, foi possível obter um banco de dados espacial com osclusters geográficos de CEPS disponíveis a partir dos pontos georeferenciados fornecidos pelo OpenAddresses, e ainda, como elemento resultante principal, um arquivo do tipo shapefile, que contém a representação da área que abrange cada um dos CEPs do estadode Minas Gerais. </p>
<p> O shapefile citado, é o produto da junção entre a operação que propiciou o uso dos limites municipais das cidades com CEP único, como melhor polígono obtido para determinar a área de código postal que abrange essas regiões, e a operação que estimou o polígono de cada um dos CEPs restantes, baseando-se em um ajuste dos agrupamentosde CEP em função da distância dos pontos para o centróide. </p>
<p> Essa estimativa, que visa diminuir a distorção dos polígonos, foi essencial para eliminar doclusterde cada CEP, as unidades residenciais e comerciais com a distância acima da tolerável para o centro de massa de cada agrupamento, devido a sua baixa probabilidade de real associação com o código postal descriminado em seu campo. </p>
<p> Em virtude da falta de unidades georeferenciadas para algumas regiões na basede dados utilizada, não foi possível estimar todos os CEPs de Minas Gerais, como pretendido. Outro entrave é que em regiões com alta densidade de áreas de CEP aserem geradas, a quantidade de pontos de alguns desses códigos postais era baixa, aponto de não ser possível eliminar possíveis outliers no Cluster, utilizando do método proposto. </p>
<p> Em suma, após a modelagem e tratamento dos dados, foi possível obter um arquivo, no formato shapefile, contendo a representação de área dos CEPs de Minas Gerais. </p>
 <p> O arquivo gerado, foi disponibilizado no seguinte repositório aberto: </p>
 <li> https://github.com/mcatrinque/area_cep_mg </li>
