# unit-tests-study 📑

Estudo sobre testes unitários. Com intuito de levantar boas práticas e aprender mais sobre o tema. 

## Testar métodos privados ? 

Isso é uma grande discussão no universo de testes unitários. Mas pesquisando em alguns sites vi o pessoal falando que se você está sentindo necessidade de testar um método privado provavelmente o seu método "pai" tem mais de uma responsibilidade o que iria contra os principios SOLID. Além disso se um método privado deve ser testado, provavelmente ele tem complexidade demais e provavelmente seria o caso de separar ele em uma nova classe, que seria responsável por lidar com toda essa complexidade exigida nesse contexto.

## Boas práticas de nomenclatura.

De acordo com as fontes abaixo o nome do seu teste deve consistir em três partes:

- O nome do método que está sendo testado.
- O cenário em que está sendo testado.
- O comportamento esperado quando o cenário é invocado.

Ruim:

```c#

[Fact]
public void Test_Single()
{
    var stringCalculator = new StringCalculator();

    var actual = stringCalculator.Add("0");

    Assert.Equal(0, actual);
}

```

Bom:

```c#

[Fact]
public void Add_SingleNumber_ReturnsSameNumber()
{
    var stringCalculator = new StringCalculator();

    var actual = stringCalculator.Add("0");

    Assert.Equal(0, actual);
}

```

Método: Add
Entrada: SingleNumber
Resultado: SameNumber

`MethodName_Input_ExpectedBehavior`

Eu ainda acho que deveriam adicionar uma sequencia. Algo como : `001_MethodName_Input_ExpectedBehavior`

## Evitar números mágicos:

101 >> const string MAXIMUM_RESULT = "1001";

## Utilize métodos auxiliares para SETUP e TEARDOWN

Exemplo: 

```c#

[Fact]
public void Add_TwoNumbers_ReturnsSumOfNumbers()
{
    var stringCalculator = CreateDefaultStringCalculator();

    var actual = stringCalculator.Add("0,1");

    Assert.Equal(1, actual);
}

// more tests...

private StringCalculator CreateDefaultStringCalculator()
{
    return new StringCalculator();
}

```

O método `CreateDefaultStringCalculator` pode ser usado em vários métodos de teste, facilitando manutenção e criação de novos testes.

## Evite mais de uma asserção 

Ao escrever seus testes, tente incluir apenas um ato por teste. Abordagens comuns para usar apenas um ato incluem:

- Crie um teste separado para cada ato.
- Use testes parametrizados.

### Por que?
- Quando o teste falha, fica claro qual ato está falhando.
- Garante que o teste seja focado em apenas um único caso.
- Dá a você uma imagem completa de por que seus testes estão falhando.
  
Vários atos precisam ser declarados individualmente e não é garantido que todos os Asserts serão executados. Na maioria das estruturas de teste de unidade, uma vez que um Assert falha em um teste de unidade, os testes subsequentes são automaticamente considerados como falhando. Esse tipo de processo pode ser confuso, pois a funcionalidade que está realmente funcionando será mostrada como falha.

```c#

[Fact]
public void Add_EmptyEntries_ShouldBeTreatedAsZero()
{
    // Act
    var actual1 = stringCalculator.Add("");
    var actual2 = stringCalculator.Add(",");

    // Assert
    Assert.Equal(0, actual1);
    Assert.Equal(0, actual2);
}

```


## Organização de testes

Arrange, Act, Assert (AAA) é um padrão comum durante o teste de unidade. Como o nome indica, consiste em três ações principais:

- Organize seus objetos, crie e configure-os conforme necessário.
- Atuar em um objeto.
- Afirme que algo é como esperado.

```c#

[Fact]
public void Add_EmptyString_ReturnsZero()
{
    // Arrange
    var stringCalculator = new StringCalculator();

    // Act
    var actual = stringCalculator.Add("");

    // Assert
    Assert.Equal(0, actual);
}

```


Fonts: 

https://learn.microsoft.com/en-us/dotnet/core/testing/unit-testing-best-practices
