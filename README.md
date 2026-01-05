# olist-analysis
Este projeto tem como objetivo avaliar padrões e identificar oportunidades para aumentar vendas e garantir satisfação dos clientes, utilizando Python para limpeza dos dados, SQL para análise e visualização estratégica em Power BI.

## 1. Sobre o Projeto
A partir de uma base pública de dados, este estudo realiza uma exploração detalhada, buscando insights que ajudem em decisões de negócio.

## 2. Objetivos da Análise
* Investigar padrões de compra por região, categoria e período
* Avaliar desempenho de entregas e impacto na satisfação dos clientes
* Identificar oportunidades de melhoria nas vendas
* Construir visualizações claras para tomadas de decisão

## 3. Escopo Técnico
Ferramentas utilizadas:
* **SQLite**: para armazenamento e consultas SQL sobre os dados brutos.
* **Python** (*Google Colab*): para conexão com o banco, manipulação de dados e criação de views.
* **Power BI**: para visualização dos resultados e construção de dashboards interativos.
* **GitHub**: para documentação do projeto.

## 4. Fonte dos Dados
[Brazilian E-Commerce Public Dataset by Olist](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce)

## 5. Dataset
* olist_customers_dataset.csv: contém informações sobre o cliente e sua localização. 
* olist_geolocation_dataset.csv: contém informações dos CEPs brasileiros e suas coordenadas.
* olist_order_items_dataset.csv: inclui as informações dos produtos comprados em cada pedido.
* olist_order_payments_dataset.csv: inclui as informações sobre as opções de pagamento.
* olist_order_reviews_dataset.csv: inclui informações das avaliações feitas pelos clientes.
* olist_orders_dataset.csv: a tabela fato, contendo informações de vendas entre 2016 e 2018.
* olist_products_dataset.csv: inclui informações sobre os produtos vendidos.
* olist_sellers_dataset.csv: inclui informações sobre os vendedores/empresas que tiveram pedidos registrados no período considerado.

## 6. Processo de Análise
* Limpeza e padronização dos dados com o Python
* Exploração e construção de queries SQL
* Modelagem das principais métricas para o negócio
* Construção do dashboard no Power BI


### 6.1. Limpeza e padronização dos dados

Antes da modelagem e da construção do dashboard, os dados passaram por um processo estruturado de análise exploratória, limpeza e padronização, com o objetivo de garantir integridade, consistência e confiabilidade das informações utilizadas nas análises.

Esse processo incluiu:

* Avaliação de valores nulos, duplicatas e compatibilidade de tipos em todas as tabelas do dataset Olist
* Tratamento de nulos conforme o contexto de negócio (ex.: produtos sem categoria, ausência de descrição ou fotos)
* Manutenção de duplicidades esperadas pela estrutura relacional (ex.: múltiplos itens por pedido, múltiplos pagamentos)
* Conversão e padronização de tipos de dados, especialmente datas, variáveis categóricas e identificadores
* Normalização de campos textuais (cidades e categorias de produtos) para reduzir inconsistências semânticas
* Validações pós-tratamento para garantir unicidade de chaves, consistência de tipos e controle de nulos

Ao final, os dados tratados foram organizados em um modelo relacional no SQLite, servindo de base para a criação das views analíticas consumidas no Power BI.

**Para detalhes sobre decisões de tratamento, exceções e validações realizadas, consulte o documento _'tratamento_dados.md'_.**

### 6.2. Modelagem
Após o processo de tratamento e padronização, os dados foram organizados em um modelo relacional no SQLite, estruturado a partir de tabelas de fatos e dimensões. O objetivo dessa etapa foi criar uma base consistente, integrada e performática para suportar análises financeiras, logísticas e de satisfação do cliente.

A modelagem contempla as seguintes tabelas principais:

📋 **Dimensões**

* dim_customers: dados consolidados de clientes, incluindo informações de localização
* dim_sellers: informações cadastrais e geográficas dos vendedores
* dim_products: atributos de produtos e categorias

🎯 **Fatos**

* fact_orders: eventos do ciclo do pedido e status logístico
* fact_order_items: itens vendidos por pedido
* fact_payments: registros de pagamentos associados aos pedidos
* fact_reviews: avaliações realizadas pelos clientes

**Os relacionamentos entre as tabelas foram definidos por meio de chaves primárias e estrangeiras**, garantindo integridade referencial entre pedidos, clientes, produtos, vendedores, pagamentos e avaliações. 

A modelagem foi pensada para que o Power BI consuma dados já organizados e semanticamente coerentes, reduzindo a necessidade de transformações adicionais.

