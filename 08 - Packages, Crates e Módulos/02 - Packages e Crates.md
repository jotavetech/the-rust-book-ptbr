# Packages e Crates

Uma _crate_ é um pequeno amontoado de código que o compilador do Rust considera por vez, mesmo se você rodar um ```rustc``` no lugar do ```cargo``` e passar um arquivo de código sozinho, o compilador vai considerar esse arquivo uma crate. 

A _crate_ pode ter duas formas, uma binária ou uma biblioteca, as _crates binárias_ são programas que você pode compilar e rodar de forma independente. Cada uma deve ter a função ```main``` que define o que acontece quando o executável rodar.

Uma _biblioteca crate_ não tem uma função ```main```, e elas não compilam para um executável. No lugar elas definem funcionalidades que podem ser compartilhados entre múltiplos projetos.

A _crate root_ é um arquivo fonte que o compilador do Rust começa dele e cria o modulo raiz para sua _crate_.

Um package é um pacote de uma ou mais _crates_ que dão um set de funcionalidades. Um _package_ contém um arquivo _Cargo.toml_ que descreve como buildar essas crates. Ele pode conter várias crates binárias mas apenas uma _crate_ de biblioteca. E um _package_ deve conter pelo menos uma _crate_, seja binária ou uma biblioteca.


