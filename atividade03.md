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

#### Etapa 2. Testar COMMIT

Execute:

```sql
START TRANSACTION;

UPDATE contas
SET saldo = saldo - 100
WHERE id = 1;

UPDATE contas
SET saldo = saldo + 100
WHERE id = 2;

COMMIT;
```

Depois:

```sql
SELECT * FROM contas;
```

**Pergunta 3**  
O que aconteceu com os saldos após o `COMMIT`?
Os saldos foram alterados e persistiram no banco.

**Pergunta 4**  
Por que as duas instruções `UPDATE` devem fazer parte da mesma transação?
Para alterar individualmente cada saldo.

---

#### Etapa 3. Testar ROLLBACK

Execute:

```sql
START TRANSACTION;

UPDATE contas
SET saldo = saldo - 50
WHERE id = 2;

UPDATE contas
SET saldo = saldo + 50
WHERE id = 3;

ROLLBACK;
```

Depois:

```sql
SELECT * FROM contas;
```

**Pergunta 5**  
Por que os valores não foram alterados ao final?
Pois o Rollback voltou ao estado anterior.

**Pergunta 6**  
Em quais situações reais o uso de `ROLLBACK` seria essencial?
Quando alguma informação errada é inserida no banco de dados.

---

#### Etapa 4. Testar erro lógico antes da confirmação

Execute:

```sql
START TRANSACTION;

UPDATE contas
SET saldo = saldo - 2000
WHERE id = 3;

SELECT * FROM contas WHERE id = 3;

ROLLBACK;
```

Depois:

```sql
SELECT * FROM contas WHERE id = 3;
```

**Pergunta 7**  
Por que a transação foi desfeita neste caso?
Pois o Rollback voltou ao estado anterior

**Pergunta 8**  
Qual problema de integridade poderia ocorrer se essa transação fosse confirmada?
O saldo ficaria negativo

---

#### Etapa 5. Testar múltiplas operações na mesma transação

Execute:

```sql
START TRANSACTION;

UPDATE contas
SET saldo = saldo - 100
WHERE id = 4;

UPDATE contas
SET saldo = saldo + 60
WHERE id = 1;

UPDATE contas
SET saldo = saldo + 40
WHERE id = 2;

COMMIT;
```

Depois:

```sql
SELECT * FROM contas;
```

**Pergunta 9**  
Qual conta foi debitada e quais contas foram creditadas?
Daniela foi debitada 100, Ana foi creditada 60 e Bruno foi creditado 40.

**Pergunta 10**  
Por que esse conjunto de operações também deve ser tratado como uma única transação?
Pra não gerar um estado fraturado no sistema.

---

#### Etapa 6. Testar leitura durante transação

Execute:

```sql
START TRANSACTION;

UPDATE contas
SET saldo = saldo - 150
WHERE id = 1;
```

Sem executar `COMMIT` ainda, em outra sessão rode:

```sql
SELECT * FROM contas WHERE id = 1;
```

Depois volte para a primeira sessão e execute:

```sql
ROLLBACK;
```

**Pergunta 11**  
Qual era o objetivo de observar o valor da conta em outra sessão antes do `COMMIT`?
Para observar o resultado antes dos dados serem gravados.

**Pergunta 12**  
Como esse teste se relaciona com o conceito de isolamento?
Para realizar operações sem alterar o banco original.

---

#### Etapa 7. Testar lock com duas sessões

Abra duas conexões.

### Sessão 1

```sql
START TRANSACTION;

SELECT * FROM contas WHERE id = 1 FOR UPDATE;

UPDATE contas
SET saldo = saldo - 200
WHERE id = 1;
```

Não execute `COMMIT` ainda.

### Sessão 2

```sql
START TRANSACTION;

UPDATE contas
SET saldo = saldo + 300
WHERE id = 1;
```

**Observe**  
A segunda sessão pode ficar bloqueada esperando a primeira terminar.

Agora volte para a Sessão 1 e faça:

```sql
COMMIT;
```

Depois finalize a Sessão 2.

**Pergunta 13**  
O que aconteceu com a segunda transação?
Precisou esperar a primeira para ser aplicada.

**Pergunta 14**  
Por que ela precisou esperar?
Pois outra sessão estava alterando o banco.

**Pergunta 15**  
Qual a função do `FOR UPDATE`?
Evita o conflito de concorrência.

---

#### Etapa 8. Testar concorrência em registros diferentes

Abra duas conexões.

### Sessão 1

```sql
START TRANSACTION;

UPDATE contas
SET saldo = saldo - 50
WHERE id = 1;
```

### Sessão 2

```sql
START TRANSACTION;

UPDATE contas
SET saldo = saldo + 70
WHERE id = 4;
```

Depois execute `COMMIT` em ambas.

**Pergunta 16**  
Por que nesse caso as transações tendem a não disputar o mesmo recurso?

**Pergunta 17**  
O que esse teste mostra sobre concorrência em linhas diferentes da tabela?

---

#### Etapa 9. Criar tabela de movimentações

Execute:

```sql
DROP TABLE IF EXISTS movimentacoes;

CREATE TABLE movimentacoes (
    id INT AUTO_INCREMENT PRIMARY KEY,
    conta_origem INT,
    conta_destino INT,
    valor DECIMAL(10,2),
    data_movimentacao TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

**Pergunta 18**  
Qual é a importância de registrar movimentações além de atualizar os saldos?

---
