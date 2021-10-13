
[//]: # (title: Tipos de dados)

Em Kotlin, tudo é objeto, no sentido de que podemos chamar funções e propriedades em qualquer variável.
Alguns tipos podem ter uma representação interna específica - por exemplo, números, caracteres e booleanos podem ser
representados como valores primitivos em tempo de execução - mas para o usuário, eles são classes comuns.
Nesta seção, descrevemos os tipos básicos usados no Kotlin: [números](#números), [booleanos](#booleanos),
[caracteres](#caracteres), [strings](#strings) e [matrizes](#matrizes).

## Números

### Int - Números inteitos

Kotlin fornece um conjunto de tipos primitivos que representam números. Para números inteiros, existem quatro tipos com tamanhos diferentes e, portanto, intervalos de valores diferentes.

| Type	 |Tamanho (bits)| Valor mínimo| Valor máximo|
|--------|-----------|----------|--------- |
| Byte	 | 8         |-128      |127       |
| Short	 | 16        |-32768    |32767     |
| Int	 | 32        |-2,147,483,648 (-2<sup>31</sup>)| 2,147,483,647 (2<sup>31</sup> - 1)|
| Long	 | 64        |-9,223,372,036,854,775,808 (-2<sup>63</sup>)|9,223,372,036,854,775,807 (2<sup>63</sup> - 1)|

Todas as variáveis inicializadas com valores inteiros não excedendo o valor máximo de `Int` têm o tipo inferido `Int` por padrão. Se o valor inicial exceder esse valor, o tipo inferido será `Longo`. Para especificar o valor `Long` explicitamente, acrescente o sufixo `L` ao valor.

```kotlin
val one = 1 // Int
val threeBillion = 3000000000 // Long
val oneLong = 1L // Long
val oneByte: Byte = 1
```

### Floating-point - Números reais

Para números reais, o Kotlin fornece os tipos de ponto flutuantes `Float` e` Double`. De acordo com o [padrão IEEE 754](https://en.wikipedia.org/wiki/IEEE_754), os tipos de ponto flutuante diferem por seus _lugares decimais_, ou seja, quantos dígitos decimais eles podem armazenar. `Float` reflete a _precisão única_ IEEE 754, enquanto` Double` fornece _precisão dupla_.  
 
| Type	 |Tamanho (bits)|Significant bits|Exponent bits|Dígitos decimais|
|--------|-----------|--------------- |-------------|--------------|
| Float	 | 32        |24              |8            |6-7            |
| Double | 64        |53              |11           |15-16          |    
  
Você pode inicializar variáveis `Double` e` Float` com números que tenham parte fracionária, para isso utilize o ponto (`.`) para separar as frações. Para variáveis inicializadas com números fracionádos, o compilador infere o tipo `Double` por padrão. 

```kotlin
val pi = 3.14 // Double
// val one: Double = 1 // Error: type mismatch
val oneDouble = 1.0 // Double
```
Para especificar o tipo `Float` em um valor, adicione o sufixo` f` ou `F`. Se o valor adicinoado conter mais de 6-7 dígitos decimais, ele será arredondado.

```kotlin
val e = 2.7182818284 // Double
val eFloat = 2.7182818284f // Float, actual value is 2.7182817
```
Observe que, ao contrário de outras linguagens, não há conversões implícitas para números em Kotlin. Por exemplo, uma função com um parâmetro `Double`só poderá receber valores` Double`, `Float`, `Int` ou outros valores numéricos não são automáticamente convertidos.

```kotlin
fun main() {
    fun printDouble(d: Double) { 
       print(d) 
    }

    val i = 1    
    val d = 1.0
    val f = 1.0f 

    printDouble(d)
//    printDouble(i) // Error: Type mismatch
//    printDouble(f) // Error: Type mismatch
}
```
Para converter valores numéricos para tipos diferentes, use [conversões explícitas](#conversões-explícitas).

### Constantes literais
Existem os seguintes tipos de constantes literais para valores:
* Decimals: `123`
  * Longs are tagged by a capital `L`: `123L`
* Hexadecimals: `0x0F`
* Binaries: `0b00001011`

>Literais octais não são suportados.
>

Kotlin também suporta uma notação convencional para números de ponto flutuante:
* Doubles por padrão: `123.5`, `123.5e10`
* Floats são marcados por `f` or `F`: `123.5f`
 
Você pode usar underline para tornar as constantes numéricas mais legíveis:
```kotlin
val oneMillion = 1_000_000
val creditCardNumber = 1234_5678_9012_3456L
val socialSecurityNumber = 999_99_9999L
val hexBytes = 0xFF_EC_DE_5E
val bytes = 0b11010010_01101001_10010100_10010010
```

### Representação numéricas na JVM

Na plataforma JVM, os números são armazenados como tipos primitivos: `int`,` double` e assim por diante. Os casos de excessão acontecem quando você cria uma referência de número nullable, como `Int?` ou usa tipos genéricos. Nestes casos, os números são encapsulados nas classes Java `Integer`,` Double` e assim por diante.

Observe que as referências nullable para o mesmo número podem ser objetos diferentes:
```kotlin
fun main() {
    val a: Int = 100
    val boxedA: Int? = a
    val anotherBoxedA: Int? = a
    
    val b: Int = 10000
    val boxedB: Int? = b
    val anotherBoxedB: Int? = b
    
    println(boxedA === anotherBoxedA) // true
    println(boxedB === anotherBoxedB) // false
}
```
Todas as referências nullable a `a` são na verdade o mesmo objeto por causa da otimização de memória que a JVM aplica a`Integer` entre `-128` e` 127`. Já que este limite não se aplica a `b`, às referências a `b` são objetos diferentes.

Por outro lado, eles ainda são iguais:

```kotlin
fun main() {
    val b: Int = 10000
    println(b == b) // Prints 'true'
    val boxedB: Int? = b
    val anotherBoxedB: Int? = b
    println(boxedB == anotherBoxedB) // Prints 'true'
}
```

> Em Kotlin, existem dois tipos de igualdade:
> * Igualdade estrutural(`==`): checa se os valores são iguais `equals()`.
> * Igualdade referencial(`===`): checa se as referências apontam para o mesmo objeto.

### Conversões explícitas

Todos os tipos numéricos oferecem suporte de conversões para outros tipos:

* `toByte(): Byte`
* `toShort(): Short`
* `toInt(): Int`
* `toLong(): Long`
* `toFloat(): Float`
* `toDouble(): Double`
* `toChar(): Char`

Em muitos casos, se tratando de expressões arítméticas, não há necessidade de conversões explícitas porque o tipo é inferido pelo contexto, por exemplo:

```kotlin
val l = 1L + 3 // Long + Int => Long
```

### Operações aritméticas

Kotlin suporta o conjunto padrão de operações aritméticas para números: `+`, `-`,` * `,` / `,`% `. 

```kotlin
fun main() {
    println(1 + 2)
    println(2_500_000_000L - 1L)
    println(3.14 * 2.71)
    println(10.0 / 3)
    println(10.0 % 3)
}
```

#### Divisão de inteiros

A divisão entre números inteiros sempre retorna um número inteiro. Qualquer parte fracionária é descartada.

```kotlin
fun main() {
    val x = 5 / 2
    //println(x == 2.5) // ERROR: Operador '==' não pode ser aplicado a 'Int' e 'Double'
    println(x == 2)
}
```
Isso é verdade para uma divisão entre quaisquer dois tipos de inteiros.

```kotlin
fun main() {
    val x = 5L / 2
    println(x == 2L)
}
```
Para retornar um tipo ponto flutuante, converta explicitamente um dos argumentos em um tipo de ponto flutuante.

```kotlin
fun main() {
    val x = 5 / 2.toDouble()
    println(x == 2.5)
}
```

### Comparação de números de ponto flutuante

As operações de comparação com números de ponto flutuante são:

* Verificações de igualdade: `a == b` and `a != b`
* Operadores de comparação: `a < b`, `a > b`, `a <= b`, `a >= b`
* Intanciando range e checando ranges: `a..b`, `x in a..b`, `x !in a..b`

Quando os operandos `a` e` b` são `Float` ou` Double` ou suas contrapartes são anuláveis, as operações com os números e o intervalo que eles formam seguem o [Padrão IEEE 754 para Aritmética de Ponto Flutuante](https://en.wikipedia.org/wiki/IEEE_754). 

No entanto, para apoiar casos de uso genéricos e fornecer ordenação, quando os operandos **não** são tipificados estaticamente como ponto flutuante (por exemplo, `Any`,` Comparable <...> `, um parâmetro de tipo), as operações usam `equals` e` compareTo` para `Float` e` Double`, que discordam do padrão `IEEE 754`, de modo que: 

* `NaN` é considerado igual a si mesmo
* `NaN` é considerado maior do que qualquer outro elemento, incluindo `POSITIVE_INFINITY`
* `-0.0` é considerado menor que `0,0`

## Booleanos

O tipo `Boolean` representa objetos booleanos que podem ter dois valores:` true` e `false`.

`Boolean` tem uma contraparte anulável `Boolean? `que também tem o valor` null`.

As operações em booleanos incluem:

* `||` - disjunção (lógica _OR_) 
* `&&` - conjunção (lógico _AND_) 
* `!` - negação (lógico _NOT_)

```kotlin
fun main() {
    val myTrue: Boolean = true
    val myFalse: Boolean = false
    val boolNull: Boolean? = null
    
    println(myTrue || myFalse)
    println(myTrue && myFalse)
    println(!myTrue)
}
```
>**No JVM**: referências anuláveis a objetos booleanos são encapsuladas de forma semelhante a [números](#representação-numéricas-na-JVM).

## Characteres

Os caracteres são representados pelo tipo `Char`. Literais de caracteres são colocados entre aspas simples: `'1'`.
Os caracteres especiais começam com uma barra invertida de escape `\`. As seguintes sequências de escape são suportadas: `\t`, `\b`, `\n`, `\r`, `\'`, `\"`, `\\` e `\$`.

Para codificar qualquer outro caractere, use a sintaxe de seqüência de escape Unicode: `'\ uFF00'`.

```kotlin
fun main() {
    val aChar: Char = 'a'
 
    println(aChar)
    println('\n') //prints an extra newline character
    println('\uFF00')
}
```

Se um valor de variável de caractere é um dígito, você pode convertê-lo explicitamente em um número `Int` usando a função [`digitToInt()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.text/digit-to-int.html)

## Strings
Strings em Kotlin são representados pelo tipo `String`. Geralmente, um valor `string` é uma sequência de caracteres entre aspas duplas (`"`).
 
```kotlin
val str = "abcd 123"
```

Os elementos de uma string são caracteres, então você pode acessar por meio de uma operação de indexação: `s[i]`.  Você pode iterar sobre esses caracteres com um loop `for`:

```kotlin
fun main() {
  val str = "abcd"
  for (c in str) {
    println(c)
  }
}
```
Strings são imutáveis. Depois de inicializar uma string, você não pode alterar seu valor ou atribuir um novo valor a ela. Todas as operações que transformam strings retornam seus resultados em um novo objeto `String`, deixando a string original inalterada.

```kotlin
fun main() {
    val str = "abcd"
    println(str.uppercase()) // Create and print a new String object
    println(str) // the original string remains the same
}
```

Para concatenar strings, use o operador `+`. Isso também funciona para concatenar strings com valores de outros tipos, desde que o primeiro elemento na expressão seja uma string:

```kotlin
fun main() {
  val s = "abc" + 1
  println(s + "def")
}
```
Observe que, na maioria dos casos, usar [modelos de string](#inferindo-valores-em-strings) ou strings brutas é preferível à concatenação de string.

### String literais

Kotlin tem dois tipos de literais de string:

* _escaped_ strings que podem conter caracteres de escape
* _raw_ strings que podem conter novas linhas e texto arbitrário

Aqui temos um exemplo de `escaped string`:

```kotlin
val s = "Hello, world!\n"
```

O escape é feito da maneira convencional, com uma barra invertida (`\`). Consulte [Caracteres](#caracteres) para obter a lista de sequências de escape suportadas. 

Uma string bruta é delimitada por aspas triplas (`"""`), não contém escape e pode conter novas linhas e quaisquer outros caracteres:

```kotlin
val text = 
"""
    for (c in "foo")
        print(c)
"""
```

Para remover o espaço em branco inicial de strings brutas, use a função [`trimMargin ()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.text/trim-margin.html): 

```kotlin
val text = 
"""
    |Tell me and I forget.
    |Teach me and I remember.
    |Involve me and I learn.
    |(Benjamin Franklin)
""".trimMargin()
```

Por padrão o caractere `|` é usado como prefixo de margem, mas você pode escolher outro caractere e passá-lo como um parâmetro, como `trimMargin(">")`.

### Inferindo valores em strings

Literais de string podem conter expressões _template_ - pedaços de código que são evaluados e cujos resultados são concatenados na string. Uma expressão modelo começa com um cifrão (`$`) e consiste em:

```kotlin
fun main() {
    val i = 10
    println("i = $i") // prints "i = 10"
}
```

ou uma expressão entre chaves:

```kotlin
fun main() {
    val s = "abc"
    println("$s.length is ${s.length}") // prints "abc.length is 3"
}
```

## Arrays

Arrays em Kotlin são representados pela classe `Array`. Ele tem as funções `get` e` set` que se transformam em `[]` por convenções de sobrecarga de operador, e contem a propriedade `size`, junto com outras funções úteis: 

```kotlin
class Array<T> private constructor() {
    val size: Int
    operator fun get(index: Int): T
    operator fun set(index: Int, value: T): Unit

    operator fun iterator(): Iterator<T>
    // ...
}
```

Para criar um Array, use a função `arrayOf ()` e passe os valores dos itens para ela, a inicialização `arrayOf(1, 2, 3)` crie o array `[1, 2, 3]`.  Alternativamente, a função `arrayOfNulls ()` pode ser usada para criar um array de um determinado tamanho preenchido com elementos `null`. Outra opção é usar o construtor `Array` que pega o tamanho do array e a função que retorna os valoresdos  elementos do array dado seu índice:

```kotlin
fun main() {
    // Creates an Array<String> with values ["0", "1", "4", "9", "16"]
    val asc = Array(5) { i -> (i * i).toString() }
    asc.forEach { println(it) }
}
```
Como dissemos acima, a operação `[]` significa chamadas para as funções-membro `get ()` e `set ()`.

Arrays em Kotlin são _invariant_. Isso significa que no Kotlin, não é permitido atribuir um `Array <String>` a um `Array <Any>`, o que evita uma possível falha de tipo em tempo de execução (mas você pode usar `Array <out Any>`).

### Arrays de tipos primitivos

Kotlin também tem classes que representam matrizes de tipos primitivos sem sobrecarga : `ByteArray`, `ShortArray`,` IntArray` e assim por diante. Essas classes não têm relação de herança com a classe `Array`, mas elas têm o mesmo conjunto de métodos e propriedades. Cada um deles também tem uma função de inicialização correspondente:

```kotlin
val x: IntArray = intArrayOf(1, 2, 3)
x[0] = x[1] + x[2]
```

```kotlin
// Array of int of size 5 with values [0, 0, 0, 0, 0]
val arr = IntArray(5)

// e.g. initialise the values in the array with a constant
// Array of int of size 5 with values [42, 42, 42, 42, 42]
val arr = IntArray(5) { 42 }

// e.g. initialise the values in the array using a lambda
// Array of int of size 5 with values [0, 1, 2, 3, 4] (values initialised to their index value)
var arr = IntArray(5) { it * 1 } 
```