### 6.3. Criação de métricas
Com base no modelo relacional, foram desenvolvidas views analíticas responsáveis por consolidar indicadores diretamente no banco de dados.

⚙️ **Views analíticas**

* Pedidos (vw_orders_kpis)<br>
Consolida métricas financeiras e logísticas por pedido, incluindo valor total, status de pagamento, tempos de aprovação, envio e entrega, além de indicadores de atraso e cumprimento de prazo.

* Produtos (vw_orders_items_kpis)<br>
Resume valores e quantidades vendidas por item e produto, permitindo análises de desempenho por categoria e participação nas vendas.

* Clientes (vw_customers_kpis)<br>
Caracteriza o comportamento de compra dos clientes, trazendo número de pedidos, valor total gasto, ticket médio e identificação de clientes recorrentes.

* Vendedores (vw_sellers_kpis)<br>
Apura itens vendidos, receita gerada por vendedor e tempo médio de entrega considerando apenas pedidos concluídos.

* Avaliações (vw_reviews_kpis)<br>
Classifica as avaliações em promoters, passives e detractors, servindo de base para análises de NPS e satisfação do cliente.


📊 **Principais indicadores**
As views foram construídas a partir de indicadores que orientaram a análise do negócio, entre eles:

* Ticket médio
* Receita total e receita por vendedor
* Gasto médio por cliente
* Categorias com maior participação em vendas
* Tempo médio de entrega
* Percentual de entregas no prazo e atrasadas
* Taxa de recompra
* Distribuição de avaliações (NPS)

**Ao disponibilizar essas métricas por meio de views, o Power BI passa a consumir dados já consolidados, limpos e padronizados, o que reduz a complexidade do modelo e facilita a criação de dashboards mais estáveis e confiáveis.**

## 7. Principais Resultados e Insights
Esta seção reúne os principais achados da análise exploratória e do dashboard, organizados por perspectiva de negócio.

### 7.1. Diagnóstico

📌 **Visão geral (big numbers)** <br>

Ao longo do período analisado (setembro/2016 a setembro/2018), a Olist registrou mais de **98 mil pedidos válidos**, totalizando um **faturamento superior a R$ 15 milhões e um ticket médio de R$ 160,25**. Nesse intervalo, mais de 96 mil clientes utilizaram a plataforma, sendo aproximadamente **12% clientes recorrentes** nesse período. Do ponto de vista operacional, **93% dos pedidos foram entregues dentro do prazo**, resultado que se reflete em um **NPS de 63**, indicando um elevado nível de satisfação dos clientes.

💰 **Vendas**
* Observa-se uma **concentração de vendedores nas regiões Sul e Sudeste do Brasil**, o que deve ser analisado em conjunto com a distribuição geográfica dos clientes.
* **A receita apresenta crescimento levemente acentuado no final de 2017, com pico em novembro, possivelmente influenciado pela sazonalidade do varejo** (festas de fim de ano). No entanto, o comportamento volta a se mostrar mais linear no início de 2018, e o período analisado não é suficiente para confirmar uma tendência estrutural de crescimento sazonal.
* **O ticket médio dos pedidos cancelados é superior ao dos pedidos concluídos**, indicando a necessidade de investigação das causas de cancelamento, especialmente em pedidos de maior valor.
* As categorias com maior participação em vendas são:
  * Beleza e saúde
  * Relógios e presentes
  * Cama, mesa e banho

Observa-se um **grande número de categorias com baixa participação em vendas**, o que dificulta análises agregadas e comparações diretas entre grupos de produtos. Esse comportamento pode ser explicado por diferentes hipóteses não excludentes:
* Falta de padronização na categorização
* Presença de uma cauda longa de produtos, característica comum em marketplaces, onde muitas categorias possuem baixo volume individual, mas representam diversidade de oferta.
* Baixa maturidade ou curadoria do catálogo, com inclusão de categorias pouco relevantes do ponto de vista comercial.
* Distribuição desigual da demanda entre categorias

👥 **Clientes**
* **O NPS de 63 indica um bom nível de satisfação quando comparado a benchmarks do setor de varejo e e-commerce**, reforçado pela concentração de aproximadamente 60% das avaliações com nota máxima (5 estrelas).
* Apesar da alta satisfação, há baixa retenção de clientes: **apenas 12% realizaram mais de uma compra no período analisado (setembro/2016 a setembro/2018), com média de 1,18 pedidos por cliente**.
* A análise conjunta da receita acumulada, do crescimento da base de clientes e da baixa retenção indica que o **crescimento da receita está fortemente condicionado à expansão da base de clientes**. Esse cenário pode representar um risco no longo prazo, especialmente considerando os custos de aquisição e expansão dessa base.
* A distribuição geográfica dos clientes também apresenta concentração no Sul e Sudeste, no entanto menor quando comparada à dos vendedores. Isso pode indicar oportunidades de mercado ainda não plenamente exploradas nas regiões de menor concentração que podem impactar custos logísticos e níveis de serviço.

