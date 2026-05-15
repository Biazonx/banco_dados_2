# Concorrência, bloqueios e problemas clássicos em transações

## 6. Atividade prática

### Atividade: simular concorrência, bloqueios, espera e inconsistências em transações

#### Etapa 1. Criar o banco de teste

```sql
DROP TABLE IF EXISTS contas;

CREATE TABLE contas (
    id INT PRIMARY KEY,
    titular VARCHAR(100),
    saldo DECIMAL(10,2)
);

INSERT INTO contas (id, titular, saldo) VALUES
(1, 'Ana', 1000.00),
(2, 'Bruno', 500.00),
(3, 'Carlos', 300.00),
(4, 'Daniela', 800.00);

SELECT * FROM contas;
```

**Pergunta 1**  
Qual é a finalidade de manter dados iniciais conhecidos antes dos testes de concorrência?
A finalidade de manter dados iniciais conhecidos é facilitar a análise dos resultados dos testes, permitindo identificar claramente as alterações causadas pelas transações concorrentes.

**Pergunta 2**  
Por que é importante que a tabela esteja em um estado consistente antes do início dos experimentos?
É importante que a tabela esteja consistente para garantir que os experimentos comecem sem erros ou dados incorretos, permitindo observar corretamente os efeitos da concorrência.

---

**Pergunta 3**  
O que aconteceu com a operação realizada na Sessão 2?
A operação da Sessão 1 ficou bloqueada aguardando a liberação do registro pela Sessão 1. Ela só continuou após o COMMIT da primeira sessão.

**Pergunta 4**  
Por que a segunda sessão precisou aguardar?
A segunda sessão precisou aguardar porque a linha com id = 1 estava bloqueada pela Sessão 1, evitando modificações simultâneas no mesmo dado.

**Pergunta 5**  
Qual é a função do comando `FOR UPDATE` nesse experimento?
O FOR UPDATE bloqueia os registros selecionados para edição exclusiva até o final da transação, impedindo alterações concorrentes.

---

**Pergunta 6**  
Por que, nesse caso, as duas transações tendem a coexistir sem espera significativa?
As transações coexistem sem espera significativa porque estão alterando registros diferentes da tabela, sem conflito direto entre os bloqueios.

**Pergunta 7**  
O que esse comportamento revela sobre bloqueios em nível de linha?
Esse comportamento mostra que os bloqueios em nível de linha permitem maior concorrência, já que apenas as linhas afetadas ficam bloqueadas.

---

**Pergunta 8**  
Qual era o objetivo de consultar o mesmo registro em outra sessão antes do `COMMIT`?
O objetivo era verificar se outra sessão conseguiria visualizar alterações ainda não confirmadas pela transação original.

**Pergunta 9**  
Como esse experimento se relaciona com o conceito de isolamento?
Esse experimento demonstra o isolamento, pois alterações não confirmadas normalmente não ficam visíveis para outras transações.

---

**Pergunta 10**  
O valor lido na Sessão 1 permaneceu o mesmo ou mudou?
O valor pode permanecer o mesmo ou mudar dependendo do nível de isolamento configurado no banco de dados.
**Pergunta 11**  
Que tipo de fenômeno esse teste procura identificar?
Esse teste procura identificar o fenômeno da leitura não repetível (non-repeatable read), em que uma mesma consulta retorna valores diferentes dentro da mesma transação.
---

**Pergunta 12**  
Por que operações concorrentes sobre o mesmo registro exigem maior controle?
Operações concorrentes sobre o mesmo registro exigem maior controle para evitar conflitos, perda de dados e inconsistências causadas por alterações simultâneas.

**Pergunta 13**  
Que inconsistência pode surgir quando duas transações tentam atualizar o mesmo dado quase ao mesmo tempo?
Pode surgir o problema de atualização perdida (lost update), em que a alteração de uma transação sobrescreve a alteração realizada por outra.

---

**Pergunta 14**  
Qual evidência mostra que havia um bloqueio ativo sobre o registro?
A evidência do bloqueio é que a operação da Sessão 2 ficou aguardando sem concluir enquanto a Sessão 1 mantinha o lock ativo.

**Pergunta 15**  
Por que a liberação do lock depende do fim da transação?
A liberação do lock depende do fim da transação porque o banco precisa garantir que nenhuma outra operação altere o dado antes da confirmação (COMMIT) ou cancelamento (ROLLBACK).

