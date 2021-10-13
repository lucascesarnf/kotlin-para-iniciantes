# Sintaxe básica

> Para uma melhor fixação dos conceitos aqui descritos, recomendamos que todos os tópicos sejam testados e alterados no [Kotlin Playground](https://play.kotlinlang.org/). 

## Definição de pacote e importações
As especificaçoes dos pacotes precisam estar no topo do arquivo.
```kotlin
package my.demo

import kotlin.text.*

// ...
```
## Ponto de partida do código
A função `main` serve como o ponto de partida para a execução do programa. Em geral, ela controla a execução direcionando as chamadas para outras funções no programa.
```kotlin
fun main() {
    println("Hello world!")
}
```
## Imprimir na saída padrão 
A função `print`, basicamente, exibe mensagens na tela de saída. 
```kotlin
print("Hello ")
print("world!")

/*
saída: 
Hello world!
*/
```
A função `println`, faz o mesmo que a função print, porém, pula uma linha logo após o texto. 
```kotlin
println("Hello ")
println("world!")

/*
saída: 
Hello 
world!
*/
```
## Funções
Uma função nada mais é do que uma sub-rotina usada em um programa.
Neste exemplo temos uma função que recebe dois parâmetros do tipo `Int` e retorna um tipo `Int`:

> Entenda melhor sobre os [tipos](02-tipos-de-dados-primitivos.md) em Kotlin
```kotlin
fun sum(a: Int, b: Int): Int {
    return a + b
}

fun main() {
    print("sum of 3 and 5 is: ")
    println(sum(3, 5))
}

/*
saída: 
sum of 3 and 5 is: 8
*/
```
O corpo de uma função pode ser uma expressão, que retorna o tipo através de uma [inferência de tipo](https://kotlinlang.org/spec/type-inference.html).

```kotlin
fun sum(a: Int, b: Int) = a + b


fun main() {
    println("sum of 19 and 23 is ${sum(19, 23)}")
}

/*
saída: 
sum of 19 and 23 is 42
*/
```

Uma função pode não retornar nenhum valor significativo.

```kotlin

fun printSum(a: Int, b: Int): Unit {
    println("sum of $a and $b is ${a + b}")
}

fun main() {
    printSum(-1, 8)
}

/*
saída: 
sum of -1 and 8 is 7
*/
```

Você ainda pode omitir o retorno do tipo`Unit` .

```kotlin
fun printSum(a: Int, b: Int) {
    println("sum of $a and $b is ${a + b}")
}

fun main() {
    printSum(-1, 8)
}

/*
saída: 
sum of -1 and 8 is 7
*/
```

## Variáveis
Variáveis locais que são definidas apenas para leitura são declaradas utilizando a palavra `val`. Variáveis exclusivas para leitura(Read-only) não poderão sofrer alterações após sua declaração. 

```kotlin
fun main() {
    val a: Int = 1  // atribuição imediata
    val b = 2   // tipo `Int` inferido
    val c: Int  // é necessário declarar o tipo da varoável quando nenhum inicializador é fornecido
    c = 3       // atribuiçao feita posteriormente
    println("a = $a, b = $b, c = $c")
}
/*
saída: 
a = 1, b = 2, c = 3
*/
```
Variáveis que podem ser alteradas durante a execução do programa utilizam a palavra `var`.

```kotlin
fun main() {
    var x = 5 // `Int` type is inferred
    x += 1
    println("x = $x")
}
/*
saída: 
x = 6
*/
```
Você pode declarar variáveis de escopo superior.
```kotlin
val PI = 3.14
var x = 0

fun incrementX() { 
    x += 1 
}

fun main() {
    println("x = $x; PI = $PI")
    incrementX()
    println("incrementX()")
    println("x = $x; PI = $PI")
}

/*
saída: 
x = 0; PI = 3.14
incrementX()
x = 1; PI = 3.14
*/
```

## Criando classes e instâncias

Para definir uma classe, use a palavra-chave `class`.

```kotlin
class Shape
```

As propriedades de uma classe podem ser listadas em sua declaração ou no seu escopo.

```kotlin
class Rectangle(var height: Double, var length: Double) {
    var perimeter = (height + length) * 2 
}

fun main() {
    val rectangle = Rectangle(5.0, 2.0)
    println("The perimeter is ${rectangle.perimeter}")
}
```

A herança entre as classes é declarada por dois pontos (`:`). As classes são `final` por padrão; para tornar uma classe herdável, utilize o prefixo `open`.

```kotlin
open class Shape

class Rectangle(var height: Double, var length: Double): Shape() {
    var perimeter = (height + length) * 2 
}
```

## Comentários
Assim como a maioria das linguagens modernas, o Kotlin suporta comentários de uma linha (ou _end-of-line_) e multilinhas (_block_). 
```kotlin
// This is an end-of-line comment

/* This is a block comment
   on multiple lines. */
```

Os comentários de bloco no Kotlin podem ser aninhados.
```kotlin
/* The comment starts here
/* contains a nested comment *&#8288;/     
and ends here. */
```

## Inferindo valores em strings

```kotlin
fun main() {
    var a = 1
    // simple name in template:
    val s1 = "a is $a" 
    
    a = 2
    // arbitrary expression in template:
    val s2 = "${s1.replace("is", "was")}, but now is $a"
    println(s2)
}
```

## Expressões condicionais

Em Kotlin, `if` é uma expressão que retorna um valor. Portanto, não há operador ternário (`condição? Então: senão`) porque o `if` comum funciona bem nesta função.

```kotlin
var max = a 
if (a < b) max = b

// Com else 
var max: Int
if (a > b) {
    max = a
} else {
    max = b
}
 
// Como expressão
val max = if (a > b) a else b
```
Ramificações de ramificações `if` podem ser blocos. Nesse caso, a última expressão é o valor de um bloco:

```kotlin
val max = if (a > b) {
    print("Choose a")
    a
} else {
    print("Choose b")
    b
}
```
Se você estiver usando `if` como uma expressão, por exemplo, para retornar seu valor ou atribuindo-o a uma variável, o `else` é obrigatório.

### Expressão When 
`when` define uma expressão condicional com vários ramos. É semelhante à instrução `switch` em linguagens como o C. Sua forma simples é parecida com esta:

```kotlin
fun describe(obj: Any): String =
    when (obj) {
        1          -> "One"
        "Hello"    -> "Greeting"
        is Long    -> "Long"
        !is String -> "Not a string"
        else       -> "Unknown"
    }

fun main() {
    println(describe(1))
    println(describe("Hello"))
    println(describe(1000L))
    println(describe(2))
    println(describe("other"))
}
```

`when` verifica todas as condições sequêncialmente até que alguma delas seja satisfeita. `when` pode ser usado como uma expressão ou como uma declaração. Assim como com `if`, cada validação pode ser um bloco, e seu valor é o valor da última expressão do bloco. A validação `else` é avaliado se nenhuma das outras condições do branch for satisfeita. Se `when` é usado como uma expressão, o ` else` é obrigatório, a menos que o compilador possa provar que todos os casos possíveis são cobertos com condições.

```kotlin
enum class Bit {
  ZERO, ONE
}

val numericValue = when (getRandomBit()) {
    Bit.ZERO -> 0
    Bit.ONE -> 1
    // the 'else' clause is not required because all cases are covered
}
```
Para definir um comportamento comum para vários casos, combine suas condições em uma única linha com uma vírgula:

```kotlin
when (x) {
    0, 1 -> print("x == 0 or x == 1")
    else -> print("otherwise")
}
```
Você pode usar expressões arbitrárias (não apenas constantes) como condições de ramificação:

```kotlin
when (x) {
    s.toInt() -> print("s encodes x")
    else -> print("s does not encode x")
}
```
## Loops

O loop `for` itera por em qualquer sequencia que aceite um iterador. Isso é equivalente ao loop `foreach` em linguagens como C.
A sintaxe de `for` é a seguinte:
```kotlin
for (item in collection) print(item)
```

O corpo de `for` pode ser um bloco:
```kotlin
for (item: Int in ints) {
    // ...
}
```
Como mencionado antes, `for` itera por meio de qualquer coisa que forneça um iterador. Isso significa que:
* tem um membro ou uma função de extensão `iterator ()` que retorna `Iterator <>`: 
  * tem um membro ou uma função de extensão `next ()` 
  * tem um membro ou uma função de extensão `hasNext ()` que retorna `Boolean`.


Todas essas três funções precisam ser marcadas como `operador`.

```kotlin
fun main() {
    for (i in 1..3) {
        println(i)
    }
    for (i in 6 downTo 0 step 2) {
        println(i)
    }
}
```
Um loop `for` em um intervalo ou em um array é compilado em um loop baseado em índices, e não cria um objeto iterador.

Se quiser iterar por meio de um array ou lista com um índice, você pode fazer isso desta forma:

```kotlin
fun main() {
val array = arrayOf("a", "b", "c")

    for (i in array.indices) {
        println(array[i])
    }

}
```

Como alternativa, você pode usar a função `withIndex`:

```kotlin
fun main() {
    val array = arrayOf("a", "b", "c")

    for ((index, value) in array.withIndex()) {
        println("the element at $index is $value")
    }

}
```
### While loop

Os loops `while` e` do-while` executam seus blocos continuamente enquanto sua condição é satisfeita. A diferença entre eles é o tempo de verificação da condição: 
* `while` verifica a condição e, se for satisfeita, executa o bloco, e então retorna para a verificação da condição. 
* `do-while` executa o corpo e então verifica a condição. Se estiver satisfeito, o loop se repete. Assim, o corpo do `do-while` é executado pelo menos uma vez, independentemente da condição.

```kotlin
while (x > 0) {
    x--
}

do {
    val y = retrieveData()
} while (y != null) // y is visible here!
```

## Break e continue em loops

Kotlin suporta operadores tradicionais `break` e` continue` em loops.

```kotlin
fun main() {

    val items = listOf("apple", "banana", "kiwifruit")
    var index = 0
    while (index < items.size) {
        println("item at $index is ${items[index]}")
        index++
    }

}
```
## Ranges

CVerifique se um número está dentro de um intervalo usando o operador `in`.

```kotlin
fun main() {

    val x = 10
    val y = 9
    if (x in 1..y+1) {
        println("fits in range")
    }

}
```

Verifique se um número está fora do intervalo.

```kotlin
fun main() {

    val list = listOf("a", "b", "c")
    
    if (-1 !in 0..list.lastIndex) {
        println("-1 is out of range")
    }
    if (list.size !in list.indices) {
        println("list size is out of valid list indices range, too")
    }

}
```

Itere sobre um intervalo.

```kotlin
fun main() {

    for (x in 1..5) {
        print(x)
    }

}
```

Ou sobre uma progressão.

```kotlin
fun main() {

    for (x in 1..10 step 2) {
        print(x)
    }
    println()
    for (x in 9 downTo 0 step 3) {
        print(x)
    }

}
```

## Coleções

Itera sobre uma coleção.

```kotlin
fun main() {
    val items = listOf("apple", "banana", "kiwifruit")

    for (item in items) {
        println(item)
    }
}
```

Verifique se uma coleção contém um objeto usando o operador `in`.

```kotlin
fun main() {
    val items = setOf("apple", "banana", "kiwifruit")

    when {
        "orange" in items -> println("juicy")
        "apple" in items -> println("apple is fine too")
    }
}
```

Usando expressões lambda para filtrar e mapear coleções:

```kotlin
fun main() {
    val fruits = listOf("banana", "avocado", "apple", "kiwifruit")
    fruits
      .filter { it.startsWith("a") }
      .sortedBy { it }
      .map { it.uppercase() }
      .forEach { println(it) }
}
```

## Valores anuláveis e verificações de nulos

Uma referência deve ser marcada explicitamente como anulável quando o valor `null` é uma possibilidade. Variáveis do tipo anuláveis têm `?` no final. Retorna `null` se` str` não tiver um número inteiro:

```kotlin
fun parseInt(str: String): Int? {
    // ...
}
```

Use uma função que retorne um valor anulável:

```kotlin
fun parseInt(str: String): Int? {
    return str.toIntOrNull()
}

fun printProduct(arg1: String, arg2: String) {
    val x = parseInt(arg1)
    val y = parseInt(arg2)

    // Using `x * y` yields error because they may hold nulls.
    if (x != null && y != null) {
        // x and y are automatically cast to non-nullable after null check
        println(x * y)
    }
    else {
        println("'$arg1' or '$arg2' is not a number")
    }    
}

fun main() {
    printProduct("6", "7")
    printProduct("a", "7")
    printProduct("a", "b")
}
```

ou

```kotlin
fun parseInt(str: String): Int? {
    return str.toIntOrNull()
}

fun printProduct(arg1: String, arg2: String) {
    val x = parseInt(arg1)
    val y = parseInt(arg2)
    
    // ...
    if (x == null) {
        println("Wrong number format in arg1: '$arg1'")
        return
    }
    if (y == null) {
        println("Wrong number format in arg2: '$arg2'")
        return
    }

    // x and y are automatically cast to non-nullable after null check
    println(x * y)
}

fun main() {
    printProduct("6", "7")
    printProduct("a", "7")
    printProduct("99", "b")
}
```

## Verificações de tipo e conversão automática

O operador `is` verifica se uma expressão é uma instância de um determinado tipo. 

```kotlin
fun getStringLength(obj: Any): Int? {
    if (obj is String) {
        // `obj` is automatically cast to `String` in this branch
        return obj.length
    }

    // `obj` is still of type `Any` outside of the type-checked branch
    return null
}

fun main() {
    fun printLength(obj: Any) {
        println("Getting the length of '$obj'. Result: ${getStringLength(obj) ?: "Error: The object is not a string"} ")
    }
    printLength("Incomprehensibilities")
    printLength(1000)
    printLength(listOf(Any()))
}
```
ou

```kotlin
fun getStringLength(obj: Any): Int? {
    if (obj !is String) return null

    // `obj` is automatically cast to `String` in this branch
    return obj.length
}

fun main() {
    fun printLength(obj: Any) {
        println("Getting the length of '$obj'. Result: ${getStringLength(obj) ?: "Error: The object is not a string"} ")
    }
    printLength("Incomprehensibilities")
    printLength(1000)
    printLength(listOf(Any()))
}
```

ou ainda

```kotlin
fun getStringLength(obj: Any): Int? {
    // `obj` is automatically cast to `String` on the right-hand side of `&&`
    if (obj is String && obj.length > 0) {
        return obj.length
    }

    return null
}

fun main() {
    fun printLength(obj: Any) {
        println("Getting the length of '$obj'. Result: ${getStringLength(obj) ?: "Error: The object is not a string"} ")
    }
    printLength("Incomprehensibilities")
    printLength("")
    printLength(1000)
}
```
