
Nos casos em que você não pode lidar com os erros, você usa o macro ```panic!```. Há duas maneiras de usar o panic na prática, pegando uma ação que causa um panico no nosso código, ou chamando o macro ```panic!``` explicitamente.

```rust
fn main() {
    panic!("crash and burn");
}
```

Ao rodar o programa você vai ver: 

```terminal
$ cargo run
   Compiling panic v0.1.0 (file:///projects/panic)
    Finished dev [unoptimized + debuginfo] target(s) in 0.25s
     Running `target/debug/panic`
thread 'main' panicked at 'crash and burn', src/main.rs:2:5
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
```

## Usando Rastreamento de Retorno panic!

```rust
fn main() {
    let v = vec![1, 2, 3];

    v[99];
}
```

Quando tentamos acessar um valor inexistente no raio do nosso vetor, o Rust vai dar panico. 