---

**Pergunta 16**  
Por que a segunda leitura com `FOR UPDATE` não pôde prosseguir imediatamente?
A segunda leitura com FOR UPDATE não pôde prosseguir porque a primeira transação já possuía um bloqueio exclusivo sobre o registro.

**Pergunta 17**  
Em que essa situação difere de uma consulta `SELECT` comum?
Uma consulta SELECT comum normalmente apenas lê os dados sem bloquear alterações. Já o FOR UPDATE cria um lock para impedir modificações concorrentes.

---

**Pergunta 18**  
Qual seria o saldo correto ao final, caso ambas as operações fossem consideradas corretamente?
O saldo correto seria 700, pois as duas subtrações deveriam ser aplicadas ao saldo inicial de 1000.

**Pergunta 19**  
Por que o resultado 800 caracteriza uma atualização perdida?
O resultado 800 caracteriza atualização perdida porque a alteração feita pela Transação A foi sobrescrita pela Transação B.

---

**Pergunta 20**  
Por que inserções em linhas diferentes nem sempre geram conflito direto?
Inserções em linhas diferentes nem sempre geram conflito porque cada transação trabalha em registros distintos, sem disputar o mesmo dado.

**Pergunta 21**  
O que esse experimento mostra sobre concorrência quando não há disputa pelo mesmo registro?
Esse experimento mostra que o banco consegue executar transações simultaneamente com eficiência quando não existe disputa pelo mesmo registro.

---

**Pergunta 22**  
Quais impactos um bloqueio mantido por muito tempo pode causar em um sistema real?
Bloqueios prolongados podem causar lentidão, filas de espera, redução de desempenho e até travamentos em sistemas com muitos usuários.

**Pergunta 23**  
Por que transações longas tendem a ser indesejáveis em ambientes concorrentes?
Transações longas são indesejáveis porque mantêm locks ativos por mais tempo, aumentando a chance de bloqueios, espera e conflitos entre usuários.

---

**Pergunta 24**  
Como verificar se o banco permaneceu consistente após todos os cenários executados?
É possível verificar a consistência analisando se os saldos e registros finais correspondem corretamente às operações realizadas, sem valores incorretos ou perdas de dados.

**Pergunta 25**  
Por que a análise final dos dados é importante após testes de concorrência?
A análise final é importante para confirmar que os mecanismos de controle de concorrência funcionaram corretamente e que o banco permaneceu íntegro após os testes.

---

### Questão 26
Explique o que é concorrência em banco de dados.
Concorrência em banco de dados é a execução simultânea de várias transações por diferentes usuários ou processos acessando os mesmos dados ou recursos do sistema.

### Questão 27
Descreva o papel dos bloqueios no controle de concorrência.
Os bloqueios controlam o acesso aos dados, impedindo que várias transações modifiquem o mesmo registro ao mesmo tempo e evitando inconsistências.

### Questão 28
Explique a diferença entre acessar registros iguais e registros diferentes em transações simultâneas.
Quando transações acessam registros diferentes, geralmente podem executar ao mesmo tempo sem conflito. Já ao acessar o mesmo registro, pode haver espera e bloqueios para proteger os dados.

### Questão 29
Por que `FOR UPDATE` é importante em determinadas operações críticas?
O FOR UPDATE é importante porque bloqueia registros que serão alterados, evitando modificações simultâneas em operações críticas como transferências e reservas.

### Questão 30
O que significa dizer que uma transação ficou esperando outra liberar um recurso?
Significa que uma transação não pôde continuar porque outra já estava utilizando ou bloqueando aquele recurso até finalizar sua operação.

### Questão 31
Explique o conceito de atualização perdida.
Atualização perdida ocorre quando duas transações alteram o mesmo dado e a alteração de uma sobrescreve a da outra.

### Questão 32
Descreva por que o isolamento é essencial em sistemas multiusuário.
O isolamento é essencial porque impede que usuários interfiram nas transações uns dos outros, mantendo os dados corretos e confiáveis.

### Questão 33
Explique como uma leitura pode ser afetada por outra transação ainda não concluída.
Uma leitura pode ser afetada se visualizar alterações ainda não confirmadas de outra transação, gerando informações temporárias ou inconsistentes.

