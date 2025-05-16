
# Construindo um Esquema Conceitual para Banco De dados

📊 Descrição do Desafio:

Agora você irá criar um esquema conceitual do zero. 
A partir da narrativa fornecida você será capaz de criar todas as entidades, relacionamentos e atributos. 
Caso encontre algo que não foi definido na narrativa, utilize a sua compreensão do contexto e 
deixe uma descrição no README do seu github. para verificação.

## Introdução

O projeto conceitual proposto tem como objetivo modelar o sistema de **controle e gerenciamento de ordens de serviço (OS)**
em uma oficina mecânica. A modelagem foi elaborada com base em uma análise de requisitos extraída de uma narrativa funcional 
do processo da oficina.
O foco do sistema está em organizar e estruturar as informações relacionadas a **clientes, veículos, equipes de mecânicos, 
serviços prestados e peças utilizadas**, garantindo o acompanhamento eficiente da execução das ordens de serviço, desde a emissão até a conclusão.

## Regras de Negócios

- Um cliente pode ser pessoa física (PF) ou pessoa jurídica (PJ), **mas não pode ser os dois simultaneamente**.
- Um cliente pode cadastrar **mais de uma forma de pagamento**, permitindo flexibilidade na escolha ao finalizar um pedido.
- Cada pedido possui uma **entrega associada com status e código de rastreamento**.
- Um produto pode estar armazenado em **diferentes locais de estoque**, com controle de quantidade por unidade armazenada.
- Os produtos podem ser disponibilizados tanto por fornecedores diretos quanto por vendedores terceiros.


## Objetivo

Cria o esquema conceitual para o contexto de oficina com base na narrativa fornecida

## Narrativa

- Sistema de controle e gerenciamento de execução de ordens de serviço em uma oficina mecânica
- Clientes levam veículos à oficina mecânica para serem consertados ou para passarem por revisões  periódicas
- Cada veículo é designado a uma equipe de mecânicos que identifica os serviços a serem executados e preenche uma OS com data de entrega.
- A partir da OS, calcula-se o valor de cada serviço, consultando-se uma tabela de referência de mão-de-obra
- O valor de cada peça também irá compor a OSO cliente autoriza a execução dos serviços
- A mesma equipe avalia e executa os serviços
- Os mecânicos possuem código, nome, endereço e especialidade
- Cada OS possui: n°, data de emissão, um valor, status e uma data para conclusão dos trabalhos.

---

##  Entidades e Justificativas

| Entidade         | Justificativa                                                                 |
|------------------|--------------------------------------------------------------------------------|
| **Cliente**       | Representa a pessoa que solicita os serviços da oficina.                      |
| **Veículo**       | Cada cliente pode ter um ou mais veículos associados.                         |
| **Mecânico**      | Contém os dados dos funcionários que executam os serviços.                    |
| **Equipe**        | Agrupamento de mecânicos que trabalham juntos nas OS.                         |
| **OrdemServico**  | Registra o serviço solicitado para um veículo específico.                     |
| **Serviço**       | Lista de serviços disponíveis com valor de referência.                        |
| **Peça**          | Peças que podem ser usadas na OS, com valor unitário.                         |
| **Equipe_has_Mecânico** | Relaciona muitos-para-muitos entre equipes e mecânicos.           |
| **OS_has_Serviço** | Relaciona os serviços prestados em cada OS.                                  |
| **OS_has_Peca**    | Relaciona as peças utilizadas em cada OS.                                    |

---

##  Relacionamentos e Cardinalidades

| Relacionamento           | Entidades Envolvidas         | Cardinalidade       | Descrição                                                                 |
|--------------------------|------------------------------|---------------------|---------------------------------------------------------------------------|
| Cliente - Veículo        | Cliente ↔ Veículo             | 1:N                 | Um cliente pode ter vários veículos.                                     |
| Equipe - Mecânico        | Equipe ↔ Mecânico             | N:M                 | Uma equipe pode ter vários mecânicos e vice-versa.                       |
| Veículo - OrdemServico   | Veículo ↔ OrdemServico        | 1:N                 | Um veículo pode ter várias ordens de serviço.                            |
| Equipe - OrdemServico    | Equipe ↔ OrdemServico         | 1:N                 | Uma equipe pode executar várias OS.                                      |
| OrdemServico - Serviço   | OrdemServico ↔ Serviço        | N:M (via OS_has_Servico) | Uma OS pode ter vários serviços e vice-versa.                       |
| OrdemServico - Peça      | OrdemServico ↔ Peça           | N:M (via OS_has_Peca)    | Uma OS pode utilizar várias peças.                                   |


