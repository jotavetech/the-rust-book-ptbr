
# Definindo Módulos para Controlar Escopo e Privacidade

- Começa da raiz da crate: Quando compilamos uma crate, o compilador primeiramente olha a seu arquivo raiz (geralmente _src/lib.rs_) e procura um arquivo para compilar;
- Declarando módulos: No arquivo raiz de uma crate você pode declarar seus novos módulos, ex, você declara um módulo "jardim" com ```mod: jardim;```. O compilador vai procurar pelo código do módulo nesses lugares: Em linha, também no arquivo _src/jardim.rs, e também no arquivo _src/jardim/mod.rs_
- Declarando submódulos: em qualquer outro arquivo que contém uma raiz de uma crate, você pode declarar submódulos, por exemplo, você quer declarar ```mod:vegetais``` em _src/jardim.rs_. O compilador vai procurar pelo modulo pai nesses lugares: Em linha, também no arquivo _src/jardim/vegetais_, e também em _src/jardim/vegetais/mod.rs_.
- Caminhos para código em módulos: Uma vez que o módulo fizer parte da sua crate você se referir ao código em qualquer lugar da mesma crate, seguindo as regras de privacidade. Por exemplo, para usar um tipo ```Aspargus``` nos vegetais do jardim você pode encontrar ele em ```crate::jardim::vegetais::Aspargus```.
- Privado vs Publico: Código dentro de um módulo é privado por padrão, você fazer ele público usando ```pub``` antes de declarar, ex: ```pub mod```.
- Palavra-chave ```use```: Dentro de um escopo, ela cria um atalho para reduzir a repetição de longos caminhos, você pode se referir a um tipo dentro de um módulo com a palavra ```use```, assim consegue se referir a aquele item sem repetir o caminho inteiro dele. 

Código de exemplo:

```rust

// src/garden/vegetais.rs
use crate::garden::vegetables::Asparagus;

pub mod garden;

fn main() {
    let plant = Asparagus {};
    println!("I'm growing {:?}!", plant);
}
```







