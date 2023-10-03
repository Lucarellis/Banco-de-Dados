# Banco-de-Dados
--comando para seleção 
SELECT * FROM produtos ORDER BY id ASC; --		<-- ordem da tabela

-- Comandos DDL (Data Definition Language) linguagem de definição de Dados (Interagir com a tabela inteira)
-- Criação de tabela
CREATE TABLE produtos(
	id INT PRIMARY KEY NOT NULL,
	nome VARCHAR(255),
	preco DECIMAL(10,2)
);

-- Excluir uma tabela
DROP TABLE produtos;

--alterar tabela, adicionar coluna
ALTER TABLE produtos ADD descricao TEXT;
--excluir coluna
ALTER TABLE produtos DROP descricao;

--DML - Linguagem de manipilação de dados (mexer com linha)

--Inserir dado na tabela
INSERT INTO produtos(id, nome, preco) VALUES
(1, 'Nescau', 8.00),
(2, 'Yakult Chamyto', 4.00),
(3, 'Alpino Dark', 5.00),
(4, 'Miojo Monica', 2.00);

-- Alterar dado da tabela
UPDATE produtos SET preco = 15.00 WHERE id=1;
UPDATE produtos SET nome = 'Toddy' WHERE nome LIKE 'Nescau';
UPDATE produtos SET descricao = 'Armazenar em ambiente gelado' WHERE id=2 or id=3;
UPDATE produtos SET descricao = 'promoção'
WHERE preco < 5.00;

--Deletar dado de uma tabela
DELETE FROM produtos WHERE nome LIKE 'Alpino Dark';
DELETE FROM produtos WHERE id = 1;

--Comandos DQL (Linguagem de Consulta de Dados)
--Selecionar todos(*) produtos, ordene ascendente
SELECT * FROM produtos ORDER BY id ASC;

--Ordenar pelo preço CRESCENTE
SELECT * FROM produtos ORDER BY preco ASC;

--Ordenar pelo preço decrescente
SELECT * FROM produtos ORDER BY preco DESC;

--Selecionar Colunas
SELECT nome, preco FROM produtos ORDER BY id;

--Selecionar produtos maior que R$4,00
SELECT preco, nome FROM produtos WHERE preco > 4.00;

--Selecionar produto mais caro
SELECT MAX(preco) AS maior_valor FROM produtos;
SELECT preco, nome FROM produtos ORDER BY preco DESC LIMIT 1;

--simulando busca por nome exato
SELECT * FROM produtos WHERE nome LIKE 'Nescau';

--Busca pelos primeiros caracteres, o resto não importa
SELECT * FROM produtos WHERE nome LIKE 'Ne%';
--A parte anterior não importa, busca o ultimo
SELECT * FROM produtos WHERE nome LIKE '%fit';
--Busca qualquer parte do nome 
SELECT * FROM produtos WHERE nome LIKE '%Nestle%';


-- LEVEL 2 --------------------------------------
--Criando tabela Funcionário---------------------
CREATE TABLE funcionario( 
	id SERIAL PRIMARY KEY,
	nome VARCHAR(255) NOT NULL,
	data_nasc DATE,
	sexo CHAR(1),
	salario DECIMAL(10,2),
	ativo BOOLEAN
);

SELECT * FROM funcionario;

INSERT INTO funcionario(nome, data_nasc,sexo,salario,ativo) VALUES
('Bob', '1990-05-15','M', 2000.00, true),
('Augusto', '1970-04-17', 'M', 1500.00, false),
('Joanir', '2000-01-01', 'F', 1800.00, false),
('Elisa', '1995-10-02', 'F', 1900.00, true);
SELECT * FROM funcionario;

--Seleciona funcionários que nasceram após 01-01-1992 
SELECT * FROM funcionario WHERE data_nasc > '1992-01-01';

--Selciona funcionários em um período
SELECT nome, data_nasc FROM funcionario
WHERE data_nasc BETWEEN '1990-01-01' AND '1997-01-01';

--Contabilizar
SELECT sexo, 
COUNT(*) AS total_pessoas,   	       --Contar
AVG(salario) AS media_salarial,        --Média
FROM funcionario GROUP BY sexo;

--seleciona funcionario inativo
SELECT * FROM funcionario WHERE ativo = false;

