# Hello, World!

Criando uma pasta para o nosso primeiro programa em rust:

``` console
$ mkdir ~/projects
$ cd ~/projects
$ mkdir hello_world
$ cd hello_world 
```

Agora vamos criar um novo arquivo e chamar ele de **main.rs**, arquivos rust sempre terminam com ".rs". Se você for criar um arquivo com mais de uma palavra, por convenção, prefira usar underline para separar elas, como _hello_world.rs_ ao invés de _helloworld.rs_.

Agora no arquivo, vamos digitar nosso primeiro código:

```rust
fn main() {
    println!("Hello, world!");
}
```

Agora podemos voltar ao terminal e para compilar e rodar o arquivo podemos digitar os seguintes comandos:

#### Linux ou Mac Os:
```console
$ rustc main.rs  #compila o arquivo
$ ./main         #roda o arquivo executável criado pelo compilador
Hello, world!    #output
```

#### Windows
```console
> rustc main.rs 
> .\main.exe 

Hello, world!
```

Agora você é um programador Rust!

### Anatomia de um Programa em Rust

A primeira parte:

```rust
fn main() {
 // ...
}
```

Aqui criamos uma função nomeada ```main```, ela é uma função especial, é sempre a primeira parte do código que vai rodar em todo programa executável em Rust. Nela não vai nenhum parâmetro e também não retorna nada. Se precisássemos de parâmetros eles iriam dentro dos parênteses ( ).  

O corpo de função esta envolto dos { } (Curly brackets). O rust precisa de curly brackets em volta de todos os corpos das funções.

O corpo da função `main` agora segura o seguinte código:

```rust
    println!("Hello, world!");
```

Essa é a linha que faz todo o trabalho nesse pequeno programa, ela imprime o texto na tela.
Temos alguns detalhes importantes para ver aqui:

- O estilo de código do Rust é identado por 4 espaços, não uma tab.
- ```println!``` chama um macro do Rust, se fosse chamado como uma função seria ```println```sem usar o ```!```. Vamos ver macros mais detalhadamente mais pra frente, por enquanto apenas precisamos saber que usar um ```!```significa que estou chamando um macro ao invés de uma função normal e que os macros não seguem sempre as mesmas regras das funções.
- A string ```Hello, world!``` é passada como um argumento para o macro ```println!``` e ela imprime a string na tela.
- Precisamos terminar a linha com ```;``` para indicar que a expressão acabou e estamos prontos para seguir com o código. 

### Compilar e Rodar o Programa São Passos Separados.

Acabamos de criar um programa, vamos examinar cada passo desse processo.

Antes de rodar o programa Rust você deve compilar ele usando o compilador do Rust, usando o comando ```rustc``` e passando o nome do arquivo do nosso código, assim:

```console
$ rustc main.rs
```

Se o código for compilado com sucesso, sem erros, será gerado um executável binário. Que você pode rodar digitando no terminal:

```console
$ ./main  # ou .\main.exe no Windows.
```

Rust é uma linguagem compilada a frente do tempo! Quer dizer que você consegue compilar o programa e dar o executável para qualquer pessoa e elas vão conseguir rodar o código mesmo sem ter o Rust instalado. Se você dar a alguém um .rb, .py ou um arquivo .js, elas vão precisar ter uma implementação de Ruby, Python ou Javascript instalada.  Mas nessas linguagens você só precisa de um comando para compilar e rodar o programa, tudo é uma troca do design da linguagem.