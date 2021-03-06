# Testes unitários e TDD com PHP e PHPUnit
#### https://www.udemy.com/course/testes-unitarios-php-phpunit/

<br>

---------------------------------------------------------------------------------------------------------------------------

## Teste de Integração X Testes unitários

### Testes de integração
    
- Testa a aplicação de forma integrada
    - broad integration test
    - narrow integration tests
 - Cobre mais código nos primeiros testes
- Bons para começar
- Mais lentos e configuração do ambiente mais trabalhosa
- Mais difícil de encontrar o motivo da falha

<br>

### Testes unitários
- Testa de forma contextual
- Mais rápidos 
- Mais fácil de encontrar a falha
- Mais testes para chegar numa boa cobertura
- Setup simples

---------------------------------------------------------------------------------------------------------------------------
## Nomes para testes

### Perguntas que devem ser respondidas no nome do método
- O que está sendo testado?<br>
- Quais as circunstâncias?<br>
- Qual o resultado esperado?<br>

Ex: <br>
shouldBeValidWhenValueIsANumber
should_BeValid_When_ValueIsANumber<br>
isValid_ValuesIsANumber_True<br>
beValidIdValueIsANumber<br>

OBS: Sempre Utilizar nomes claros

---------------------------------------------------------------------------------------------------------------------------

## Data Providers
Para que possamos criar vários testes sem a necessidade da repetição de código, por eles serem muito parecidos (alternando apenas os valores de entrada e saída), o phpunit trás a possibilidade de utilização dos data providers:

```php
/**
*@test 
*indica para o phpunit que esta função é um test, retirando a
*necessidade de utilizar o prefixo test no nome do método
*
*@dataprovider valueDataProvider 
*indica para o phpunit que esta função será utilizada para retornar todos
*os valores de $value que serão testados com os seus respectivos nomes de teste
*/
public function testFoo($value)
{
    //teste
}

public function valueDataProvider()
{
    return [
        ‘nameTest’ => [‘varName’ => ‘varValue’],
        ‘nameTest2’ => [‘varName’ => ‘varValue2]
    ];
}
```
---------------------------------------------------------------------------------------------------------------------------

## Dublês de teste
Stubs<br>
Mocks<br>
Dummies<br>
Spies<br>
Fakes<br>

### Stub
Classe fake que apenas implementa o necessário para realizar o processamento necessário para o teste. Normalmente utilizada quando as dependências da classe real necessária são inviáveis de serem instanciadas.
Ex: Controller que faz a instanciação um repository, que por sua vez recebe uma classe de conexão com o db.

Utilizando PhpUnit:
```php
    use PHPUnit\Framework\TestCase;

    public function test()
    {
        $classStub = $this->createMock(ClassOriginal::class);
        $classStub->method("getMessage")
            ->willReturn("This is a message on my stub, not a message real");
    }
    
```

Por trás, o phpunit cria uma classe e acrescenta nela os métodos indicados que irão retornar apenas o necessário para a realização do teste unitário. Evitando, assim, que o mesmo se torne um teste de integração.

### Mocks 
É utilizado para testar comportamentos e não necessáriamente retornos.

Ex:

```php
use PHPUnit\Framework\TestCase;

    /**
     * @test
    */
    public function testCreateUser()
    {
        $payload = [
            'email' => 'test@gmail.com',
            'password' => 'secretPassword'
        ];

        $repository = $this->createMock(UserRepository::class);
        $repository->expects($this->once())
            ->method('create');
        //funciona como uma assert
        //verifica se o método create do repository é chamado em 
        //algum momento pelo método postCreate do controller

        $controller = new UserController($repository);
        $controller->postCreate($payload);
    }
```

Os mocks podem retornar diferentes valores dependendo dos parâmetros passados

```php
use PHPUnit\Framework\TestCase;

    /**
     * @test
    */
    public function testShouldSuccessfullyPayWhenApiReturnOk()
    {
        $httpClient = $this->createMock(HttpClient::class);
        $service = new PaymentService($httpClient);

        $map = [
            [
                'POST',
                [
                    'user' => 'login'
                    'password' => 'password'
                ],
                'api.PaymentServiceWeb.com/authenticate',
                'meu-token'
            ],
            [
                'POST',
                [
                    'name' => 'userName',
                    'creditCardNumber' =: '999999999',
                    'validity' => '12/30',
                    'value' => 100
                    'token' => $token
                ],
                'api.PaymentServiceWeb.com/pay',
                ['paid' => true]
            ]
        ];

        $httpClient->expects($this->atLeast(2))
            ->method('sendRequest')
            ->will($this->returnValueMap($map));
        
        /**
         * O método sendRequest do httpClient será chamado duas vezes
         * Logo, será necessário retornar valores diferentes para cada chamada, imitando o comportamento do service
         * O método sendRequest espera 3 parâmentros, logo o quarto parâmetro passado em cada array de map 
         * é referente ao retorno fictício do método
        */

        $paid = $service->pay(
            [
                'user' => 'login'
                'password' => 'password'
            ],
            'userName',
            '999999999',
            '12/30',
            100
        );

        $this->assertEquals(true, $paid);
    }
```
E também podem retornar valores diferentes dependendo da ordem da chamada

```php
use PHPUnit\Framework\TestCase;

    /**
     * @test
    */
    public function testShouldSuccessfullyPayWhenApiReturnOk()
    {
        $httpClient = $this->createMock(HttpClient::class);
        $service = new PaymentService($httpClient);

        $httpClient->expects($this->at(0))
            ->method('sendRequest')
            ->willReturn('meu-token');
        /**
         * Na primeira vez que o método sendRequest for chamado, o retorno será meu-token, 
         * independente de quais sejam os parâmetros passados
         * 
         * Este comportamento é possível graças ao método at()
        */

        $httpClient->expects($this->at(1))
            ->method('sendRequest')
            ->willReturn(['paid' => true]);

        $paid = $service->pay(
            [
                'user' => 'login'
                'password' => 'password'
            ],
            'userName',
            '999999999',
            '12/30',
            100
        );

        $this->assertEquals(true, $paid);
    }
```

### Dummies
São entidades criadas apenas para satisfazer alguma necessidade de métodos, não irão necessariamente ser usados.
Podem ser criados a partir do createMock do PHPUnit, mas existe necessidade de criação de nenhuma assert nem retorno de método.

### Spies 
Assert pelo estado do objeto.

Curti não rs

### Fakes
Simular comunicação com banco de dados, apis entre outras coisas.
