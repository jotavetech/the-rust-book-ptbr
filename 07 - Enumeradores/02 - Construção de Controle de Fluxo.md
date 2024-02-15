# Construção de Controle de Fluxo match

Rust tem um controle de fluxo chamado ```match``` que permite você comparar um valor com uma série de padrões e executar um código com base no padrão correspondente. 

Padrões ```patterns``` podem ser feitos por valores literais, nomes de variáveis, e outras coisas. 

Pense que uma expressão ```match``` é como uma máquina de sortear moedas:

```rust
enum Coin {
    Penny,
    Nickel,
    Dime,
    Quarter,
}

fn value_in_cents(coin: Coin) -> u8 {
    match coin {
        Coin::Penny => 1,
        Coin::Nickel => 5,
        Coin::Dime => 10,
        Coin::Quarter => 25,
    }
}
```

Primeiros chamamos a palavra ```match``` seguida de uma expressão, que é o valor ```coin```. É bem parecido com o ```if```, mas tem uma diferença: com o ```if```, a condição precisa avaliar um valor booleano, mas aqui pode ser qualquer tipo. O tipo de ```coin``` nesse exemplo é o que definimos no Enum.

Agora nos braços do ```match```. Um braço tem duas partes, um padrão e um código, o primeiro padrão é o valor que espera ```Coin::Penny``` e depois vem o operador ```=>``` que separa o padrão do código para rodar, nesse caso, o código apenas retorna 1, caso seja do padrão Penny.

Quando uma expressão ```match``` executa ele compara o valor com o padrão de cada braço, em ordem. Se um pattern encontra o valor, ele executa o código, se não, continua procurando. 

Como era apenas um retorno o que estávamos fazendo não usamos {}, mas podemos escrever múltiplas linhas de código também: 

```rust
fn value_in_cents(coin: Coin) -> u8 {
    match coin {
        Coin::Penny => {
            println!("Lucky penny!");
            1
        }
        Coin::Nickel => 5,
        Coin::Dime => 10,
        Coin::Quarter => 25,
    }
}
```


## Patterns Que Vinculam a Valores

Outra feature útil dos braços do match é que eles podem vincular as partes dos valores que correspondem ao padrão. 

Vamos mudar a variante ```Quarter``` para incluir algum valor guardado dentro de ```UsState```: 

```rust
enum UsState {
    Alabama,
    Alaska,
    // --snip--
}

enum Coin {
    Penny,
    Nickel,
    Dime,
    Quarter(UsState),
}

fn main() {}
```

Quando um ```Coin::Quarter``` corresponde, a variável ```state``` vai estar vinculada ao valor do estado de quarters, então podemos usar o ```state``` nos braços: 

```rust
fn value_in_cents(coin: Coin) -> u8 {
    match coin {
        Coin::Penny => 1,
        Coin::Nickel => 5,
        Coin::Dime => 10,
        Coin::Quarter(state) => {
            println!("State quarter from {:?}!", state);
            25
        }
    }
}
```


## Matching com Option

Podemos manusear o Option, visto na seção anterior, usando ```match```, como fizemos com o enum de ```coin```. Ao invés de comparar coins, vamos comparar as variantes de ```Option```.

Vamos criar uma função que pega um ```Option<i32>```, e se tiver algum valor, atribui 1 para ele, se não retorna o valor de ```None``` e não faz nenhuma ação. 

```rust
    fn plus_one(x: Option<i32>) -> Option<i32> {
        match x {
            None => None,
            Some(i) => Some(i + 1),
        }
    }

    let five = Some(5);
    let six = plus_one(five);
    let none = plus_one(None);
```


## Matches São Exaustivos 

Os braços de um ```match``` tem que cobrir todas as possibilidades. 

Por exemplo, no código aqui, a falta de cobrir o pattern ```None``` vai gerar um erro e não vai compilar: 

```rust
    fn plus_one(x: Option<i32>) -> Option<i32> {
        match x {
            Some(i) => Some(i + 1),
        }
    }
```

O Rust sabe que não cobrimos todas as possibilidades, e esquecemos um padrão, isso nos previne de ter um valor não tratado. 


### Padrões Abrangentes e Espaços _ Reservados.

Digamos que queremos rolar um dado, caso caia 3 ele faz algo, se cair 7 ele faz outra coisa, e se cair qualquer outro número ele faz outra coisa, teríamos que cobrir todo o resto de números para fazer a mesma coisa, porém podemos fazer assim:

```rust
    let dice_roll = 9;
    match dice_roll {
        3 => add_fancy_hat(),
        7 => remove_fancy_hat(),
        other => move_player(other),
    }

    fn add_fancy_hat() {}
    fn remove_fancy_hat() {}
    fn move_player(num_spaces: u8) {}
```

Assim, colocando no último braço, cobrimos todas as opções que o dado pode cair, porque o último padrão vai corresponder com todos os valores não especificados. Temos que fazer isso sempre no último  braço porque o match verifica os valores em ordem. Logo, se não for 3 nem 7, vai ser o último. 

No pattern ```other```, pegamos o valor do dado para passar na função ```move_player```, mas caso não queiramos fazer nada com esse valor e apenas chamar uma função, podemos usar o ```_```.

```rust
    let dice_roll = 9;
    match dice_roll {
        3 => add_fancy_hat(),
        7 => remove_fancy_hat(),
        _ => reroll(),
    }

    fn add_fancy_hat() {}
    fn remove_fancy_hat() {}
    fn reroll() {}
```

