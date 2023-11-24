

<div align="center">

<img width="250" height="250" align="center" src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/postgresql/postgresql-plain-wordmark.svg"/>

</div>
<br>
Projeto criado no intuito de listar os principais comandos do postgresql e quando devo utiliza-los

#
SELECT: Utilizado para recuperar dados de uma ou mais tabelas.
```postgresql
   SELECT * FROM nome_da_tabela;
```
#
INSERT: Usado para adicionar novos registros a uma tabela.
```postgresql
INSERT INTO nome_da_tabela (coluna1, coluna2) VALUES (valor1, valor2);
```
#
UPDATE: Utilizado para modificar os registros existentes em uma tabela.
```postgresql
UPDATE nome_da_tabela SET coluna1 = novo_valor WHERE condição;
```
#
DELETE: Usado para remover registros de uma tabela.
```postgresql
DELETE FROM nome_da_tabela WHERE condição;
```
#
CREATE: Utilizado para criar novas tabelas, índices, visões ou outros objetos de banco de dados.
```postgresql
   CREATE TABLE nome_da_nova_tabela (
       coluna1 tipo_de_dado,
       coluna2 tipo_de_dado,
       ...
   );

```
#
ALTER: Usado para modificar a estrutura de uma tabela existente.
```postgresql
ALTER TABLE nome_da_tabela ADD coluna_nova tipo_de_dado;
```
#
DROP: Utilizado para excluir tabelas, índices, visões ou outros objetos de banco de dados.
```postgresql
   DROP TABLE nome_da_tabela;
```
#
GRANT: Usado para conceder permissões a usuários e grupos de usuários.
```postgresql
GRANT tipo_de_permissao ON nome_da_tabela TO nome_do_usuario;
```

Tipo de permissões:

* SELECT: Permite selecionar dados de uma tabela.

<br>

* INSERT: Permite adicionar novos registros a uma tabela.

<br>

* UPDATE: Permite modificar os registros existentes em uma tabela.

<br>

* DELETE: Permite remover registros de uma tabela.

<br>

* REFERENCES: Permite criar uma chave estrangeira que referencia a tabela especificada.

<br>

* TRIGGER: Permite criar um gatilho (trigger) na tabela especificada.

<br>

* ALL PRIVILEGES: Concede todos os privilégios disponíveis para a tabela especificada.

<br>

#
REVOKE: Utilizado para revogar permissões previamente concedidas.
```postgresql
REVOKE DELETE ON nome_da_tabela FROM nome_do_usuario;
```
#

## Triggers

Os "triggers" (gatilhos) no PostgreSQL são procedimentos armazenados que são automaticamente executados em resposta a determinados eventos em tabelas do banco de dados. Eles podem ser acionados antes ou depois de uma operação de INSERT, UPDATE ou DELETE em uma tabela, permitindo a execução de ações personalizadas, como a validação de dados, a atualização de outras tabelas ou o registro de atividades.
Aqui está um exemplo de como criar um trigger no PostgreSQL:
Suponha que temos duas tabelas, "tabela_origem" e "tabela_destino", e queremos que sempre que um novo registro for inserido na "tabela_origem", um registro correspondente seja automaticamente inserido na "tabela_destino". Podemos fazer isso com um trigger da seguinte forma:

~~~postgresql
CREATE OR REPLACE FUNCTION after_insert_trigger_function()
RETURNS TRIGGER AS $$
BEGIN
    INSERT INTO tabela_destino (coluna1, coluna2) VALUES (NEW.valor1, NEW.valor2);
    RETURN NEW;
END;
$$ LANGUAGE plpgsql;

CREATE TRIGGER after_insert_trigger
AFTER INSERT ON tabela_origem
FOR EACH ROW
EXECUTE FUNCTION after_insert_trigger_function();
~~~

Neste exemplo, estamos criando uma função chamada "after_insert_trigger_function" que é acionada após a inserção de um novo registro na "tabela_origem". Dentro dessa função, estamos inserindo um novo registro na "tabela_destino" com base nos valores do novo registro inserido na "tabela_origem".
Depois, criamos o trigger em si, chamado "after_insert_trigger", que é acionado após a inserção de um novo registro na "tabela_origem" e executa a função "after_insert_trigger_function".
Os triggers oferecem grande flexibilidade e podem ser usados para uma variedade de finalidades, como a manutenção de registros de auditoria, a aplicação de regras de negócios complexas e a garantia de integridade dos dados. É importante usá-los com cuidado, pois podem impactar o desempenho do banco de dados se não forem bem projetados e testados.

#
## Índices

Os índices no PostgreSQL são estruturas que melhoram a velocidade de recuperação de dados em tabelas, funcionando de forma semelhante a um índice em um livro, permitindo que o banco de dados encontre linhas específicas de uma tabela de forma mais eficiente. Eles são particularmente úteis em tabelas grandes, onde a busca por registros específicos pode ser lenta sem um índice apropriado.
Os índices devem ser utilizados em colunas que são frequentemente usadas em cláusulas WHERE, JOIN e ORDER BY em consultas. Eles podem acelerar significativamente a recuperação de dados em consultas que filtram ou ordenam os resultados com base nos valores das colunas indexadas.

Aqui está um exemplo de como criar um índice no PostgreSQL:
Suponha que temos uma tabela chamada "clientes" e frequentemente realizamos consultas para buscar clientes com base em seus nomes. Podemos criar um índice na coluna "nome" da seguinte forma:

~~~postgresql
CREATE INDEX indice_nome ON clientes (nome);
~~~

Neste exemplo, estamos criando um índice chamado "indice_nome" na tabela "clientes" para a coluna "nome". Isso permitirá que consultas que envolvam a coluna "nome" sejam executadas de forma mais eficiente.
É importante notar que a criação de índices pode melhorar o desempenho das consultas de leitura, mas pode diminuir o desempenho das operações de escrita, como INSERT, UPDATE e DELETE, pois o banco de dados precisa manter os índices atualizados.

#

## Explain Analyze

O comando EXPLAIN ANALYZE no PostgreSQL é uma ferramenta poderosa para analisar o plano de execução de consultas SQL e obter informações detalhadas sobre como o PostgreSQL executa uma determinada consulta. Ele fornece estatísticas de desempenho reais, como o tempo de execução de cada etapa do plano de execução, o número de linhas processadas em cada etapa e outras informações úteis para identificar possíveis gargalos de desempenho.

~~~postgresql
EXPLAIN ANALYZE SELECT * FROM tabela_exemplo WHERE coluna = 'valor';
~~~

Ao executar essa consulta, o PostgreSQL retornará o plano de execução da consulta, juntamente com as estatísticas de desempenho. Isso pode ajudar a identificar se o PostgreSQL está usando os índices de forma eficiente, se está realizando operações de junção de forma otimizada e onde estão os principais pontos de custo na execução da consulta.

#