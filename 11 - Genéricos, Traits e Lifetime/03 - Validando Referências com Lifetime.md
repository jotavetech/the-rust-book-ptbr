# NAO FOI TERMINADO, PRECISA MELHORAR MUITO.

Lifetimes (ciclos de vida) são outro tipo de genérico que já usamos. Diferente que garantir que um tipo tenha o comportamento que queremos, lifetimes garantem que as referências serão válidas contanto que precisamos que elas sejam.

## Prevenindo Referências Pendentes com Lifetimes

Referências pendentes são referências que apontam para um dado inválido, e o Rust não gosta disso.

Exemplo: 

```rust
fn main() {
    let r;

    {
        let x = 5;
        r = &x;
    }

    println!("r: {}", r);
}
```

Declaramos a variável ```r``` e dentro de um novo escopo criamos a variável ```x```, tentamos colocar uma referência para ```x``` em ```r``` dentro do escopo, mas esse código irá quebrar, porque ao sair do escopo o ```x``` não existe mais e ```r``` estaria apontando para uma referência inválida (inexistente) agora.

## Lifetime Genérico em Funções

Anotação genérica de lifetime descreve a relação entre o ciclo de vida de múltiplas referências e como elas estão relacionadas entre elas 

Vamos usar nesse código que cria duas String e depois chama uma função que recebe a referência dessas string e retorna a mais longa:

```rust
fn main() {
    let string1 = String::from("abcd");
    let string2 = String::from("xyz");

    let result = longest(string1.as_str(), string2.as_str());

    println!("A string mais longa é {}", result)
}

// &i32 -> referencia
// &'a i32 -> referencia com ciclo de vida explicito
// &'a mut i32 -> referencia mutavel com ciclo de vida explicito

fn longest<'a>(x: &'a str, y: &'a str) -> &'a str {
    if x.len() > y.len() {
        x
    } else {
        y
    }
}
```

Anotações de lifetime usam ```'``` no começo seguido do nome, pode ser qualquer coisa. Assim estamos dizendo que ```x```, ```y``` e o valor de retorno da função tem seu ciclo de vida relacionado. 