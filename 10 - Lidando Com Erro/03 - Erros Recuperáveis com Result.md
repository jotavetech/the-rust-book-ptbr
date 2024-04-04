
# Erros Recuperáveis com Result

A maioria dos erros não são sérios o suficiente para fazer o programa paras, as vezes, quando uma função falha, é por uma razão que você pode interpretar e responder a ela. Por exemplo, se você tentar abrir um arquivo e ele não existir, ao invés de parar o programa, podemos criar o arquivo. 

Lembre do enum ```Result```:

```rust
enum Result<T, E> {
    Ok(T),
    Err(E),
}
```

Onde os tipos ```T```, e ```E``` são tipos genéricos, no qual, ```T``` corresponde ao tipo do valor que será retornado em caso de sucesso, e ```E``` o tipo de erro em uma falha. 

Vamos chamar uma função que retorna um valor ```Result```, tentaremos abrir um arquivo:

```rust 
use std::fs::File;

fn main() {
    let greeting_file_result = File::open("hello.txt");
}
```

Nesse caso, se ```File::open``` der sucesso, o valor retornado será uma instância de Ok, que contém um manipulador de arquivos, se falhar, será uma instância de Err, que contém mais informações sobre o tipo de erro que aconteceu. 

Podemos lidar com os dois tipos de resultado usando ```match```:

```rust
use std::fs::File;

fn main() {
    let greeting_file_result = File::open("hello.txt");

    let greeting_file = match greeting_file_result {
        Ok(file) => file,
        Err(error) => panic!("Problem opening the file: {:?}", error),
    };
}
```

Também podemos usar o enum ```io::ErrorKind``` para verificar o tipo de erro e tomar ações baseadas nele, por exemplo, se o motivo do erro for um arquivo não encontrado, então criamos um novo:

```rust
use std::fs::File;
use std::io::ErrorKind;

fn main() {
    let greeting_file_result = File::open("hello.txt");

    let greeting_file = match greeting_file_result {
        Ok(file) => file,
        Err(error) => match error.kind() {
            ErrorKind::NotFound => match File::create("hello.txt") {
                Ok(fc) => fc,
                Err(e) => panic!("Problem creating the file: {:?}", e),
            },
            other_error => {
                panic!("Problem opening the file: {:?}", other_error);
            }
        },
    };
}
```


### Atalhos para o Erro Panic! 

Podemos usar o método ```unwrap``` como um atalho que retornará o arquivo, em caso de sucesso, e irá chamar ```panic!``` em caso de erro. 

```rust
use std::fs::File;

fn main() {
    let greeting_file = File::open("hello.txt").unwrap();
}
```

Também temos o método ```expect``` que nos permite passar uma mensagem personalizada ao ```panic!```:

```rust
use std::fs::File;

fn main() {
    let greeting_file = File::open("hello.txt")
        .expect("hello.txt should be included in this project");
}
```





