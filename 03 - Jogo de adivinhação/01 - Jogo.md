# Jogo de Adivinhação

## Juntando Um Pouquinho de Tudo

Agora vamos criar um jogo de adivinhação juntos, para aprender um pouco mais sobre variáveis, métodos, etc.. Vamos ver detalhadamente mais tarde, por agora vamos  apenas praticar os fundamentos.

Vamos criar um programa básico que vai gerar um número inteiro entre 1 e 100 e vai pedir para o usuário digitar um chute, depois vai dizer se o chute está alto ou baixo, se for o chute correto vai ser impresso parabéns no terminal e terminar o programa.

Para começar vamos criar um novo projeto com Cargo, como você já aprendeu anteriormente.

```console
$ cargo new guessing_game
$ cd guessing_game
```

E agora dentro do diretório _src/main.rs_, que o Cargo cria por padrão, vamos digitar o seguinte código:

```rust
use std::io;

fn main() {
    println!("Guess the number!");

    println!("Please input your guess.");

    let mut guess = String::new();

    io::stdin()
        .read_line(&mut guess)
        .expect("Failed to read line");

    println!("You guessed: {guess}");
}
```


### Vamos passar agora por cada linha do código

Para conseguir o número que nosso usuário digitar precisamos da biblioteca ```io``` que vem da biblioteca ```standart```, conhecida como ```std```:

```rust
use std::io;
```

Por padrão, o Rust tem um set de itens definidos na biblioteca standard, que traz isso para o escopo de todo programa. Esse set é chamado de _prelude_.

Se você quiser algo que não está no _prelude_, precisa trazer isso para o escopo usando a declaração ```use```. Usar a biblioteca ```std:io``` traz várias features úteis. Como a habilidade de aceitar entradas do usuário.

Agora temos a função ``main`` que como vimos anteriormente, é a porta de entrada para todo programa em Rust.

```rust
fn main() {
```

A sintaxe ```fn```declara uma nova função.

Depois usamos o macro ```println!``` para imprimir na tela a string.

```rust
    println!("Guess the number!");

    println!("Please input your guess.");
```


#### Armazenando Valores com Variáveis. 

Agora criamos a variável que irá armazenar a entrada do usuário, assim:

```rust
let mut guess = String::new();
```

Por padrão, as variáveis no Rust são imutáveis, isso significa que uma vez que damos um valor para a variável, esse valor não irá mudar, e para criar uma variável mutável usamos a palavra ```mut``` antes do nome da variável:

```rust
let bananas = 5; // imutavel
let mut bananas = 5 // mutavel
```

Voltando ao nosso jogo de adivinhação, sabemos que ```let mut guess``` vai iniciar uma variável mutável chamada ```guess```. O símbolo de = diz ao Rust que queremos vincular algo a aquela variável, assim associamos aquela variável ao resultado de ```String::new```, uma função que retorna uma instancia de String, um tipo provido pela biblioteca standard.  Que é um pedaço de texto codificado em UTF-8 dinâmico, que pode crescer. 

O ```::``` na linha ```::new``` indica que ```new```é uma função associada ao tipo String, uma função associada é uma função que é implementada em um tipo, nesse caso String. A função ```new``` cria uma nova string vazia.


#### Recebendo entrada do usuário

Agora vamos usar o módulo a função ```stdin```  do módulo ```io``` que importamos lá no começo, ele nos permite lidar com a entrada do usuário.

```rust
    io::stdin()
        .read_line(&mut guess)
```

Agora a linha ```.read_line(&mut guess)``` chama o método ```read_line``` que pega a entrada do usuário e coloca isso em uma string, depois passamos ```&mut guess``` como argumento para dizer onde vamos armazenar a entrada do usuário.

O ```&``` indica que esse argumento é uma referencia, que nos da um jeito acessar um dado na memória sem precisar copiar copiar ele várias vezes. Também veremos mais a fundo depois.

A função ```read_line``` também retorna um resultado, que é ```Ok``` ou ```Err``` e precisamos lidar com isso, esse resultado tem métodos definidos nele, como o método ```expect``` que é chamado caso a instancia do resultado seja um valor ```Err```, ele vai crashar o programa e mostrar a mensagem que você passou como argumento na tela, assim como fizemos no código

```rust
.expect("Failed to read line");
```

Caso retorne um ```Ok``` o _expect_ irá retornar, nesse caso, a quantidade de bytes na entrada do usuário. 

