# Tratamento e estruturação de dados

Este documento detalha o processo de limpeza, padronização e validação dos dados utilizados no projeto **Brazilian E-Commerce: Insights with SQL and Power BI**.  
O objetivo é garantir a integridade e consistência das informações antes da modelagem e análise no Power BI.

## :compass: 1. Análise Exploratória Inicial
A análise inicial teve como objetivo avaliar a integridade, consistência e qualidade dos dados em cada tabela do conjunto Olist. Foram verificados valores nulos, compatibilidade de tipos de dados e duplicatas em colunas-chave.

<img width="1425" height="655" alt="Captura de tela 2025-11-28 161719" src="https://github.com/user-attachments/assets/0cda427e-d876-4a5e-a093-ad1fe348a6ea" />

<img width="1423" height="433" alt="Captura de tela 2025-11-28 161922" src="https://github.com/user-attachments/assets/1b070c76-9fcb-4dcb-99d5-c4e9b17d519b" />

### :mag_right: 1.1. Principais achados por tabela

* __orders__ <br />
Nulos esperados em colunas de datas de aprovação, envio e entrega (pedidos em andamento ou cancelados). Sem duplicatas em order_id.  Campo order_status assume valor dentro de um conjunto de categorias. 

* __order_items__  <br />
Sem nulos, porém várias colunas (como order_id, product_id, seller_id) contêm valores repetidos, conforme esperado pela estrutura relacional (um pedido pode ter múltiplos itens).

* __customers__  <br />
Sem nulos. Tipos compatíveis com o dicionário. customer_id é único, mas customer_unique_id apresenta duplicatas, conforme esperado pela estrutura relacional (um cliente pode realizar mais de um pedido).

* __sellers__  <br />
Dados completos, sem nulos ou duplicatas. Tipos compatíveis com o dicionário. seller_id é único.

* __products__  <br />
Colunas relacionadas à descrição e dimensões físicas apresentam grande volume de nulos. Campo product_id é único. Campo product_category possui valores que podem representar redundância (categorias de mesmo nome, apenas com acréscimo de índice).

* __payments__  <br />
Sem nulos, mas presença de duplicatas esperadas em colunas como order_id e payment_type. Campo payment_type assume valor dentro de um conjunto de categorias. 

* __reviews__  <br />
Colunas textuais (review_comment_title, review_comment_message) contêm nulos esperados, visto que nem todo cliente terá preenchido a avaliação de maneira completa. A coluna review_id apresenta duplicatas não exatas — mesmo ID com diferentes valores em outros campos.

* __location__  <br />
Sem nulos. Alta quantidade de duplicatas — comportamento esperado, já que diferentes coordenadas podem compartilhar o mesmo prefixo de CEP.

## 🧩 2. Estratégias de Tratamento e Padronização

__Valores Nulos__

* product_category_name: valores nulos substituídos por "sem_categoria", representando produtos sem categoria registrada.
* product_name_length e product_description_length: nulos substituídos por 0, indicando ausência de nome e/ou descrição.
* product_photos_qty: nulos substituídos por 0, representando produtos sem fotos.
* Colunas físicas (product_weight_g, product_length_cm, product_height_cm, product_width_cm): mantidos como NaN, por se tratarem de ausências de registro e não valores iguais a zero.

__Duplicatas__
* reviews: registros com o mesmo review_id mas diferentes review_score foram mantidos, assumindo que refletem alterações do cliente (e não duplicatas exatas).
* location: considerada a coluna geolocation_zip_code_prefix como chave primária, usando apenas os cinco primeiros dígitos do CEP.
* Demais tabelas: duplicatas em colunas não identificadoras foram consideradas comportamentos esperados (ex.: múltiplos pagamentos por pedido, múltiplos itens por venda).

__Conversão de Tipos__
* Colunas de datas e timestamps foram convertidas para o tipo datetime, preservando informações de hora quando presentes.
* Variáveis do tipo object foram convertidas para string, e colunas categóricas (‘order_status’, ‘payment_type’) para category, visando otimização de memória e performance.
* Colunas ‘product_name_length’, ‘product_description_length’ e ‘product_photos_qty’ foram convertidas de float para int, visto que não podem assumir valores decimais.
* Colunas ‘customer_zip_code_prefix’, ‘seller_zip_code_prefix’ e ‘geolocation_zip_code_prefix’ foram inicialmente importadas como tipo string para preservar os zeros à esquerda que compõem o valor.

__Padronização__
* Normalização e remoção de caracteres especiais em ‘city’ (tabelas customers, sellers, location)
* Unificação de categorias removendo índices numéricos e caracteres especiais com exceção do ‘_’ (ex.: ‘casa_conforto’, ‘casa_conforto_2’, etc.). Reconhece-se que tais categorias poderiam representar subdivisões legítimas em um contexto real, mas foram agrupadas para fins analíticos.

## ✅ 3. Validação Pós-Tratamento
Após a padronização, foram realizadas checagens para confirmar a integridade dos dados:
* Verificação de unicidade das chaves primárias (‘order_id’ na tabela orders, ‘customer_id’ na tabela customers, etc.)  
* Checagem de consistência de tipos de dados 
* Validação do número de valores nulos por coluna
  
<img width="1380" height="432" alt="Captura de tela 2025-11-28 163319" src="https://github.com/user-attachments/assets/52a28846-2d03-406f-97dc-17ac73c56026" />

## :file_folder: 4. Construção do Modelo Relacional

Nesta etapa, os dados tratados foram organizados, através do SQLite, em uma estrutura relacional composta por tabelas de dimensão e fatos. A criação dessas tabelas, junto com seus identificadores e relacionamentos, permitiu consolidar informações de diferentes origens em um modelo único, consistente e consultável, o que garantiu maior eficiência nas análises posteriores.

Com a execução das etapas mencionadas, garantimos maior eficiência nas análises posteriores, facilitando a criação das views analíticas que serão detalhadas no readme.
