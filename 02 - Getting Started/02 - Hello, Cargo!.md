# Hello, Cargo!

O Cargo é um sistema de build e gerenciador de pacotes do Rust, é muito usado porque o Cargo da conta de um monte de tarefas para você, como fazer a build do código, instalar bibliotecas e dependências do código, além de construir essas bibliotecas.

Se você instalou rust seguindo a documentação do Rust você poderá digitar o seguinte comando pra ver a versão do Cargo que você está usando, se não for encontrado, procure como instalar o Cargo separadamente.

```console
$ cargo --version
```


### Criando um projeto com Cargo

Vamos iniciar um novo projeto usando o Cargo, você vai ver a diferença dele com o nosso programa original "Hello, World!".

Primeiro vamos criar um novo projeto e acessar a sua pasta.

```console
$ cargo new hello_cargo 
$ cd hello_cargo
```

O primeiro comando cria um novo diretório e um projeto chamado hello_cargo, agora podemos ir na nossa pasta que foi criada e ver a lista dos arquivos, o Cargo criou dois arquivos para nós: _Cargo.toml_ e um diretório _src_ com um arquivo _main.rs_ dentro dele.

Se você não estiver dentro de um repositório git o cargo irá gerar um arquivo .gitignore, caso o contrario você vai ter que usar a flag ```--vcs=git``` para  criar. 

Abrindo o arquivo _Cargo.toml_ você vai ver isso:

```toml
[package] 
name = "hello_cargo" 
version = "0.1.0" 
edition = "2021" 
# See more keys and their definitions at https://doc.rust-lang.org/cargo/reference/manifest.html 

[dependencies]
```

Esse é um arquivo de configuração de uma aplicação Rust usando o Cargo.

A primeira linha ```[package]``` é a seção de heading que indica as declarações que estão configurando o pacote, as próximas três linhas são as informação que o Cargo precisa para compilar o nosso programa, o nome, a versão e a edição do Rust que vai usar.

A ultima linha ```[dependencies]``` é o começo da seção para você listar qualquer dependência do seu projeto, em Rust, os pacotes de código são chamados de _crates_.

Agora podemos abrir _src/main.rs_ e dar uma olhada:

```rust
fn main() {
    println!("Hello, world!");
}
```

O Cargo gerou um programa "Hello, world!", igual fizemos anteriormente, até aqui as diferenças do nosso primeiro projeto e o projeto Cargo é que o Cargo colocou o código dentro do diretório _src_ e agora nós temos um arquivo de configuração _Cargo.toml_.

Cargo espera que todo seu código fonte esteja dentro do diretório _src_, o diretório onde temos o _Cargo.toml_ é apenas para arquivos README, informações de licença, e nada relacionado ao código. 

### Compilar e Rodar um Projeto Cargo

Para fazer a build do nosso projeto podemos digitar o comando:

```console
$ cargo build
```

Esse comando irá gerar um arquivo executável em _/target/debug/hello_cargo_. O arquivo padrão de build é uma build de debug, o Cargo coloca os binários em um diretório chamado debug, agora podemos rodar o executável rodando o seguinte comando:

```console
$ ./target/debug/hello_cargo # ou hello_cargo.exe no Windows
Hello, world!
```

Se tudo ocorrer corretamente você vai ter um ```Hello, world!``` impresso no seu terminal, rodar o comando ```cargo build``` pela primeira vez também vai gerar um arquivo Cargo.lock, esse arquivo acompanha as exatas versões das dependências do seu projeto. Você nunca vai precisar mudar isso manualmente, o Cargo cuidará disso.

Nós também podemos usar ```cargo run```para compilar o código e rodar o executável final em apenas um comando:

```console
$ cargo run
Hello, world!
```

Usar ```cargo run``` é mais conveniente que ter que rodar ```cargo build``` e depois usar todo o caminho para o binário, então a maioria dos desenvolvedores usam ```cargo run```. Sempre que o nosso código mudar você vai poder ver uma mensagem na tela que diz que o ```cargo run```está compilando o código fonte.

Para apenas checar se seu código compila mas não gerar nenhum executável você pode usar ```cargo check```. Por que você usaria isso? Ele é muito mais rápido que ```cargo build```porque ele pula a parte de produzir um executável. Você pode usar o _check_ para verificar se está tudo certo durante o desenvolvimento e quando acabar você pode fazer a _build_. 



