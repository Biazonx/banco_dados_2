# Banco de Dados 2

## Questão 1 
Liste todos os alunos cadastrados.

  SELECT nome from aluno

## Questão 2
Mostre apenas o nome e o curso dos alunos.

SELECT nome,curso from aluno

## Questão 3
Liste os alunos do curso de Computacao.

SELECT nome from aluno WHERE curso = 'Computacao'

## Questão 4
Liste os alunos que moram em Maringa.

SELECT nome from aluno WHERE cidade = 'Maringa'

## Questão 5
Mostre os alunos ordenados pelo nome em ordem alfabética.

SELECT nome from aluno order by nome

## Questão 6
Mostre os alunos ordenados pelo ano de ingresso, do mais antigo para o mais recente.

SELECT nome from aluno order by ano_ingresso

## Questão 7
Liste os alunos que ingressaram a partir de 2022.

SELECT nome from aluno where ano_ingresso >= 2022

## Questão 8
Liste os alunos cujo nome começa com a letra A.

SELECT nome from aluno where nome like 'A%'

## Questão 9
Liste os alunos dos cursos Computacao ou Engenharia.

SELECT nome from aluno where curso = 'Computacao' Or 'Engenharia'

## Questão 10
Liste as disciplinas com carga horária entre 60 e 80 horas.

SELECT nome from disciplina where carga_horaria BETWEEN '60' and '80'

## Questão 11
Conte quantos alunos existem cadastrados.

SELECT COUNT(id) FROM aluno 

## Questão 12
Calcule a média das notas da tabela matricula.

SELECT AVG(nota) from matricula 

## Questão 13
Mostre a maior nota registrada.

SELECT MAX(nota) from matricula

## Questão 14
Mostre a menor nota registrada.

SELECT MIN(nota) from matricula

## Questão 15
Calcule a soma das cargas horárias de todas as disciplinas.

SELECT SUM(carga_horaria) from disciplina

## Questão 16
Mostre a quantidade de alunos por curso.

SELECT curso,COUNT(id) from aluno GROUP by curso

## Questão 17
Mostre a quantidade de alunos por cidade.

SELECT cidade,COUNT(id) from aluno GROUP by cidade

## Questão 18
Mostre a média das notas por situação da matrícula.

SELECT situacao,AVG(nota) from matricula GROUP by situacao

## Questão 19
Mostre quantas matrículas existem por semestre.

SELECT semestre,count(aluno_id) from matricula GROUP by semestre

## Questão 20
Mostre os cursos que possuem mais de 1 aluno cadastrado.

SELECT curso from aluno GROUP by curso having COUNT(id) > 1

## Questão 21

Liste o nome dos alunos e a situação de suas matrículas.

SELECT a.id, a.nome, m.situacao
FROM aluno a
JOIN matricula m ON a.id = m.aluno_id

## Questão 22

Liste o nome dos alunos e o nome das disciplinas em que estão matriculados.

## Questão 23

Liste o nome do aluno, o nome da disciplina e a nota.

## Questão 24

Liste apenas os alunos matriculados em disciplinas do departamento Computacao.

## Questão 25

Mostre o nome dos alunos que tiveram matrícula com situação Reprovado.

## Questão 26

Mostre o nome dos alunos de Computacao e as disciplinas que eles cursaram.

## Questão 27

Mostre a média de notas por aluno.

## Questão 28

Mostre a quantidade de disciplinas cursadas por cada aluno.

## Questão 29

Liste os alunos cuja média de notas foi maior que 8.

## Questão 30

Mostre o departamento e a quantidade de matrículas em disciplinas de cada departamento.