---

##  Entidades, Atributos e Tipos de Dados

| Entidade            | Atributo               | Tipo de Dado        | Descrição                                              |
|---------------------|------------------------|----------------------|--------------------------------------------------------|
| **Cliente**         | idCliente              | INT (PK)             | Identificador único do cliente                         |
|                     | nome                   | VARCHAR(100)         | Nome completo do cliente                               |
|                     | telefone               | VARCHAR(20)          | Telefone de contato                                    |
|                     | endereco               | VARCHAR(150)         | Endereço completo                                      |
|                     | email                  | VARCHAR(100)         | E-mail do cliente (opcional)                          |
| **Veículo**         | idVeiculo              | INT (PK)             | Identificador único do veículo                         |
|                     | modelo                 | VARCHAR(50)          | Modelo do veículo                                      |
|                     | placa                  | VARCHAR(10)          | Placa do veículo                                       |
|                     | ano                    | INT                  | Ano de fabricação                                      |
|                     | Cliente_idCliente      | INT (FK)             | Referência ao cliente                                  |
| **Mecânico**        | idMecanico             | INT (PK)             | Identificador único do mecânico                        |
|                     | nome                   | VARCHAR(100)         | Nome completo                                          |
|                     | endereco               | VARCHAR(150)         | Endereço                                               |
|                     | especialidade          | VARCHAR(100)         | Área de especialização do mecânico                     |
| **Equipe**          | idEquipe               | INT (PK)             | Identificador único da equipe                          |
|                     | nomeEquipe             | VARCHAR(50)          | Nome da equipe (opcional)                              |
| **Equipe_has_Mecanico** | Equipe_idEquipe     | INT (PK, FK)         | FK para equipe                                         |
|                     | Mecanico_idMecanico    | INT (PK, FK)         | FK para mecânico                                       |
| **OrdemServico**    | idOS                   | INT (PK)             | Número da ordem de serviço                             |
|                     | dataEmissao            | DATE                 | Data em que foi emitida a OS                           |
|                     | status                 | VARCHAR(30)          | Status atual (aberta, em execução, finalizada, etc.)   |
|                     | dataConclusao          | DATE                 | Previsão de conclusão dos serviços                     |
|                     | valorTotal             | DECIMAL(10,2)        | Valor total da OS                                      |
|                     | Veiculo_idVeiculo      | INT (FK)             | FK para o veículo relacionado                          |
|                     | Equipe_idEquipe        | INT (FK)             | FK para equipe responsável                             |
| **Servico**         | idServico              | INT (PK)             | Identificador do serviço                               |
|                     | descricao              | VARCHAR(100)         | Descrição do serviço                                   |
|                     | valorUnitario          | DECIMAL(10,2)        | Valor base da mão de obra                              |
| **OS_has_Servico**  | OrdemServico_idOS      | INT (PK, FK)         | FK para a ordem de serviço                             |
|                     | Servico_idServico      | INT (PK, FK)         | FK para o serviço                                      |
|                     | quantidade             | INT                  | Quantidade de vezes que o serviço foi aplicado         |
| **Peca**            | idPeca                 | INT (PK)             | Identificador da peça                                  |
|                     | nome                   | VARCHAR(100)         | Nome da peça                                           |
|                     | precoUnitario          | DECIMAL(10,2)        | Valor da peça                                          |
| **OS_has_Peca**     | OrdemServico_idOS      | INT (PK, FK)         | FK para a ordem de serviço                             |
|                     | Peca_idPeca            | INT (PK, FK)         | FK para a peça                                         |
|                     | quantidade             | INT                  | Quantidade usada da peça na OS                         |


##  Considerações da Modelagem


- O relacionamento N:N entre **Equipe e Mecânico** permite que um mecânico participe de múltiplas equipes, conforme a necessidade da oficina.
- Os relacionamentos N:N entre **OrdemServico com Servico e Peça** permitem listar detalhadamente todos os serviços e peças utilizados em cada OS.
- O atributo `autorizado` na OS assegura controle sobre a execução apenas mediante consentimento do cliente.
- O campo `valorTotal` da OS pode ser derivado da soma dos serviços e peças associados.
- Todos os relacionamentos possuem integridade referencial garantida por **chaves estrangeiras (FK)**.

---


##  Diagrama

![Diagrama de Entidade-Relacionamento](diagrama.png)
