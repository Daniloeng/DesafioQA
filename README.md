# DesafioQA
Desafio QA Maxxidata


## Revisão de documentação

### Cadastrar Pessoa
Permite realizar o cadastro de uma nova pessoa. Os campos disponíveis para cadastro são:

- nome: Uma string, obrigatória, de até 255 caracteres que armazena o nome da pessoa;
- dataNascimento: Uma string, obrigatório, que armazena a data de nascimento da pessoa. A data de nascimento não pode ser maior que a data atual. O formato da data deve ser YYYY-MM-DD;
- cpf: Uma string, não obrigatória, que armazena o CPF da pessoa (com a máscara do campo). Este campo pode ser vazio, mas se for informado os dígitos verificadores do CPF devem ser validados (https://dicasdeprogramacao.com.br/algoritmo-para-validar-cpf/);
- ativo: Um booleano, obrigatório, que indica se a pessoa está ativa na viagem. O valor true indica que a pessoa irá participar da viagem e o valor false indica que a pessoa não irá participar da viagem. Por default deve ser setado como True, indicando a participação da viagem;
- meta: Um valor numérico, obrigatório, que indica a meta em Reais que a pessoa busca alcançar. Este valor deve ser maior que 0.

### Visualizar Pessoas
Retorna uma lista com todas as pessoas (considerando uma paginação de registros) e suas informações. Os parâmetros página e tamanho são utilizados para controlar os registros que são retornados na requisição.

### Visualizar Pessoa Específica
Retorna apenas às informações de uma pessoa. Sendo obrigatório informar o parâmetro: idPessoa(ou selecionar - filtro front-end) a ser visualizada. 


### Editar Pessoa
Permite editar as informações de uma pessoa. O registro deve respeitar as mesmas regras de negócio da API de cadastro de pessoa, onde os campos nome, dataNascimento, ativo e meta são obrigatórios e o CPF é opcional, porém se estiver preenchido deve seguir as regras de validação conforme link previsto no cadastro de pessoa.

### Cadastrar Receita
Permite realizar o cadastro de uma nova receita. Os campos disponíveis para cadastro são:
- pessoaId: Um select que deve apresentar o número inteiro do registro concatenado com o nome da pessoa, obrigatório, que contém o ID da pessoa (válida) que está armazenando a receita. Não deve ser possível informar o ID de uma pessoa não cadastrada;
- data: Uma string, obrigatório, que armazena a data em que a pessoa está guardando o dinheiro. A data da inclusão da receita não pode ser maior que a data atual. O formato da data deve ser YYYY-MM-DD; Por default, este campo já deve vir preenchido com a data atual(do dia), podendo ser alterado pela pessoa.
- valor: Um número numérico, obrigatório, que representa o montante de dinheiro que a pessoa está guardando. Este valor deve ser maior que 0.

### Visualizar Receitas
Retorna uma lista com todos os dados das receitas (valor da receita, data e pessoaId) cadastradas. Os parâmetros página e tamanho são utilizados para controlar os registros que são retornados na requisição.

### Visualizar Receitas Específica
Retorna as informações de uma receita específica. Sendo obrigatório informar o parâmetro: idReceita(ou selecionar - filtro front-end) a ser visualizada.

### Editar Receita
Permite editar as informações de uma receita. O registro deve respeitar as mesmas regras de negócio da API de cadastro de receita, onde todos os campos pessoaId, data e valor são obrigatórios;
 
### Deletar Receita
Permite deletar uma receita. Para isso é obrigatório informar(ou selecionar  - filtro front-end) o id da receita a ser deletada.

### Visualizar Totalizadores
Calcula o valor total que foi guardado por cada pessoa, somando todas receitas registradas por pessoa. Com a opção de filtro de ativo, é possível retornar o cálculo do valor total de todas as pessoas ativas na viagem.



## Criação de casos de teste de API

### GET pessoas

```
pm.test("Status code is 200", function () {
    pm.response.to.have.status(200);
});

pm.test("Verificar atributo de totalização", function() {
    const responseJson = pm.response.json();   
    pm.expect(responseJson.pessoas.length).to.equal(responseJson.numeroRegistros)
});

pm.test("testa se todas pessoas possuem campos obrigatórios", function(){
    _.forEach(pm.response.json(), function(pessoa){
        pm.expect(pessoa.nome).to.not.be.null &&
        pm.expect(pessoa.dataNascimento).to.not.be.null && 
        pm.expect(pessoa.meta).to.not.be.null ;
    })
});
```

### POST pessoas

```
pm.test("Successful POST request", function () {
    pm.expect(pm.response.code).to.be.oneOf([201, 202]);
});

pm.test("testa se a pessoa possui campos obrigatórios", function(){
    _.forEach(pm.response.json(), function(pessoa){
        pm.expect(pessoa.nome).to.not.be.null &&
        pm.expect(pessoa.dataNascimento).to.not.be.null && 
        pm.expect(pessoa.meta).to.not.be.null ;
    })
});
```
### PUT pessoas

```
pm.test("Successful PUT request", function () {
    pm.expect(pm.response.code).to.be.oneOf([200, 204]);
});

pm.test("testa se todas pessoas possuem campos obrigatórios", function(){
    _.forEach(pm.response.json(), function(pessoa){
        pm.expect(pessoa.nome).to.not.be.null &&
        pm.expect(pessoa.dataNascimento).to.not.be.null && 
        pm.expect(pessoa.meta).to.not.be.null ;
    })
});
```

### GET receitas
```
pm.test("Status code is 200", function () {
    pm.response.to.have.status(200);
});

pm.test("testa se todas receitas possuem campos obrigatórios", function(){
    _.forEach(pm.response.json(), function(receita){
        pm.expect(receita.pessoaId).to.not.be.null &&
        pm.expect(receita.data).to.not.be.null && 
        pm.expect(receita.valor).to.not.be.null ;
    })
});
```

### POST receita

```
pm.test("Successful POST request", function () {
    pm.expect(pm.response.code).to.be.oneOf([201, 202]);
});

pm.test("testa se a receitas possui campos obrigatórios", function(){
    _.forEach(pm.response.json(), function(receita){
        pm.expect(receita.pessoaId).to.not.be.null &&
        pm.expect(receita.data).to.not.be.null && 
        pm.expect(receita.valor).to.not.be.null ;
    })
});
```

### PUT receita

```
pm.test("Successful PUT request", function () {
    pm.expect(pm.response.code).to.be.oneOf([200, 204]);
});

pm.test("testa se a receita possui campos obrigatórios", function(){
    _.forEach(pm.response.json(), function(receita){
        pm.expect(receita.pessoaId).to.not.be.null &&
        pm.expect(receita.data).to.not.be.null && 
        pm.expect(receita.valor).to.not.be.null ;
    })
});
```

## Criação de casos de teste de interface

### Pessoa
COMO Pessoa PRECISO cadastrar dados de Pessoa e meta PARA viajar após a pandemia.

- [ ] DADO uma pessoa, QUANDO a pessoa preencher seus dados (nome, dataNascimento, cpf, ativo), definir sua meta E “Cadastrar”, ENTÃO os dados são persistidos.

| Passo nº | Caminho | Detalhes | Resultado esperado | Aprovado? |
| -------- | ------- | -------- | ------------------ | --------- |
| 1        | Principal | O usuário acessa a página Lista de Pessoas e clica no botão Cadastrar Nova Pessoa.| A aplicação apresenta o cadastro de Pessoa com os campos para preenchimento conforme especificado no da revisão da documentação. | |
| 2        | Principal | O usuário preenche todos os campos obrigatórios e pressiona a opção Cadastrar.| A aplicação apresenta mensagem informando ao usuário que a Pessoa foi salva com sucesso. | |
| 3        | Alternativo | O usuário não preenche todos os campos obrigatórios e pressiona a opção Cadastrar.| A aplicação apresenta mensagem apontando os campos obrigatórios não preenchidos, que devem ser preenchidos. | |


COMO Pessoa PRECISO editar dados de Pessoa PARA atualizar dados antes de viajar.

- [ ] DADO uma pessoa, QUANDO a pessoa edita seus dados (nome, dataNascimento, cpf, ativo, meta) E “Atualizar”, ENTÃO os dados são persistidos.

| Passo nº | Caminho | Detalhes | Resultado esperado | Aprovado? |
| -------- | ------- | -------- | ------------------ | --------- |
| 1        | Principal | usuário acessa a página Lista de Pessoas e clica no botão Atualizar na linha correspondente ao registro da Pessoa desejada.| A aplicação apresenta o formulário de Pessoa com os campos preenchidos. | |
| 2        | Principal | O usuário faz as alterações desejadas e pressiona a opção Atualizar.| A aplicação apresenta mensagem informando ao usuário que a Pessoa foi atualizada com sucesso. | |
| 3        | Alternativo | O usuário ao editar não preenche todos os campos obrigatórios e pressiona a opção Atualizar.| A aplicação apresenta mensagem apontando os campos obrigatórios não preenchidos, que devem ser preenchidos. | |


### Receita
COMO Pessoa PRECISO cadastrar dados de Receita PARA viajar após a pandemia.

- [ ] DADO uma receita, QUANDO a pessoa preencher dados de receita (idPessoa, data, valor) E “Cadastrar”, ENTÃO os dados são persistidos.


| Passo nº | Caminho | Detalhes | Resultado esperado | Aprovado? |
| -------- | ------- | -------- | ------------------ | --------- |
| 1        | Principal | usuário acessa a página Lista de Receitas e clica no botão Cadastrar Nova Receita.| A aplicação apresenta o cadastro de Receita com os campos para preenchimento conforme especificado no da revisão da documentação. | |
| 2        | Principal | O usuário preenche todos os campos obrigatórios e pressiona a opção Cadastrar.| A aplicação apresenta mensagem informando ao usuário que a Receita foi salva com sucesso. | |
| 3        | Alternativo | O usuário não preenche todos os campos obrigatórios e pressiona a opção Cadastrar.| A aplicação apresenta mensagem apontando os campos obrigatórios não preenchidos, que devem ser preenchidos. | |


COMO Pessoa PRECISO editar dados de Receita PARA atualizar dados da receita.

- [ ] DADO uma função, QUANDO a pessoa edita seus dados (idPessoa, Data e Valor) E “Atualizar”, ENTÃO os dados são persistidos.

| Passo nº | Caminho | Detalhes | Resultado esperado | Aprovado? |
| -------- | ------- | -------- | ------------------ | --------- |
| 1        | Principal | O usuário acessa a página Lista de Receitas e clica no botão Atualizar na linha correspondente ao registro da Receita desejada | A aplicação apresenta o formulário da Receita com os campos preenchidos. | |
| 2        | Principal | O usuário faz as alterações desejadas e pressiona a opção Atualizar.| A aplicação apresenta mensagem informando ao usuário que a Receita foi atualizada com sucesso. | |
| 3        | Alternativo | O usuário não preenche todos os campos obrigatórios e pressiona a opção Atualizar.| A aplicação apresenta mensagem apontando os campos obrigatórios não preenchidos, que devem ser preenchidos. | |


COMO Pessoa PRECISO deletar Receita PARA atualizar dados da receita total.

- [ ] DADO uma função, QUANDO a pessoa deleta sua receita E “Atualizar”, ENTÃO os dados são deletados.

| Passo nº | Caminho | Detalhes | Resultado esperado | Aprovado? |
| -------- | ------- | -------- | ------------------ | --------- |
| 1        | Principal | O usuário acessa a página Lista de Receitas e clica no botão Excluir na linha correspondente ao registro da Receita desejada. | A aplicação apresenta a mensagem de confirmação de exclusão da receita. | |
| 2        | Principal | O usuário faz a leitura da mensagem e pressiona a opção Confirmar.| A aplicação deleta a receita e apresenta mensagem informando ao usuário que a Receita foi excluída com sucesso. | |
| 3        | Alternativo | O usuário faz a leitura da mensagem e pressiona a opção Cancelar.| A aplicação mantém o registro da receita e retorna para lista de Receitas. | |


## Criação da Automação de API

- Conforme código no repositório, realizar o clone para ambiente local;
- executar `npm install` para baixar as dependências;
- executar `npm start` para subir a api e realizar os testes.

### Dados para execução de testes

#### No formato JSON para o endpoit **/api/pessoas** para o método POST para criar as pessoas:

```
{
    "nome": "Patricia da Silva",
    "dataNascimento": "1991-01-01",
    "cpf": "277.490.610-53",
    "ativo": true,
    "meta": 800.00
} 
{
    "nome": "João da Silva",
    "dataNascimento": "1981-05-03",
    "cpf": "184.444.660-34",
    "ativo": true,
    "meta": 1000.00
} 
{
    "nome": "Maria Clara da Silva",
    "dataNascimento": "1985-10-11",    
    "ativo": true,
    "meta": 600.00
} 
{
    "nome": "Pedro Ricardo da Silva",
    "dataNascimento": "1989-15-12",
    "cpf": "087.327.040-11",
    "ativo": false,
    "meta": 500.00
} 
```

#### No formato JSON para o endpoit **/api/receitas** para o método POST para criar as receitas:

```
{
    "pessoaId": 1,
    "data": "2021-03-12",
    "valor": 300
}
{
    "pessoaId": 2,
    "data": "2021-04-15",
    "valor": 150
}
{
    "pessoaId": 2,
    "data": "2021-03-15",
    "valor": 500
}
{
    "pessoaId": 4,
    "data": "2021-05-10",
    "valor": 1000
}
```