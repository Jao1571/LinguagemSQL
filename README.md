# Resumo Acadêmico: Banco de Dados e `SQL`
*Guia prático sobre infraestrutura, manipulação de dados e configuração de ambiente.*

---

##  1. Introdução ao Mundo dos Dados

> **O que é `SQL`?** > `SQL` significa `Linguagem de Consulta Estruturada`, e é frequentemente pronunciado como *SeQuel*. É uma linguagem utilizada para interagir com dados armazenados em bancos de dados, permitindo a execução de cálculos complexos em grandes conjuntos de informações.

**Por que essa habilidade é tão exigida?**
*  **Comunicação:** Permite falar diretamente com os sistemas de dados.
*  **Mercado:** Há uma alta demanda por essa habilidade para carreiras como Desenvolvedores de Software, Analistas, Cientistas e Engenheiros de Dados.
*  **Padrão da Indústria:** O `SQL` é usado como linguagem base com ferramentas como `Power BI`, `Tableau`, `Kafka`, `Spark` e `Synapse`.

**O `DBMS` (Sistema de Gerenciamento de Banco de Dados)**
* Atua como a interface direta entre o usuário e o próprio banco de dados.
* Estes bancos são normalmente hospedados em servidores ou na nuvem e funcionam 24/7 para que a disponibilidade dos dados seja contínua.
* Ferramentas de BI e aplicativos (APPs) enviam as suas consultas ao `DBMS` para gerenciar essas informações.

---

##  2. Tipos e Arquitetura 

Bancos de dados atendem a diferentes necessidades e podem ser divididos nas seguintes categorias:
* **Relacional**: `PostgreSQL`, `MySQL`, `Microsoft SQL Server`, `Amazon Redshift`.
* **Documento**: `MongoDB`.
* **Grafo**: `Neo4j`.
* **Chave-Valor**: `Redis`, `Amazon DynamoDB`.
* **Base de Coluna**: `Apache Cassandra`.

###  Estrutura Hierárquica (`Relacional`)
Os bancos relacionais seguem uma ordem rígida de organização:
1. **`Servidor`**: O host principal que guarda um ou mais bancos de dados.
2. **`Banco de Dados`**: O contêiner primário e de alto nível (ex: "RH" ou "Vendas").
3. **`Esquema` (`Schema`)**: Agrupamentos lógicos internos para organização (ex: "Clientes").
4. **`Tabela`**: O local físico onde a informação reside e é armazenada em disco.
   * **`Colunas`**: A categoria vertical que define o tipo de dado esperado.
   * **`Linhas`**: O registro horizontal de um dado inserido.
   * **`Célula`**: A unidade que fica na interseção de uma linha com a coluna.
   * **`Chave Primária`**: O identificador que torna cada registro único dentro da tabela.

---

##  3. Montando o Ambiente de Trabalho

Para colocar os comandos em prática, é necessário montar o ambiente. 

**Instalação do Banco:**
* O instalador do `PostgreSQL` pode ser baixado e executado no `Windows`. 
* Durante a instalação, deve-se registrar e anotar com muito cuidado a senha mestra exigida pelo superusuário (chamado `postgres`), que será cobrada em todas as conexões futuras.

**Ferramentas Gerenciadoras Gráficas:**
* **`pgAdmin` (`pgAdmin 4`)**: Geralmente fornecido e instalado em conjunto com o pacote do `PostgreSQL`, sendo a interface oficial para o gerenciamento das bases locais.
* **`DBeaver`**: Um gerenciador de banco de dados poderoso e universal. Ao contrário do `pgAdmin`, ele permite conexão com diferentes SGBDs (como `Oracle`, `MySQL` e o próprio `PostgreSQL` na mesma interface) baixando e injetando os drivers necessários instantaneamente para a conexão.

---

##  4. Tipos de Dados

Ao criar estruturas, o `SQL` exige que você defina que tipo de informação cada coluna vai segurar:

*  **Numéricos:**
  * `INT`: Utilizado para números inteiros.
  * `DECIMAL`: Utilizado para valores numéricos contendo frações.
  * `SERIAL`: Usado nativamente no `PostgreSQL` para gerar números sequenciais em sistemas de auto-incremento.
  * `MONEY`: Ocupa 64 bits e é indicado exclusivamente para armazenar valores financeiros ou monetários.
*  **Textos (Strings):**
  * `CHAR`: Usado para cadeias de strings com um comprimento fixo.
  * `VARCHAR`: Usado para cadeias de strings com comprimentos variáveis.
*  **Tempo e Lógica:**
  * `DATE`: Registra dias no formato `YYYY-MM-DD`.
  * `TIME`: Registra horas no formato `HH:MM:SS`.
  * `BOOLEAN`: Usado para registrar valores lógicos de verdadeiro ou falso (`true/false`) ocupando apenas um byte.
*  **Tipos de Rede (`PostgreSQL`):**
  * O `PostgreSQL` tem suporte avançado e especializado oferecendo tipos como o `INET` ou `CIDR` para endereçamentos de protocolos IPv4 ou IPv6, assim como suporte nativo para salvar endereços MAC (`MACADDR`) em infraestruturas.

---

##  5. Sublinguagens do Sistema

Os comandos `SQL` são quebrados em categorias dependendo de onde e como atuam.

###  `DDL` (Linguagem de Definição de Dados)
Permite gerenciar a "planta" e o esqueleto do banco. Ela define ou modifica os objetos.
* **`CREATE`**: Cria e constrói novos objetos (Bancos ou Tabelas) totalmente do zero.
  * *Exemplo: `CREATE DATABASE Sales;`*
* **`ALTER`**: Modifica a estrutura de um objeto que já existe, como adicionar uma coluna nova (`ADD`), sem que você precise excluí-lo.
* **`DROP`**: Ação destrutiva que remove uma estrutura permanentemente do banco, levando com ela todos os registros ali armazenados.

###  `DML` (Linguagem de Manipulação de Dados)
Enquanto a DDL cria o "recipiente", o DML gerencia os dados reais e o seu conteúdo interno.
* **`INSERT`**: Comando para inserir e adicionar novas linhas na tabela.
  * Pode ser executado injetando os valores manualmente com o comando `VALUES`.
  * Ou populando usando uma cópia de pesquisa atrelada com o comando `SELECT`.
* **`UPDATE`**: Comando para editar valores já gravados na tabela.
* **`DELETE`**: Comando para remover de vez um registro contido na tabela.

>  **A REGRA DE OURO DO `DML`:** Sempre utilize obrigatoriamente a cláusula `WHERE` ao executar comandos `UPDATE` e `DELETE`. Caso contrário, a alteração será feita sem filtros e você acabará atualizando ou apagando acidentalmente TODAS as linhas contidas na sua tabela.

###  DQL, DCL e TCL
* **`DQL` (Linguagem de Consulta):** O comando essencial aqui é o `SELECT`, utilizado unicamente para buscar e recuperar informações contidas no banco.
* **`DCL` (Linguagem de Controle):** Serve para o gerenciamento de permissões e do controle de acesso. Usa o `GRANT` para dar privilégios e o `REVOKE` para removê-los.
* **`TCL` (Controle de Transações):** Gerencia como as ações são salvas. Conta com o `COMMIT` (salva as modificações no banco de forma permanente), o `ROLLBACK` (retrocede as mudanças no sistema caso ocorram falhas e erros), e o `SAVEPOINT` (cria um marco de segurança para rollbacks parciais).# LinguagemSQL