Se você não colocar o ```expect``` o programa vai compilar mas vai mostrar um aviso dizendo que o programa não vai lidar com um possível erro.

#### Imprimindo Valores com println!

Agora na próxima linha temos:

```rust
    println!("You guessed: {guess}");
```

Essa linha imprime a string que contém a entrada do usuário, o { } podemos pensar como as garras de um caranguejo que seguram um valor no seu lugar. quando queremos imprimir o valor de uma variável podemos colocar ela dentro dos {} assim ```{guess}``` mas também podemos deixar {} vazios e depois passar as variáveis em ordem: 

```rust
let x = 5;
let y = 10;

println!("x = {x} and y + 2 = {}", y + 2);
```

Pronto! Agora você já tem a primeira parte do programa pronto e entende como ela funciona, se rodar usando ```cargo run``` você poderá digitar um número e ver ele impresso no seu terminal.

#### Usando Crate

Agora, para dar continuidade ao programa vamos ter que gerar o nosso número aleatório, para isso vamos usar uma _biblioteca crate_ (crate é o termo do Rust para um binário ou uma biblioteca), que contém o código que pode ser usado em outros programas. 

Para adicionar vamos ao nosso arquivo _Cargo.toml_ e vamos escrever:

```toml
[dependencies] 
rand = "0.8.5"
```

Assim dizemos ao Cargo quais dependências externas queremos usar no nosso projeto. 

Agora se você rodar ```cargo build``` vai ver uma diferença no que aparece no terminal, pois o Cargo vai baixar e compilar todas as dependências novas.

Quando instalamos crates e especificamos a versão ela ficará salva no _Cargo.lock_ e mesmo que saiam novas versões das dependências elas não serão alteradas se você não alterar explicitamente no _Cargo.toml_ ou usando ```cargo update```.

```console
$ cargo update
    Updating crates.io index
    Updating rand v0.8.5 -> v0.8.6

```

Dessa forma ele irá ignorar as versões 0.9.0, e se quiser vai ter que alterar no _Cargo.toml_:

```toml
[dependencies]
rand = "0.9.0"
```

E na próxima vez que você rodar ```cargo build``` o Cargo vai atualizar o registro das crates e reavaliar seus requisitos de acordo com a nova versão. 

## Gerando Número Aleatório

Agora vamos usar a _crate rand_ para gerar um número aleatório:

```rust
use std::io;
use rand::Rng;

fn main() {

	println!("Guess the number!");

	let secret_number = rand::thread_rng().gen_range(1..=100);

	println!("The secret number is: {secret_number}");

	println!("Please input your guess:");

	let mut guess = String::new();

	io::stdin()
		.read_line(&mut guess)
		.expect("Failed to read line");

	println!("You guessed: {guess}"); 
}
```

Assim geramos um número aleatório no range de 1 a 100 e armazenamos na variável imutável ```secret_number```, depois mostramos o resultado na tela.

Vamos precisar comparar o valor de ```guess``` com o ```secret_number``` para saber se o jogador acertou ou errou, mas para isso primeiro temos que transformar o ```guess``` que inicialmente recebe uma string, em número, assim:

```rust
let guess: u32 = guess.trim().parse().expect("Invalid number");
```

Especificamos que ```guess``` será do tipo u32, vamos ver isso mais pra frente, por agora saiba que é um número positivo, e caso o valor de ```guess``` digitado seja um número válido vamos fazer o parse, caso contrário o programa finalizará com o erro especificado "Invalid number".

Após essa verificação, para finalizar o jogo vamos usar as expressões condicionais if/else, comparando se ```guess``` é igual a ```secret_number```, se sim, vai imprimir na tela que o jogador ganhou, se não, vai ser impresso que o jogador perdeu.

```rust
if guess == secret_number {
	println!("You win!");
} else {
	println!("You lost!");
}
```

Pronto, seu jogo de adivinhação feito em rust está finalizado, agora vamos rodar com ```cargo run```!

Código final:

```rust
use std::io;
use rand::Rng;

fn main() { 
	println!("Guess the number!");
	
	let secret_number = rand::thread_rng().gen_range(1..=100);

	println!("The secret number is: {secret_number}");

	println!("Please input your guess:");

	let mut guess = String::new();

	io::stdin()
		.read_line(&mut guess)
		.expect("Failed to read line");
		
	let guess: u32 = guess.trim().parse().expect("Invalid number");

	println!("You guessed: {guess}");

	if guess == secret_number {
		println!("You win!");
	} else {
		println!("You lost!");
	}

}
```