### Questão 34
Por que transações longas podem prejudicar o desempenho de sistemas concorrentes?
Transações longas prejudicam o desempenho porque mantêm locks ativos por muito tempo, aumentando filas de espera e reduzindo a concorrência.

### Questão 35
Qual é a relação entre concorrência e consistência dos dados?
A concorrência precisa ser controlada para preservar a consistência dos dados, evitando conflitos e alterações incorretas.

### Questão 36
Descreva um exemplo real em que duas transações possam disputar o mesmo dado.
Um exemplo real é dois clientes tentando comprar o último ingresso disponível de um evento ao mesmo tempo.

### Questão 37
Explique por que nem toda operação simultânea gera conflito.
Nem toda operação simultânea gera conflito porque muitas transações trabalham em registros diferentes, sem disputar os mesmos dados.

### Questão 38
Como o banco de dados contribui para impedir que alterações simultâneas corrompam os dados?
O banco utiliza mecanismos como locks, controle de concorrência e níveis de isolamento para evitar que alterações simultâneas corrompam os dados.

### Questão 39
Explique o que aconteceria em um sistema bancário sem mecanismos de lock.
Sem mecanismos de lock, poderiam ocorrer perdas de dados, saldos incorretos e inconsistências causadas por alterações simultâneas não controladas.

### Questão 40
Qual a importância de observar a ordem de execução das transações em testes práticos?
Observar a ordem de execução é importante porque o resultado das transações pode variar dependendo de qual operação ocorre primeiro, afetando bloqueios e consistência dos dados.

---

## 8. Atividade prática com enunciado formal

### Enunciado
Um sistema bancário multiusuário precisa permitir operações simultâneas sem comprometer a integridade dos dados. Para isso, implemente testes em SQL que demonstrem:

- bloqueio explícito de registros com `FOR UPDATE`
- espera de uma transação por outra
- diferença entre concorrência em registros iguais e em registros diferentes
- risco de atualização perdida
- análise da consistência final dos dados após execuções concorrentes

-- =====================================================
-- TESTE 1 - BLOQUEIO EXPLÍCITO COM FOR UPDATE
-- =====================================================

-- Sessão 1
START TRANSACTION;

SELECT * FROM contas
WHERE id = 1
FOR UPDATE;

UPDATE contas
SET saldo = saldo - 100
WHERE id = 1;

-- NÃO executar COMMIT ainda


-- Sessão 2
START TRANSACTION;

UPDATE contas
SET saldo = saldo + 50
WHERE id = 1;

-- A transação ficará aguardando


-- Voltar para Sessão 1
COMMIT;


-- Depois finalizar Sessão 2
COMMIT;

-- =====================================================
-- TESTE 2 - CONCORRÊNCIA EM REGISTROS DIFERENTES
-- =====================================================

-- Sessão 1
START TRANSACTION;

UPDATE contas
SET saldo = saldo - 50
WHERE id = 1;


-- Sessão 2
START TRANSACTION;

UPDATE contas
SET saldo = saldo + 70
WHERE id = 4;


-- Finalizar ambas
COMMIT;

-- Consultar resultado
SELECT * FROM contas;

-- =====================================================
-- TESTE 3 - ESPERA ENTRE TRANSAÇÕES
-- =====================================================

-- Sessão 1
START TRANSACTION;

SELECT * FROM contas
WHERE id = 2
FOR UPDATE;

-- Manter transação aberta


-- Sessão 2
START TRANSACTION;

UPDATE contas
SET saldo = saldo + 10
WHERE id = 2;

-- Aguardar bloqueio


-- Voltar para Sessão 1
COMMIT;


-- Finalizar Sessão 2
COMMIT;

-- =====================================================
-- TESTE 4 - RISCO DE ATUALIZAÇÃO PERDIDA
-- =====================================================

-- Sessão 1
START TRANSACTION;

SELECT saldo
FROM contas
WHERE id = 4;

UPDATE contas
SET saldo = saldo - 100
WHERE id = 4;


-- Sessão 2
START TRANSACTION;

SELECT saldo
FROM contas
WHERE id = 4;

UPDATE contas
SET saldo = saldo - 200
WHERE id = 4;


-- Finalizar ambas
COMMIT;

-- Consultar resultado final
SELECT * FROM contas
WHERE id = 4;

-- =====================================================
-- TESTE 5 - LEITURA DURANTE TRANSAÇÃO NÃO FINALIZADA
-- =====================================================

