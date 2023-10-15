# Tratamento de dados com Power Query explorando noções de Cloud

Projeto desenvolvido como parte do Santander Bootcamp 2023 - Ciência de Dados com Python.

O objetivo é realizar o processamento, com Power BI, de dados provenientes de uma instância MySQL na nuvem.

Passos realizados:

1. Criação de uma instância RDS na AWS para MySQL

<img src="Img\aws.png">


> arquivos .csv foram incluídos na [pasta de Dados](Dados), caso queiram reproduzir o tratamento de dados sem a etapa de criação de uma instância de Banco de Dados.


2. Criar o banco de dados com base nos scripts

* [ddl.sql](Img\aws.png\Dados\ddl.sql): Cria o banco de dados, bem como suas tabelas e chaves primárias e estrangeiras

* [insert.sql](Img\aws.png\Dados\insert.sql): insere os dados nas tabelas

3. Integração do Power BI com MySQL no Azure

<img src="Img\Integracao-pbi.png">

4. Verificar problemas nas bases a fim de realizar a transformação dos dados e preparação para os reports analíticos

Na etapa de transformação dos dados, os seguintes passos foram realizados para cada tabela:

1. Alteração nos nomes das colunas para nomes mais intuitivos
2. Verificação dos tipos de dados: manager_id estava como texto e foi trocado para numérico, salário foi trocado para decimal fixo.
3. Verificação de dados faltantes: apenas um registro na tabela Employee foi encontrado com Manager_id nulo. Assume-se que este seja o CEO da empresa e, portanto, não tem um Manager associado, sendo desnecessário excluir o registro.
4. Verificação de registros duplicados: não existem registros duplicados
4. Verificação de outros erros: não foram encontrados outros erros nas bases
5. Substituição de siglas na variável Sex.
5. Para a tabela Employee, a coluna de endereço foi divida, já que continha o número, o logradouro, a cidade e o estado em uma mesma coluna. Uma cidade continha um símbolo de hífen "-", o mesmo utilizado para separar cada instância do endereço, por esse motivo, a divisão foi feita por etapas: primeiro, na primeira ocorrência do separador, e depois, sempre a partir do separador mais à direita.
6. Esconder campos que não serão usados no relatório, mas que são necessários nas tabelas (como ids)
7. Excluir campos desnecessários para as análises a serem realizadas

Além disso, conforme instruções do projeto, outras manipulações foram realizadas:
* as informações do nome do departamento de cada empregado foram mescladas na tabela employee
* As colunas de first name, middle name inital e last name do empregado foram combinadas em uma só (Employee Name) e excluídas
* as informações do nome do gerente foram incluídas na tabela employee
* Criação de uma nova tabela de departamento, mesclada com dept-location e com department-location como chave. A opção por mesclar foi necessária pois foi preciso buscar as informações da outra tabela.
* Criação de uma tabela com o agrupamento de número de colaboradores por Manager

## Relacionamento entre tabelas

Employee.Department_id (*)---(1) Departament.Department_id 

Departament.Department_id (1)---(*) Dept_locations (inativa, pois levaria a um relacionamento many-to-many com Employee)

Employee.Employee_id (1)---(*) Dependent.Employee_id

Employee.Employee_id (1)---(*) works_on.Employee_id

works_on.Project_id (*) --- (1) Project.Project_id

Project.Department_id (*)---(1) Departament.Department_id (inativa, pelo relacionamento circular com Employee)

## Report (Work in Progress)
Como o foco foi o tratamento de dados em si, a parte do report foi feita apenas com alguns dados, sem muitos detalhes. Pretende-se concluir a parte visual posteriormente.
