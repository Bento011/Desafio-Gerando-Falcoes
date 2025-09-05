# Desafio Gerando Falcões

## SUMÁRIO
1. [INTRODUÇÃO](#id1) <br>
    1.1 [Objetivo](#id1_1) <br>
    1.2 [Resultado esperado](#id1_2) <br>
    1.3 [Conteúdos do repositório](#id1_3) <br>
    1.4 [Instruções para Executar o Notebook no Databricks](#id1_4) <br>
2. [BASE DE DADOS](#id2) <br>
    2.1 [Descrição](#id2_1) <br>
3. [METODOLOGIA](#id3) <br>
    3.1 [Tratamentos realizados](#id3_1) <br>
    3.2 [Premissas](#id3_2) <br>
4. [RESULTADOS](#id4) <br>
    4.1 [Tabela Final](#id4_1) <br>

<a name="id1"></a>
## 1. INTRODUÇÃO
 
<a name="id1_1"></a>
### 1.1 Objetivo
O objetivo deste projeto é analisar os dados de 3 bases de dados fonecidas e construir uma análise focada na geração de insights e apoio a tomada de decisão.

<a name="id1_2"></a>
### 1.2 Resultado esperado
Espera-se a elaboração de um notebook de exploração, explicitando as características importantes da base e definindo as transformações necessárias. 
Além disso, uma rotina de ETL, otimiziando e estruturando a exploração dos dados em uma pipeline escalável para alimentar a base final. 
Por fim, um painel negocial, exibindo os dados da base final e servindo como ferramenta de apoio para tomada de decisão e geração de insights.
 
<a name="id1_3"></a>
### 1.3 Conteúdos do repositório
Abaixo, encontram-se as descrições de cada item do repositório:

- **Gerando_Falcoes_Painel**: Arquivo .pbxi (Power BI) com o Painel Negocial alimentado pela base final.

- **Gerando_Falcoes_Painel**: Arquivo .pbxi (Power BI) com uma segunda versão do Painel Negocial alimentado pela base final.

- **Gerando_Falcoes_ETL**: Notebook contendo o processo de ETL aplicado nos dados fornecidos.

- **Gerando_Falcoes_v1**: Notebook de exploração estruturado, com o passo-a-passo dos testes necessários para a construção da ETL.

- **Gerando_Falcoes_v0**: Notebook de exploração não estruturado, com a primeira análise exploratória dos dados.

- **Base_Unificada_GF**: Amostra da base final e saída da ETL.

- **clientes.csv, vendas.csv, produtos.csv**: Dados de origem.

<a name="id1_4"></a>
### 1.4 Instruções para Executar o Notebook no Databricks

#### Pré-requisitos
1. **Conta no Databricks**: Certifique-se de ter uma conta no Databricks. Se não tiver, crie uma em [Databricks](https://databricks.com/).

2. **Acesso ao GitHub**: Tenha uma conta no GitHub para clonar o repositório onde o notebook está armazenado.

#### Passo-a-Passo

##### 1. Configuração Inicial
1. **Login no Databricks**:
   - Acesse o Databricks e faça login com suas credenciais.

##### 2. Importação do Notebook
1. **Importar Notebook**:
   - No menu lateral, clique em "Workspace".
   - Navegue até a pasta onde deseja importar o notebook.
   - Clique em "Import" e selecione "URL".
   - Insira a URL do notebook no GitHub ou faça upload do arquivo `.dbc` ou `.ipynb` do notebook.

2. **Importar Base de Dados**:
   - No repositóno no GitHub, baixe os arquivos fornecidos: `vendas.csv`, `produtos.csv` e `clientes.csv`.
   - No menu lateral, clique em "Data".
   - Clique em "Add Data" e selecione "Upload File".
   - Selecione o arquivo CSV e faça o upload.

##### 3. Execução do Notebook
1. **Abrir o Notebook**:
   - Navegue até o notebook importado no seu workspace.
   - Clique para abrir o notebook.

2. **Anexar o Cluster**:
   - No topo do notebook, selecione o cluster padrão.

3. **Executar as Células**:
   - Execute cada célula do notebook sequencialmente clicando no ícone de "play" ao lado de cada célula ou pressionando `Shift + Enter`.

Seguindo esses passos, você conseguirá executar o notebook no Databricks e integrar suas mudanças ao repositório GitHub.

<a name="id2"></a>
## 2. BASE DE DADOS

<a name="id2_1"></a>
### 2.1 Descrição

A tabela `workspace.default.vendas` contém informações sobre as vendas realizadas. Abaixo está a descrição das colunas presentes na tabela:

| Coluna       | Tipo     | Descrição                                      |
|--------------|----------|------------------------------------------------|
| id_venda     | Integer  | Identificador único para cada venda            |
| id_cliente   | Integer  | Identificador único para cada cliente          |
| id_produto   | Integer  | Identificador único para cada produto          |
| canal_venda  | String   | Canal através do qual a venda foi realizada    |
| valor        | Float    | Valor monetário da venda                       |
| data_venda   | Date     | Data em que a venda foi realizada              |

A tabela `workspace.default.produtos` contém informações sobre os produtos comercializados:

| Coluna       | Tipo     | Descrição                                      |
|--------------|----------|------------------------------------------------|
| id_produto   | Integer  | Identificador único do produto                 |
| nome_produto | String   | Nome do produto                                |
| descricao    | String   | Descrição do produto                           |
| marca        | String   | Marca do produto                               |

A tabela `clientes.csv` contém informações sobre os clientes:

| Coluna       | Tipo     | Descrição                                      |
|--------------|----------|------------------------------------------------|
| id_cliente   | Integer  | Identificador único do cliente                 |
| nome         | String   | Nome do cliente                                |
| email        | String   | Email do cliente                               |
| telefone     | String   | Telefone do cliente                            |
| cidade       | String   | Cidade do cliente                              |
| estado       | String   | Estado do cliente                              |
| data_cadastro| Date     | Data de cadastro do cliente                    |
 
## 3. METODOLOGIA
 
<a name="id3_1"></a>
### 3.1 Tratamentos realizados

Abaixo está o passo-a-passo das transformações realizadas:

1. **Extração dos Dados**
   - Os dados são extraídos de arquivos CSV e carregados em tabelas Delta: `vendas`, `produtos` e `clientes`.
   - Caso as tabelas já existam, são lidas diretamente do catálogo Delta.

2. **Unificação das Bases**
   - As tabelas são integradas em uma base única (`base_unificada_gf`) por meio de joins entre vendas, produtos e clientes, utilizando os identificadores (`id_cliente`, `id_produto`) na tabela `vendas`.

3. **Enriquecimento dos Dados**
   - Novas colunas são criadas a partir do processamento dos nomes dos produtos (coluna `nome_produto`), como `categoria`, `marca`, `modelo`, entre outras características técnicas.

   - Para a identificação da `marca`, utiliza-se uma lista predefinida com o mapeamento das marcas no catálogo de produtos atual para itendificar o token da marca na análise da coluna `nome_produto`.

   - A coluna `categoria` é construída com strings de até 2 palavras (3 se contiver stop-words) que descrevem, em termos gerais, a categoria do produto e se encontram no início da coluna `nome_produto`. São aplicadas regras para padronização e limpeza dos dados, visando garantir que, nessa análise, sejam desconsideradas as marcas e que sejam registradas somente categorias com até 2 palavras (desconsiderando stop-words).

   - A coluna `modelo` é preenchida com as informações após o token da `marca`.
   
   - Extração de Capacidade: `r"(\d+(?:,\d+)?\s?[lL])"` - Este regex extrai valores numéricos seguidos por "l" ou "L", indicando capacidade em litros.
   
   - Extração de Dimensão: `r"(\d+(?:\.\d+)?\s*(\"|”|''|pol))"` - Este regex extrai valores numéricos seguidos por caracteres de polegadas (", ”, '', pol), indicando dimensões.
   
   - Extração de Potência: `r"(\d+(?:,\d+)?\s?W)"` - Este regex extrai valores numéricos seguidos por "W", indicando potência em watts.

4. **Transformações Adicionais**
   - Conversão de tipos de dados para garantir consistência (ex: datas, valores numéricos).

   - Criação de colunas derivadas, como `ano_mes_venda` para facilitar análises temporais.

5. **Persistência**
   - A base enriquecida é salva como tabela Delta (`base_unificada_gf`), pronta para análises e visualizações.

Este pipeline garante que os dados estejam integrados, limpos e enriquecidos para apoiar análises avançadas e tomadas de decisão.
 
<a name="id3_2"></a>
### 3.2 Premissas
 
- Na tabela de origem `produtos.csv` existe a coluna `marca`, com informações a respeito da categoria do produto e incompletas, optou-se pela eliminação da coluna e pela construção de outra coluna `marca` do 0, com a lógica descrita acima.

- Na tabela de origem `cliente.csv` existe a coluna `cidade`, cujas informações não se referem a cidades brasileiras, aparentando serem parte do nome dos clientes. Além disso, os dados não são consistentes entre si, assim, optou-se pela eliminação da coluna da análise.
 
## 4. RESULTADOS
 
<a name="id4_1"></a>
### 4.1 Tabela Final

A tabela `workspace.default.base_unificada_gf` contém dados integrados de vendas, produtos e clientes e é alimentada pelo notebook Gerando_Falcoes_ETL:

| Coluna           | Tipo     | Descrição                                      |
|------------------|----------|------------------------------------------------|
| id_venda         | Integer  | Identificador da venda                         |
| canal_venda      | String   | Canal da venda                                 |
| valor            | Float    | Valor da venda                                 |
| data_venda       | Date     | Data da venda                                  |
| ano_mes_venda    | String   | Ano e mês da venda                             |
| id_produto       | Integer  | Identificador do produto                       |
| nome_produto     | String   | Nome do produto                                |
| descricao        | String   | Descrição do produto                           |
| categoria        | String   | Categoria do produto                           |
| marca            | String   | Marca do produto                               |
| modelo           | String   | Modelo do produto                              |
| capacidade       | String   | Capacidade do produto (L)                      |
| dimensao         | String   | Dimensão do produto (cm ou pol)                |
| potencia         | String   | Potência do produto (W)                        |
| id_cliente       | Integer  | Identificador do cliente                       |
| nome             | String   | Nome do cliente                                |
| email            | String   | Email do cliente                               |
| telefone         | String   | Telefone do cliente                            |
| estado           | String   | Estado do cliente                              |
| data_cadastro    | Date     | Data de cadastro do cliente                    |

















