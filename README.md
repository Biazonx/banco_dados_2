# Banco de Dados 2

SELECT nome from aluno

SELECT nome,curso from aluno

SELECT nome from aluno WHERE curso = 'Computacao'

SELECT nome from aluno WHERE cidade = 'Maringa'

SELECT nome from aluno order by nome

SELECT nome from aluno order by ano_ingresso

SELECT nome from aluno where ano_ingresso >= 2022

SELECT nome from aluno where nome like 'A%'

SELECT nome from aluno where curso = 'Computacao' Or 'Engenharia'

SELECT nome from disciplina where carga_horaria BETWEEN '60' and '80'