-- 1.Criar uma tabela "clientes" com campos para ID, nome e Email
-- 2.Adicionar uma coluna "Telefone" à tabela "Clientes" usando ALTER TABLE
-- 3.Remover a coluna "Email" da tabela "clientes" usando ALTER TABLE
-- 4.Criar uma tabela "Item" com campos para ID, Nome e preço
-- 5.Inserir um novo cliente na tabela "clientes" insert
-- 6.Inserir 3 novos itens na tabela "itens" INSERT
-- 7.Atualizar o nome de um cliente na tabela "itens" UPDATE
-- 8.Atualizar o preco de um item na tabela "itens" UPDATE
-- 9.Excluir um cliente da tabela "clientes" DELETE
-- 10.Excluir um item da tabela "itens" DELETE
SELECT * FROM clientes;
CREATE TABLE clientes(
	id SERIAL PRIMARY KEY,
	nome VARCHAR(255),
	email VARCHAR(255)
);

ALTER TABLE clientes ADD telefone VARCHAR(20);

ALTER TABLE clientes DROP email;

INSERT INTO clientes(id, nome, telefone) VALUES
(13, 'Gerisvaldo', '(44)4002-8922');

UPDATE clientes SET nome = 'Júlio' WHERE nome LIKE 'Gerisvaldo';

DELETE FROM clientes WHERE id = 12;

SELECT * FROM item ORDER BY id ASC;
CREATE TABLE item(
	id INT PRIMARY KEY,
	nome VARCHAR(255),
	preco DECIMAL(10,2)
);

INSERT INTO item(id, nome, preco) VALUES
(1, 'Arroz', 8.00),
(2, 'Feijão', 4.00),
(3, 'Macarrão', 7.50);

UPDATE item SET preco = 9.00 WHERE id=1;

DELETE FROM item WHERE id = 1;


--Entendendo relacionamentos entre tabelas no banco de dados
 
--criação das tabelas
CREATE TABLE estados(
	id_estado SERIAL PRIMARY KEY,
	nome_estado Varchar(50) NOT NULL,
	sigla CHAR(2)
);

CREATE TABLE cidades(
	id_cidade SERIAL PRIMARY KEY,		--PK chave primária
	nome_cidade VARCHAR(50),
	cep VARCHAR(50),
	id_estado INT REFERENCES estados(id_estado)		--FK chave estrangeira
);

--vizualizando tabelas
SELECT * FROM estados;
SELECT * FROM cidades;

--Inserindo Dados Nas Tabelas
INSERT INTO estados(id_estado, nome_estado, sigla) VALUES
(1, 'Paraná', 'PR'),
(2, 'São Paulo', 'SP'),
(3, 'Minas Gerais', 'MG');

INSERT INTO cidades(id_cidade, nome_cidade, cep, id_estado) VALUES
(10, 'Nova Londrina','87970-000', 1),
(11, 'Marilena', '87960-000', 1),
(12, 'Itaúna', '87980-000', 1),
(13, 'Primavera', '19273-000', 2),
(14, 'Rosana', '19274-000', 2),
(15, 'Cachoeira da Prata', '35765-000', 3);

--SELECINAR TODAS AS CIDADES  E SEUS ESTADOS
SELECT cidades.nome_cidade, estados.nome_estado
FROM estados INNER JOIN cidades
ON cidades.id_estado = estados.id_estado;

--SELECIONA TODAS AS CIDADES DO PARANÁ
SELECT cidades.nome_cidade, estados.sigla
FROM estados INNER JOIN cidades
ON cidades.id_estado = estados.id_estado
WHERE estados.sigla LIKE 'PR'
ORDER BY cidades.nome_cidade ASC;

--SELECIONA OS ESTADOS COM MAIS CIDADES
SELECT estados.nome_estado, COUNT(cidades.id_cidade) AS total_cidades
FROM estados INNER JOIN cidades
ON estados.id_estado = cidades.id_estado
GROUP BY estados.nome_estado
ORDER BY total_cidades DESC;

--CRIANDO MAIS UMA TABELA PARA O RELACIONAMENTO DE CIDADE-ESTADO
CREATE TABLE pessoa(
	id_pessoa SERIAL PRIMARY KEY,
	nome_pessoa VARCHAR(60),
	data_nascimento DATE,
	sexo CHAR(1),
	estado_civil VARCHAR(60),
	profissao VARCHAR(60),
	id_cidade INT REFERENCES cidades(id_cidade)
);

