# Como Escrever Testes

## Por que Escrever Testes?

O Rust já faz um ótimo trabalho cuidando dos nossos tipos, e outras coisas como o verificador de empréstimos (borrow checker), verificando se não estamos gerenciado mal a nossa memória, mas, não conseguimos realmente checar se as funções estão fazendo a coisa certa, a lógica de negócio, isso cuidamos nos nossos testes.

## Criando Testes

Primeiro, vamos usar o ```cargo new adder --lib``` para criar uma nova biblioteca para nossos testes, após criar, dentro do arquivo _src/lib.rs_, você vai encontrar um módulo de testes já escrito:

```rust
pub fn add(left: usize, right: usize) -> usize {
    left + right
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn it_works() {
        let result = add(2, 2);
        assert_eq!(result, 4);
    }
}
```

Em Rust, as funções de teste tem o atributo ```#[test]```.

Para rodar os testes, usamos ```cargo test```.

Vamos ter algo assim no terminal, indicando quais testes passaram e quais falharam:

```bash
test result: ok. 1 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out; finished in 0.00s
```

### Teste Que Falha

Dentro do nosso módulo de testes, vamos escrever uma nova função, que lançará um ```panic!```, para falhar o teste:

```rust
#[test]
fn failing_test() {
	panic!("Faça esse teste falhar")
}
```

Agora, rodando o teste, vamos ver que temos uma falha:

```bash
test result: FAILED. 1 passed; 1 failed; 0 ignored; 0 measured; 0 filtered out; finished in 0.00s
```


## Testando Código de Produto

Vamos criar a estrutura de um retângulo e um método que verifica se um retângulo é maior que o outro, depois nos testes vamos retornar se foi verdadeiro com falso com o macro de acerto ```assert!``` que retorna true/false.

```rust
#[derive(Debug)]
struct Rectangle {
    width: u32,
    height: u32,
}

impl Rectangle {
    fn can_hold(&self, other: &Rectangle) -> bool {
        self.width > other.width && self.height > other.height
    }
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn larger_can_hold_smaller() {
        let larger = Rectangle {
            width: 8,
            height: 7,
        };

        let smaller = Rectangle {
            width: 5,
            height: 1,
        };

        assert!(larger.can_hold(&smaller));
    }
}
```

Esse teste vai passar com sucesso! Pois ```larger``` consegue conter ```smaller```.

### Macro assert!

O macro ```assert!``` também nos permite comparar dois valores:

```rust
pub fn add_two(a: i32) -> i32 {
    a + 2
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn it_adds_two() {
        assert!(4, add_two(2));
    }
}
```

O macro ```assert!``` compara se o resultado do segundo parâmetro é igual o primeiro. 

Também tem o macro ```assert_eq!``` que retornará ```true``` caso os dois parâmetros passados não sejam iguais, ex: ```assert_eq!(4, 4)``` será verdadeiro.


## Testes Para Garantir Falhas

Quando queremos que uma função falhe para passar no teste, usamos o atributo ```#[should_panic]```, a função deveria falhar, mas, vai passar nos testes, pois é isso que estávamos esperando: 

```rust
#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    #[should_panic]
    fn it_adds_two() {
        assert_eq!(4, 2);
    }
}
```

## Retornando Tipo de Resultado

Também podemos retornar tipos de resultados, como ```Ok``` e ```Err```: 

```rust
  #[test]
    fn it_works() -> Result<(), String> {
        if 2 + 3 == 4 {
            Ok(())
        } else {
            Err(String::from("two plus two does not equal four"))
        }
    }
    ```

Dentro de ```Err``` passamos a mensagem que será retornada nesse tipo.