-- Sessão 1
START TRANSACTION;

UPDATE contas
SET saldo = saldo - 200
WHERE id = 3;

-- NÃO finalizar


-- Sessão 2
SELECT * FROM contas
WHERE id = 3;

-- Voltar para Sessão 1
ROLLBACK;

-- =====================================================
-- TESTE 6 - ANÁLISE FINAL DA CONSISTÊNCIA
-- =====================================================

SELECT * FROM contas;

-- Verificar:
-- 1. Se os saldos permanecem corretos
-- 2. Se não houve perda de atualização
-- 3. Se os locks impediram conflitos
-- 4. Se as transações concorrentes mantiveram a integridade

### Objetivos
Ao final da atividade, o estudante deve ser capaz de:

- compreender o conceito de concorrência
- identificar situações de bloqueio
- analisar o efeito de locks em duas sessões simultâneas
- perceber quando há disputa por recursos
- discutir riscos de inconsistência em operações concorrentes
- relacionar concorrência com integridade e desempenho

### Tarefa final
Com base nos testes realizados, produza um texto explicando:

- o que é concorrência em banco de dados
- como funcionam os locks
- por que algumas transações precisam esperar
- o que é atualização perdida
- por que o isolamento é importante
- como o banco preserva a consistência em acessos simultâneos

A concorrência em banco de dados ocorre quando várias transações são executadas ao mesmo tempo por diferentes usuários ou processos. Em sistemas multiusuário isso é essencial, pois permite que várias operações aconteçam simultaneamente, aumentando o desempenho e a utilização do sistema. Porém, quando diferentes transações tentam acessar ou modificar os mesmos dados ao mesmo tempo, podem surgir inconsistências.
Para evitar esses problemas, o banco de dados utiliza mecanismos de controle de concorrência, como os locks. Os locks são bloqueios aplicados sobre registros para impedir alterações simultâneas indevidas. O comando FOR UPDATE, por exemplo, bloqueia uma linha selecionada até o fim da transação, garantindo acesso exclusivo ao dado.
Durante os testes realizados, foi possível observar que algumas transações precisaram esperar enquanto outra mantinha o lock ativo. Isso acontece porque o banco precisa preservar a integridade dos dados e impedir conflitos entre operações concorrentes. Quando duas transações acessam registros diferentes, normalmente elas podem executar juntas sem grandes problemas. Porém, quando disputam o mesmo registro, o sistema precisa coordenar o acesso.
Também foi possível analisar o risco de atualização perdida (lost update). Esse problema ocorre quando duas transações leem o mesmo valor inicial e gravam alterações diferentes, fazendo com que uma sobrescreva a outra. Sem mecanismos de bloqueio e isolamento, o resultado final pode ficar incorreto.
O isolamento é importante porque impede que transações interfiram diretamente umas nas outras antes da conclusão das operações. Assim, alterações não confirmadas não ficam visíveis indevidamente para outros usuários, garantindo maior segurança e previsibilidade.
Dessa forma, concorrência, locks e isolamento trabalham em conjunto para preservar a consistência do banco de dados. O banco coordena os acessos simultâneos, controla conflitos e garante que os dados permaneçam corretos mesmo em ambientes com muitos usuários executando operações ao mesmo tempo.

---

## 9. Questão integradora

### Questão 41
Considerando todos os experimentos realizados, explique de forma integrada como concorrência, bloqueios e isolamento atuam juntos para evitar inconsistências no banco de dados.
Concorrência, bloqueios e isolamento atuam juntos para evitar inconsistências em bancos de dados multiusuário. A concorrência permite que várias transações ocorram simultaneamente, aumentando o desempenho do sistema.
Porém, isso pode gerar conflitos quando diferentes transações acessam o mesmo dado ao mesmo tempo.
Os bloqueios controlam esse acesso, impedindo alterações simultâneas indevidas. Com mecanismos como FOR UPDATE, o banco garante que apenas uma transação modifique determinado registro enquanto as outras aguardam a liberação do lock. 
Isso evita problemas como atualização perdida e sobrescrita de dados.
O isolamento complementa esse processo ao impedir que transações visualizem alterações incompletas de outras transações. Assim, cada operação ocorre de forma segura e previsível, preservando a integridade dos dados.
Em conjunto, esses mecanismos garantem que o banco permaneça consistente mesmo com vários usuários realizando operações simultaneamente.

---