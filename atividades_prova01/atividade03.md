# Aplicando conceitos de transações em banco de dados sequencial

## 1. Atividade prática

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
Qual é o objetivo da tabela `contas` neste cenário prático?

Armazenar cada individuo e seu saldo.

**Pergunta 2**  
Quais são os saldos iniciais de cada titular antes da execução das transações?

1000, 500, 300 e 800 respectivamente.

---

**Pergunta 3**  
O que aconteceu com os saldos após o `COMMIT`?

Os saldos foram alterados e persistiram no banco.

**Pergunta 4**  
Por que as duas instruções `UPDATE` devem fazer parte da mesma transação?

Para alterar individualmente cada saldo.

---

**Pergunta 5**  
Por que os valores não foram alterados ao final?

Pois o Rollback voltou ao estado anterior.

**Pergunta 6**  
Em quais situações reais o uso de `ROLLBACK` seria essencial?

Quando alguma informação errada é inserida no banco de dados.

---

**Pergunta 7**  
Por que a transação foi desfeita neste caso?

Pois o Rollback voltou ao estado anterior

**Pergunta 8**  
Qual problema de integridade poderia ocorrer se essa transação fosse confirmada?

O saldo ficaria negativo

---

**Pergunta 9**  
Qual conta foi debitada e quais contas foram creditadas?

Daniela foi debitada 100, Ana foi creditada 60 e Bruno foi creditado 40.

**Pergunta 10**  
Por que esse conjunto de operações também deve ser tratado como uma única transação?

Pra não gerar um estado fraturado no sistema.

---

**Pergunta 11**  
Qual era o objetivo de observar o valor da conta em outra sessão antes do `COMMIT`?

Para observar o resultado antes dos dados serem gravados.

**Pergunta 12**  
Como esse teste se relaciona com o conceito de isolamento?

Para realizar operações sem alterar o banco original.

---

**Pergunta 13**  
O que aconteceu com a segunda transação?

Precisou esperar a primeira para ser aplicada.

**Pergunta 14**  
Por que ela precisou esperar?

Pois outra sessão estava alterando o banco de dados.

**Pergunta 15**  
Qual a função do `FOR UPDATE`?

Evita o conflito de concorrência.

---

**Pergunta 16**  
Por que nesse caso as transações tendem a não disputar o mesmo recurso?

Pois estão acessando partes distintas do banco de dados.


**Pergunta 17**  
O que esse teste mostra sobre concorrência em linhas diferentes da tabela?

O isolamento só se aplica quando os dados disputados são os mesmos.

---

**Pergunta 18**  
Qual é a importância de registrar movimentações além de atualizar os saldos?

É importante para monitorar e corrigir possíveis erros.

---

**Pergunta 19**  
Por que o `INSERT` na tabela `movimentacoes` deve estar na mesma transação dos `UPDATE`s?

Para assegurar que a movimentação esteja com dados atualizados de acordo com as mudanças feitas nos updates.

**Pergunta 20**  
O que poderia acontecer se o histórico fosse gravado, mas os saldos não fossem atualizados, ou vice-versa?

Alguns dados seriam perdidos.

---

**Pergunta 21**  
O que o `ROLLBACK` garantiu nesse cenário?

O rollback garantiu a consistência dos dados.

**Pergunta 22**  
Como esse teste demonstra a propriedade de atomicidade?

Ao realizar uma transação incompleta, as alterações que estavam ocorrendo foram interrompidas.

---

**Pergunta 23**  
Como verificar se o banco permaneceu consistente após todas as operações realizadas?

Se as informações permanecem coerentes com as alteraçẽos feitas então o banco permaneceu consistente.

**Pergunta 24**  
Por que a consistência do banco depende não apenas dos comandos SQL, mas também da forma como eles são agrupados em transações?

Pois nem todos os comandos SQL estarão no banco de dados final. 

---

### Questão 25
Explique o que é uma transação em banco de dados.

Uma transação em banco de dados é um conjunto de operações executadas como uma única unidade lógica. Todas as operações devem ser concluídas com sucesso ou nenhuma delas deve ser aplicada.

### Questão 26
Descreva a diferença entre `COMMIT` e `ROLLBACK`.

COMMIT confirma as alterações realizadas pela transação, salvando-as permanentemente no banco de dados. ROLLBACK desfaz todas as alterações feitas pela transação desde o último COMMIT.

### Questão 27
Explique por que uma transferência bancária deve ser tratada como transação.

Uma transferência bancária envolve pelo menos duas operações: retirar dinheiro de uma conta e adicionar em outra. Se uma delas falhar, a outra também deve ser desfeita para evitar inconsistências financeiras.

