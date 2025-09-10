A mini Bankerize é uma API para cadastro de propostas em um sistema de terceiro que eventualmente fica indisponivel e
além disso você precisa enviar um email ou sms para uma API de terceiro que também pode ficar eventualmente indisponivel.
Você precisa conseguir realizar o cadastro completamente em 100% das vezes sem retornar falhas para o usuario final.

Requisitos funcionais:

1. Para cadastrar a proposta precisamos dos seguintes dados: CPF, Nome, Data de nascimento, Valor do empréstimo e chave pix.
2. Para simular a indisponibilidade da API de terceiro para realizar o cadastro utilize essa API mock: https://util.devi.tools/api/v2/authorize
3. Utilize o seguinte mock para simular o envio da notificação de empréstimo: https://util.devi.tools/api/v1/notify.
4. Esse serviço precisa ser restful

Requisitos não funcionais:

1. Deve ser feito com PHP 8 em diante 
2. Utilize Docker (utilizei xampp)
3. Banco de dados mysql ou postgresql - (utilizei mysql)
4. Construa testes automatizados
5. Utilize o Laravel

Endpoint de cadastro:
Você pode implementar o que achar necessário para esse serviço, porém iremos analisar somente o fluxo de cadastro de proposta.
A implementação deve serguir o exemplo abaixo:
POST /proposal
Content-Type: application/json

{
"cpf": "123123123123",
"nome": "Fulano de Tal",
"data_nascimento": "2024-10-10",
"valor_emprestimo": 1000.00,
"chave_pix": "teste@teste.com"
}

O que será avaliado:
Funcionar! - (está funcionando)
Aplicação de padrões de projeto (Se necessário) 
Conhecimentos em SOLID - OK
Aderência as PSRs 
Ports and Adapters (Hexagonal Architecture) 
Clean Code
Aderência as requisitos funcionais e não funcionais

<hr>


Passo a passo

Clone o repositório
git clone https://github.com/victoraraujo03/mini-bankerize.git
cd mini-bankerize

Instale dependências
composer install

Configure ambiente
cp .env.example .env
php artisan key:generate

Configure banco no .env
# DB_DATABASE=bankerize
# DB_USERNAME=root
# DB_PASSWORD=

Execute migrações
php artisan migrate

Instale APIs (Laravel 11+)
php artisan install:api


Para Executar:

Terminal 1: Servidor
php artisan serve

Terminal 2: Processador de filas
php artisan queue:work

Terminal 3: Testes
curl http://localhost:8000/api/health

Criar Proposta:

POST /api/proposal
Content-Type: application/json

{
    "cpf": "11144477735",
    "nome": "Maria Silva",
    "data_nascimento": "1985-03-15",
    "valor_emprestimo": 15000.00,
    "chave_pix": "maria@email.com"
}

Resposta (sempre sucesso):
{
    "message": "Proposta criada com sucesso",
    "data": {
        "id": 1,
        "cpf": "11144477735", 
        "nome": "Maria Silva",
        "data_nascimento": "1985-03-15",
        "valor_emprestimo": "15000.00",
        "chave_pix": "maria@email.com",
        "status": "pending"
    }
}

Consultar Proposta

GET /api/proposal/1


Listar Propostas

GET /api/proposal
GET /api/proposal?status=completed

health check:

GET /api/health

.ENV (variaveis de ambiente)

Banco de dados
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_DATABASE=bankerize
DB_USERNAME=root
DB_PASSWORD=

Redis (filas)
REDIS_HOST=127.0.0.1
REDIS_PORT=6379
QUEUE_CONNECTION=redis

APIs Externas  
EXTERNAL_API_URL=https://util.devi.tools/api/v2/authorize
NOTIFICATION_API_URL=https://util.devi.tools/api/v1/notify

Executar os Testes:
php artisan test


