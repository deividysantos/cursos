# Testes unitários e TDD com PHP e PHPUnit
#### https://www.udemy.com/course/testes-unitarios-php-phpunit/

<br>

## Teste de Integração X Testes unitários
Testes de integração
    
Testa a aplicação de forma integrada
    broad integration test
    narrow integration tests

cobre mais código nos primeiros testes
bons para começar
mais lentos e configuração do ambiente mais trabalhosa
mais difícil de encontrar o motivo da falha


Testes unitários

Testa de forma contextual
Mais rápidos 
Mais fácil de encontrar a falha
Mais testes para chegar numa boa cobertura
Setup simples

---------------------------------------------------------------------------------------------------------------------------

Nomes para testes

3 perguntas que devem ser respondidas

-O que está sendo testado?
-Quais as circunstâncias?
-Qual o resultado esperado?

Ex: 
shouldBeValidWhenValueIsANumber
should_BeValid_When_ValueIsANumber
isValid_ValuesIsANumber_True
beValidIdValueIsANumber

### NOMES LEGÍVEIS E CLAROS ###
---------------------------------------------------------------------------------------------------------------------------










Data Providers
Para que possamos criar vários testes sem a necessidade da repetição de código, por eles serem muito parecidos (alternando apenas os valores de entrada e saída), o phpunit trás a possibilidade de utilização dos data providers:

/**
*@test (indica para o phpunit que esta função é um test, retirando a necessidade de utilizar o prefixo test no nome do método)
*dataprovider valueDataProvider (indica para o phpunit que esta função será utilizada para retornar todos os valores de $value que serão testados com os seus respectivos nomes de teste)
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
---------------------------------------------------------------------------------------------------------------------------























Dublês de teste
Stubs
Mocks
Dummies
Spies
Fakes



Stub
Classe fake que apenas implementa o necessário para realizar o processamento necessário para o teste. Normalmente utilizada quando as dependências da classe real necessária são inviáveis de serem instanciadas.
Ex: Controller que faz a instanciação um repository, que por sua vez recebe uma classe de conexão com o db.

Utilizando PhpUnit:
    $classStub = $this->createMock(ClassOriginal::class);
    $classStub->method(‘getMessage’)
          ->willReturn(‘This is a message on my stub, not a message real’);

Por trás, o phpunit cria uma classe e acrescenta nela os métodos indicados que irão retornar apenas o necessário para a realização do teste unitário. Evitando, assim, que o mesmo se torne um teste de integração.
