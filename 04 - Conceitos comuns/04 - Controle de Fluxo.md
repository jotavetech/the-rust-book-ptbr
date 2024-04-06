# Controle de Fluxo

A maioria das estruturas em Rust que vão te permitir controlar o fluxo do seu programa, se algo for verdadeiro ou falso, são as expressões ```if``` e loops.

### Expressões if

Uma expressão ```if``` te permite fazer algo dependendo a condição que você passar pra ela, se estiver chovendo, não saia de casa, se não estiver chovendo, vá passear.

```rust
let is_raining = false;

if is_raining { // se estiver chovendo rode esse bloco de código:
	println!("Fique em casa!");
} else { // se não, rode este:
	println!("Vá passear");
}
```

Uma expressão ```if``` recebe uma condição para checar se ela é verdadeira, caso sim, ela roda o bloco de código. No nosso exemplo criamos uma variável is_raining e associamos ela a um valor booleano de _false_, depois verificamos: is_raining é verdadeiro? (true), se sim mostre na tela **Fique em casa**, caso contrário, mostre na tela **Vá passear***. 

Podemos fazer comparações com operadores lógicos também, como ```a == b```, que verifica se ```a``` é igual a ```b```. Outro exemplo ```a > b```, que verifica se ```a``` é maior que ```b```.

Também podemos verificar condições múltiplas usando ```else if```: 

```rust
fn main() {
    let number = 6;

    if number % 4 == 0 {
        println!("number é divisivel por 4");
    } else if number % 3 == 0 {
        println!("number é divisivel por 3");
    } else if number % 2 == 0 {
        println!("number é divisivel por 2");
    } else {
        println!("number é divisivel por 4, 3, ou 2");
    }
}
```

Também podemos usar a declaração ```if``` para retornar valores caso uma condição seja verdadeira e associar ele à variável:

```rust
fn main() {
    let condition = true;
    // se a condição for verdadeira retorna 5, se não, retorna 6.
    let number = if condition { 5 } else { 6 }; 

    println!("O valor de number é: {number}");
}
```

Se tentarmos retornar um número no ```if``` e uma string no ```else``` vai gerar um erro, porque variáveis só podem ter um tipo e o compilador precisa saber qual é esse tipo na hora de compilar o código. 

```rust
fn main() {
    let condition = true;

    let number = if condition { 5 } else { "six" };

    println!("The value of number is: {number}");
}
```


### Repetição com Loops

A palavra ```loop``` diz ao Rust para executar um bloco de código até você pedir explicitamente para parar.

No exemplo abaixo, o código será executado infinitamente, até pararmos ele manualmente:

```rust
fn main() {
    loop {
        println!("de novo!");
    }
}
```

Para poder parar um laço de repetição ```loop``` usamos a palavra ```break``` e podemos passar o valor a ser retornado depois dessa palavra:

```rust
fn main() {
    let mut counter = 0;

    let result = loop {
        counter += 1;

        if counter == 10 {
            break counter * 2;
        }
    };

    println!("O resultado é {result}");
}
```

Na função acima criamos um loop que vai aumentar o valor de ```counter``` toda vez que passar, e sempre que rodar o bloco de código ele vai verificar se ```counter``` é 10, se sim, vai parar o loop com a palavra ```break``` e retornar o valor de ```counter``` multiplicado por 2 (10 x 2 = 20). Agora a variável result vai ter o valor 20 associado a ela.

#### Loops Condicionais com While

Um programa geralmente precisa de uma condição para continuar com um loop, enquanto a condição for verdadeira o loop roda. E quando a condição deixa de ser verdadeira o loop para.

```rust
fn main() {
    let mut number = 3;

    while number != 0 {
        println!("{number}!");

        number -= 1;
    }

    println!("Saiu do loop!!!");
}
```

No loop ```while``` a cima passamos a condição de que se ```number``` for diferente de zero o bloco de código será rodado, então para podermos parar o loop, toda vez que ela passa no bloco de código diminuímos o valor de ```number``` em -1 até chegar a 0 e parar o loop. Enquanto a condição for ```true``` o loop roda, quando deixar de ser ```true``` o loop para.

Outra alternativa para executar loops e algum código em cada item de uma coleção, por exemplo, é o ```for``` loop.

```rust
fn main() {
    let a = [10, 20, 30, 40, 50];

    for element in a {
        println!("O valor do elemento é: {element}");
    }
}
```

O código acima passa uma coleção de números e o que ele faz e passar por todos os números dessa coleção e associar cada index que passa em ordem, na primeira vez que passar ```element``` será o valor do index 0 de ```a``` (10), na segunda vez que passar, ```element``` será o valor do index 1 de ```a``` (20), e por assim vai..

Ele é mais seguro e consistente, pois cuida sozinho, por exemplo, do tamanho de um array e sabe exatamente quantas vezes temos que passar. 

Também podemos passar um range de valores, assim:

```rust
fn main() {
    for number in (1..4) {
        println!("{number}!");
    }
    println!("Saiu do loop!!!");
}
```

