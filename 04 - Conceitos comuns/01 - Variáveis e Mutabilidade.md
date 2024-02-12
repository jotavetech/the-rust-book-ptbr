# Variáveis e Mutabilidade

Por padrão, variáveis no Rust são imutáveis. Isso é uma das cutucadas que o Rust da para escrever códigos seguros. 

Se você tentar rodar o código abaixo, terá um erro no terminal:

```rust
fn main() { 
	let x = 5; 
	println!("The value of x is: {x}"); 
	x = 6; 
	println!("The value of x is: {x}"); 
}
```

O erro será porque você tentou modificar o valor de uma variável imutável. Esses erros na hora de compilar são importantes e nos ajudam a prevenir de bugs depois que nossa aplicação já estiver compilada. Se uma parte do seu código assumir que a variável não vai mudar mas em outra parte o nosso código mudar esse valor, é possível que não rode como o esperado, vai ser difícil de rastrear esse bug.

Mas mutabilidade pode ser bem útil, para criar uma variável mutável você precisa adicionar ```mut``` antes do nome da variável, isso também indica para quem estiver lendo o código que aquela variável vai ser alterada futuramente:

```rust
fn main() {
    let mut x = 5;
    println!("The value of x is: {x}");
    x = 6;
    println!("The value of x is: {x}");
}
```

### Constantes

Assim como variáveis imutáveis, constantes são valores que são designados para serem de um tipo e não podem ser alterados. Mas há algumas diferenças entre constantes e variáveis:

- Primeiramente você não pode usar ```mut``` com constantes, elas sempre serão imutáveis, e para criar uma constante você usa ```const``` ao invés de ```let```, seu tipo de valor sempre será anotado, mas vamos ver mais pra frente.

- Constantes podem ser declaradas em qualquer escopo, inclusive no global. O que faz elas serem úteis para serem usadas em várias partes do código.

- A última diferença é que constantes só podem ser associadas a uma expressão constante, e não o resultado de algo que só vai ser computado na hora de rodar o código.

```rust
const THREE_HOURS_IN_SECONDS: u32 = 60 * 60 * 3;
```

Operações em constantes são feitas na hora de compilar o código e salvando os valores no seu binário, deixando assim o código mais rápido. 

### Shadowing

Como vimos antes, podemos declarar mais de uma variável com o mesmo nome, popularmente é falado que a primeira variável é _sombreada_ pela segunda, isso quer dizer que a segunda variável é o que o compilador vai ver quando você usar o nome da variável. Em efeito, a segunda variável ofusca a primeira. Nós podemos sombrear uma variável usando o mesmo nome dela e repetindo o uso da keyword.

```rust
fn main() {
    let x = 5;

    let x = x + 1;

    {
        let x = x * 2;
        println!("The value of x in the inner scope is: {x}"); // 12
    }

    println!("The value of x is: {x}"); // 6
}
```

Sombrear (shadowing) é como alterar uma variável mas sem alterar, por exemplo no código acima, aonde depois de sair do outro escopo eu queria continuar com o valor que x teve no começo. 

Também colocar tipos diferentes nas variáveis:
```rust
let x = 10;
let x = "X is a string";
let x = true;
```