🚚 **Operação**
* **A maior parte dos pedidos foi enviada e entregue dentro do prazo, além de apresentarem, em média, antecedência relevante em relação à data estimada**. Isso pode indicar eficiência operacional ou estimativas conservadoras de prazo — hipótese que merece investigação adicional.
* O tempo médio de entrega por estado de origem do vendedor mostra tendência de entregas mais rápidas para vendedores localizados no Sul e Sudeste. Destaca-se ainda que as diferenças entre as origens não são muito altas - no caso do Amazonas, origem com maior média de entrega, o valor é impactado por um outlier, e como há apenas um vendedor no estado, o indicador apresenta maior viés.
* **O tempo médio de aprovação (~6 horas) pode estar associado à participação relevante de boletos como meio de pagamento**. Essa hipótese sugere uma oportunidade de investigação adicional para possíveis ganhos operacionais e redução de cancelamentos.
  
### 7.2. Recomendações e próximos passos
Com base no diagnóstico, destacam-se as seguintes linhas de investigação e ação:
* Aprofundar a análise de cancelamentos, especialmente em pedidos de maior valor, avaliando sua relação com meio de pagamento, tempo de aprovação e status logístico.
* Reavaliar a estrutura de categorias de produtos, explorando agrupamentos, hierarquias ou filtros que reduzam a pulverização e facilitem análises estratégicas.
* Investigar estratégias de retenção de clientes, dado o contraste entre alta satisfação (NPS) e baixa recompra, analisando recorrência por categoria, ticket e perfil de cliente.
* Avaliar a expansão ou redistribuição da base de vendedores, considerando regiões com menor concentração atual e possível impacto em custos logísticos e nível de serviço.
* Analisar prazos estimados de entrega, verificando se a antecedência observada reflete eficiência real ou estimativas conservadoras que podem ser otimizadas.
* Explorar alternativas operacionais para meios de pagamento, especialmente boletos, visando reduzir tempo de aprovação e mitigar cancelamentos.

## 8. Estrutura do Projeto

 **olist-analytics** <br>
├── LICENSE
├── README.md<br>
├── **dashboard/** <br>
│   ├── README.md<br>
│   ├── olist_clientes.png<br>
│   ├── olist_operacao.png<br>
│   └── olist_vendas.png<br>
├── **data/** <br>
│   └── README.md<br>
├── **docs/** <br>
│   └── tratamento_dados.md<br>
└── **notebooks/** <br>
     └── olist_brazilian_ecommerce.ipynb<br>

- **README.md**: documentação principal do projeto, incluindo contexto, modelagem, métricas e principais insights.
- **dashboard/**: documentação do dashboard em Power BI, com imagens das principais abas e link para a versão interativa.
- **data/**: descrição das fontes de dados e decisões relacionadas ao armazenamento e versionamento dos dados.
- **docs/**: documentação complementar do projeto, com detalhamento técnico do tratamento e validação dos dados.
- **notebooks/**: notebooks utilizados para exploração, limpeza, padronização e preparação dos dados.

## 9. Como Reproduzir o Projeto

Este projeto foi desenvolvido a partir do conjunto de dados público **Olist Brazilian E-commerce Dataset**.

Devido a limitações de tamanho do GitHub, os arquivos de dados tratados (banco SQLite .db) e o arquivo do dashboard (.pbix) não estão versionados neste repositório. Esses arquivos estão disponíveis para download via Google Drive.

Para reproduzir o projeto:

1. Faça o download do dataset original do Olist (Kaggle)
2. Execute o notebook disponível em _‘notebooks/olist_brazilian_ecommerce.ipynb’_ para realizar a limpeza, padronização e construção do modelo relacional. Como o projeto foi desenvolvido no Google Colab, a importação dos arquivos foi feita por meio do Google Drive.
3. Utilize o banco SQLite gerado como fonte de dados no Power BI Desktop.
4. Replique as métricas e visualizações conforme documentado neste README e na pasta _‘dashboard/’_.

Os critérios de tratamento dos dados estão detalhados em _‘docs/tratamento_dados.md’_, e as métricas analíticas são descritas na seção de modelagem deste README.


## 10. Créditos e Licença

Este projeto é disponibilizado sob a licença MIT.

Os dados utilizados são de domínio público e pertencem aos seus respectivos autores (Olist / Kaggle), sendo utilizados exclusivamente para fins educacionais e de demonstração analítica.

