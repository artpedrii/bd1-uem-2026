### PARTE I 
A. 
A integridade de dados é a garantia de que as informações de uma organização ou sistemma são precisas, completas, consistentes e confiáveis durante todo o seu ciclo de vida, desde a coleta até o armazenamento.

B.
1. Garantir que cada registro na tabela seja único e identificável.
2. A chave primária segue essa mesma cartilha, deve ser única e identificável.
3. Não.
4. Não seria íntegra, pois devem ser únicas e sempre identificáveis.
5. Cadastro de aluno:
Aluno
_________
IdAluno (PK) - garantindo que nenhum aluno seja cadastrado sem ID ou com um ID já existente de outro aluno.
Nome
DataNascimento

C.
1. A integridade referencial é a regra de bancos de dados relacionais que garante a consistência dos relacionamentos entre as tabelas. Ela dita que, se uma tabela faz referência a dados de outra tabela, essa referência deve ser obrigatoriamente válida.
Na prática, ela controla a relação entre a Chave Primária (tabela principal) e a Chave Estrangeira (tabela dependente), estabelecendo que:
- Uma chave estrangeira só pode apontar para uma chave primária que já existe.
- Se um registro na tabela principal for excluído ou alterado, o banco de dados deve saber o que fazer com os registros vinculados a ele para não deixar "dados órfãos".
2. Ela deve garantir que a chave estrangeira tenha referência válida.
3. Não.
4. Quando um registro é exluido na tabela principal, os registros vinculados a ele ficam órfãos.
5. CASCADE
6.
Aluno 
________
IdAluno (Pk)
Nome

AluguelLivro 
_______________
IdAluguel (Pk)
IdAluno (Fk)
Titulo

D.
1.  Conjunto de todos os valores válidos e aceitáveis que uma coluna pode receber.
2. Tipo de dado, formato, intervalo ou lista de valores e condição de nulidade.
3.  Sim.
4. Sim, resondido no item 2.
5. 
    i - devem ser representados valores do tipo de dado float maiores que zero, sem nulidade NOT NULL.
    ii - Estoque deveria ser representado apenas pelo tipo booleano, NOT NULL também.

### PARTE II
Nome escrito como -, Não esta no formato de um nome, Integridade de dominio, restringir o formato

Preço negativo, não pode existir preço assim, integridade de dominio, colocar num intervalo

Referencia a chave nao existente, Não existe esse Id na tabela principal, Integridade Referencial, sempre ter um referente válido

email repetido, emails sao unicos, integridade de dados, garantir unicidade de um email

Estoque negativo, o numero minimo de um estoque é 0, integridade de dominio, restringir por um intervalo

Id repetido, id deve ser unico, integridade de dados, garantir unicidade de ID

Nome de produto no formato errado, integridade de dominio, garantir formato do nome

Nome de cliente errado formato, formato invalido, integridade de domínio, deve garantir formato correto.

Id rpeetido estrangeiro em PEDIDO, deve ser unico, integridade de dados, garantir unicidade.

### PARTE III
CLIENTE -> R01 Todo cliente deve ter um identificador unico (E), R02 Todo nome deve ser único e estar no formato correto (D), R03 Todo email deve ser único e estar no formato correto (D)

PRODUTO -> R01 Todo produto deve ter identificador unico (E), R02 todo nome deve ser unico e estar no formato correto (D), R03 Todo preço deve ser maior que zero e ser float (D). R04 Estoque deve ser int maior que zero (D).

PEDIDO -> R01 Todo pedido tem id unico (E), R02 data deve estar no formato valido e nao ser repetida (E) (D), R03 valor deve ser float e maior ou igual a zero (D), id cliente deve existir (R)

### PARTE IV
PRIMARY KEY -> 1. Unicidade identificadora. 2. CPF de alguem
FK -> 1. Relação única, 2. Associar uma compra a um CPF
NOT NULL -> 1. valores nao podem ser nulos, 2. nome de alguem
UNIQUE -> Unicidade pura. Email
CHECK -> Garantia. Garantir pertencimento a um cliente existente de pedido.
DEFAULT -> Obrigatoriamente iniciar um valor padrao. Saldo em conta bancaria.

PK
NOT NULL
UNIQUE
CHECK
CHECK
DEFAULT

Sim. Os mecanismos existem justamente para resolver esses problemas de integridade.