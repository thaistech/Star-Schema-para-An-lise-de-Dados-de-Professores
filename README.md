# Star Schema para Análise de Dados de Professores

Este repositório contém a implementação de um **Star Schema** para análise de dados relacionados a professores, cursos, departamentos e disciplinas, utilizando MySQL e MySQL Workbench. O objetivo do modelo é permitir consultas analíticas eficientes sobre as interações entre os professores, cursos que oferecem, os departamentos aos quais pertencem, as disciplinas que ministram, e a distribuição temporal desses cursos.

## Diagrama Dimensional Star Schema

O **Star Schema** é um modelo de dados usado em sistemas de **Data Warehouse**, no qual uma tabela **Fato** centralizada está conectada a várias tabelas de **Dimensões**. Neste caso, o foco está em analisar dados dos **professores** e suas atividades acadêmicas. O modelo foi projetado com base em um banco de dados relacional com informações sobre:

- Professores
- Cursos
- Departamentos
- Disciplinas
- Datas

## Estrutura do Banco de Dados

### Tabelas de Dimensão

As tabelas de **dimensão** fornecem contexto sobre as entidades analisadas. Cada uma contém dados descritivos que são utilizados para filtrar e agrupar as métricas.

1. **Dim_Professor**: Contém informações sobre os professores.
   - ID_Professor (Chave Primária)
   - Nome_Professor
   - Titulo
   - Ano_Ingresso
   - Area_Atuacao

2. **Dim_Departamento**: Contém informações sobre os departamentos acadêmicos.
   - ID_Departamento (Chave Primária)
   - Nome_Departamento
   - Faculdade
   - Localizacao_Departamento

3. **Dim_Disciplina**: Contém informações sobre as disciplinas oferecidas pelos professores.
   - ID_Disciplina (Chave Primária)
   - Nome_Disciplina
   - Descricao_Disciplina
   - Departamento_Associado (Chave estrangeira para Dim_Departamento)

4. **Dim_Curso**: Contém informações sobre os cursos oferecidos pela instituição.
   - ID_Curso (Chave Primária)
   - Nome_Curso
   - Nivel_Curso
   - Duracao
   - Departamento_Associado (Chave estrangeira para Dim_Departamento)

5. **Dim_Data**: Contém informações sobre as datas de oferta dos cursos e disciplinas.
   - ID_Data (Chave Primária)
   - Data_Completa
   - Ano
   - Mes
   - Dia
   - Trimestre
   - Dia_Semana
   - Semestre

### Tabela Fato

A **tabela fato** centraliza as medidas de análise e está conectada às tabelas de dimensões:

- **Fato_Professor_Curso**: Contém informações sobre a carga horária, número de aulas e rendimento dos cursos oferecidos pelos professores.
  - ID_Professor (Chave estrangeira para Dim_Professor)
  - ID_Curso (Chave estrangeira para Dim_Curso)
  - ID_Departamento (Chave estrangeira para Dim_Departamento)
  - ID_Disciplina (Chave estrangeira para Dim_Disciplina)
  - ID_Data (Chave estrangeira para Dim_Data)
  - Carga_Horaria
  - Numero_Aulas
  - Rendimento_Curso

## Como Usar

### Requisitos

- **MySQL**: Você precisará do MySQL instalado em sua máquina para criar e gerenciar o banco de dados.
- **MySQL Workbench**: Ferramenta visual para modelagem e administração do banco de dados.

### Passos para Criar o Banco de Dados

1. Clone este repositório:
    ```bash
    git clone https://github.com/seu-usuario/star-schema-professores.git
    cd star-schema-professores
    ```

2. Crie o banco de dados e as tabelas no MySQL:
    - Abra o MySQL Workbench e conecte-se ao seu servidor MySQL.
    - Execute os scripts de criação de tabelas no MySQL Workbench.

    ```sql
    CREATE DATABASE StarSchema_Professor;
    USE StarSchema_Professor;
    ```

    - Copie e cole o código SQL das tabelas de dimensão e da tabela fato para criar a estrutura do banco de dados.

3. Insira os dados de exemplo nas tabelas de dimensão e na tabela fato. Isso pode ser feito executando os scripts de inserção fornecidos neste repositório.

### Consultas de Exemplo

Após a criação das tabelas e inserção dos dados, você pode realizar consultas analíticas. Aqui estão alguns exemplos:

1. **Consulta: Carga horária média por departamento**

    ```sql
    SELECT d.Nome_Departamento, AVG(f.Carga_Horaria) AS Media_Carga_Horaria
    FROM Fato_Professor_Curso f
    JOIN Dim_Departamento d ON f.ID_Departamento = d.ID_Departamento
    GROUP BY d.Nome_Departamento;
    ```

2. **Consulta: Número de aulas por professor e curso**

    ```sql
    SELECT p.Nome_Professor, c.Nome_Curso, SUM(f.Numero_Aulas) AS Total_Aulas
    FROM Fato_Professor_Curso f
    JOIN Dim_Professor p ON f.ID_Professor = p.ID_Professor
    JOIN Dim_Curso c ON f.ID_Curso = c.ID_Curso
    GROUP BY p.Nome_Professor, c.Nome_Curso;
    ```

### Diagrama EER

O diagrama **Enhanced Entity-Relationship (EER)** foi criado no MySQL Workbench para representar visualmente o **Star Schema**. O diagrama mostra as tabelas de **dimensões** conectadas à tabela **fato**, facilitando a compreensão da estrutura e dos relacionamentos entre as entidades.

### Contribuindo

Se você deseja contribuir com melhorias, correções ou novos recursos, sinta-se à vontade para abrir uma **issue** ou enviar um **pull request**. Agradecemos por sua contribuição!

## Licença