INSERT INTO pessoa (id_pessoa, nome_pessoa, data_nascimento, sexo, estado_civil, profissao, id_cidade)
VALUES
(1, 'Marcele', '2000-01-01', 'F', 'Solteira', 'Arquiteto', 10),
(2, 'Ananias', '1980-02-20', 'M', 'Casado', 'Carpinteiro', 10),
(3, 'Silvio', '1950-10-10', 'M', 'Casado', 'Dublador', 11),
(4, 'Léo', '2001-01-02', 'M', 'Casado', 'Entregador', 11),
(5, 'Chris', '1990-05-05', 'M', 'Solteiro', 'Fisiculturista', 12),
(6, 'Ana', '1997-04-01', 'F', 'Solteira', 'Cantora', 13),
(7, 'Jaime', '1970-03-01', 'M', 'Casado', 'Carteiro', 14),
(8, 'Alice', '2002-01-10', 'F', 'Solteira', 'Advogada', 15),
(9, 'Valentina', '1995-05-03', 'F', 'Solteira', 'Zootecnista', 15),
(10, 'Laura', '1998-05-09', 'F', 'Solteira', 'Balconista', 15);

SELECT * FROM pessoa;

--Selecionar pessoa com sua cidade (relacionando tabela pessoa com cidade)
SELECT pessoa.nome_pessoa, cidades.nome_cidade
FROM pessoa INNER JOIN cidades
ON pessoa.id_cidade = cidades.id_cidade;

--Seleciona estado, cidade, pessoa
SELECT pessoa.nome_pessoa, cidades.nome_cidade, estados.nome_estado
FROM pessoa INNER JOIN cidades
ON pessoa.id_cidade = cidades.id_cidade
INNER JOIN estados
ON cidades.id_estado = estados.id_estado;

--Seleciona pessoas do estado do Paraná
SELECT pessoa.nome_pessoa, estados.nome_estado, cidades.nome_cidade
FROM estados INNER JOIN cidades
ON estados.id_estado = cidades.id_estado
INNER JOIN pessoa
ON cidades.id_cidade = pessoa.id_cidade
WHERE estados.nome_estado LIKE 'Paraná';

--Seleciona o nome da pessoa, profissão e qual cidade pertence
SELECT pessoa.nome_pessoa, cidades.nome_cidade, pessoa.profissao
FROM pessoa INNER JOIN cidades
ON pessoa.id_cidade = cidades.id_cidade;

--Selecionar cidade com mais mulher/homem
SELECT cidades.nome_cidade, COUNT(*) AS Quantidade
FROM cidades INNER JOIN pessoa
ON cidades.id_cidade = pessoa.id_cidade
WHERE pessoa.sexo = 'F'
GROUP BY cidades.id_cidade
ORDER BY Quantidade desc;


--1 - Selecione todas as pessoas
SELECT pessoa.nome_pessoa FROM pessoa;

--2 - Selecione todas pessoas do sexo masculino "M"
SELECT pessoa.nome_pessoa FROM pessoa
WHERE pessoa.sexo = 'M';

--3 - Selecione todas as pessoas com estado civil solteiro
SELECT pessoa.nome_pessoa, pessoa.estado_civil FROM pessoa
WHERE pessoa.estado_civil LIKE 'solteiro'
OR pessoa.estado_civil LIKE 'solteira';

--4 - Selecione as pessoas e sua profissão
SELECT pessoa.nome_pessoa, pessoa.profissao FROM pessoa;

--5 - Selecione as pessoas que nasceram entre 1980-01-01 e 2000-01-01
SELECT pessoa.nome_pessoa, pessoa.data_nascimento
FROM pessoa WHERE data_nascimento BETWEEN '1980-01-01' AND '2000-01-01';

--6 - Selecione as pessoas do estado de São Paulo
SELECT pessoa.nome_pessoa, estados.nome_estado, cidades.nome_cidade
FROM estados INNER JOIN cidades
ON estados.id_estado = cidades.id_estado
INNER JOIN pessoa
ON cidades.id_cidade = pessoa.id_cidade
WHERE estados.nome_estado LIKE 'São Paulo';