### Questão 28
O que pode acontecer se duas transações alterarem o mesmo dado ao mesmo tempo sem controle de concorrência?

Sem controle de concorrência, podem ocorrer inconsistências, como perda de dados, sobrescrita de alterações ou informações incorretas devido a modificações simultâneas.

### Questão 29
Qual a relação entre transações e as propriedades ACID?

As transações seguem as propriedades ACID: Atomicidade, Consistência, Isolamento e Durabilidade. Essas propriedades garantem segurança e confiabilidade nas operações do banco de dados.

### Questão 30
Explique o significado da propriedade de atomicidade no contexto de uma operação bancária.

Atomicidade significa que a operação acontece por completo ou não acontece. Em uma transferência bancária, o valor só pode ser retirado se também for depositado corretamente na outra conta.

### Questão 31
Explique o que significa dizer que uma transação preserva a consistência do banco de dados.

Uma transação preserva a consistência quando mantém os dados válidos e de acordo com as regras do sistema antes e depois da execução.

### Questão 32
Descreva o papel do isolamento em ambientes com múltiplos usuários acessando o mesmo banco.

O isolamento garante que várias transações executadas ao mesmo tempo não interfiram umas nas outras, evitando resultados incorretos para os usuários.

### Questão 33
Explique a importância da durabilidade após a execução de um `COMMIT`.

A durabilidade garante que, após um COMMIT, os dados permanecem salvos mesmo em caso de falha no sistema ou queda de energia.

### Questão 34
O que é controle de concorrência e por que ele é necessário?

Controle de concorrência é o mecanismo que organiza o acesso simultâneo aos dados. Ele é necessário para evitar conflitos e inconsistências causadas por múltiplos usuários.

### Questão 35
Explique a função do lock em transações concorrentes.

O lock bloqueia temporariamente um dado para impedir que outra transação o modifique ao mesmo tempo, garantindo segurança nas operações concorrentes.

### Questão 36
Descreva um exemplo prático em que o `FOR UPDATE` seja necessário.

Um exemplo é a reserva de uma vaga em um sistema de passagens. O FOR UPDATE bloqueia o registro da vaga enquanto a compra está sendo processada, evitando dupla reserva.

### Questão 37
O que é uma atualização perdida (*lost update*)?

Atualização perdida ocorre quando duas transações alteram o mesmo dado simultaneamente e a alteração de uma delas sobrescreve a outra.

### Questão 38
Explique por que nem toda leitura concorrente gera problema, mas algumas atualizações simultâneas sim.

Leituras concorrentes normalmente não alteram dados. Já atualizações simultâneas podem gerar conflitos, sobrescrever informações e causar inconsistências no banco.

### Questão 39
Qual é a importância de registrar operações em uma tabela de histórico dentro da mesma transação?

Registrar operações de histórico na mesma transação garante que o registro seja salvo apenas se toda a operação principal for concluída corretamente.

### Questão 40
Em um sistema acadêmico, cite um exemplo de operação que deveria ser tratada como transação.

Um exemplo é o processo de matrícula de um aluno, que pode envolver cadastro da disciplina, atualização de vagas e registro financeiro ao mesmo tempo.

### Questão 41
Em um sistema de estoque, cite um exemplo de falha que poderia justificar o uso de `ROLLBACK`.

Em um sistema de estoque, caso a quantidade de produtos seja reduzida mas a emissão da nota falhe, o ROLLBACK deve desfazer a alteração para manter o estoque correto.

### Questão 42
Como o processamento de transações contribui para a confiabilidade de sistemas de informação?

O processamento de transações aumenta a confiabilidade porque garante integridade, segurança e recuperação correta dos dados mesmo em situações de falha ou acesso simultâneo.

---

### Questão 43
Considerando todos os experimentos realizados, explique de forma integrada como atomicidade, consistência, isolamento e durabilidade atuam em conjunto no processamento de transações.

As propriedades ACID atuam juntas para garantir segurança e confiabilidade no processamento de transações. A atomicidade garante que todas as operações sejam concluídas completamente ou desfeitas em caso de falha.
A consistência assegura que os dados permaneçam corretos e dentro das regras do sistema antes e depois da transação. O isolamento impede que transações simultâneas interfiram umas nas outras, evitando conflitos e inconsistências.
Já a durabilidade garante que, após o COMMIT, as alterações permaneçam salvas mesmo em caso de falhas no sistema. Em conjunto, essas propriedades tornam o banco de dados seguro e confiável.
