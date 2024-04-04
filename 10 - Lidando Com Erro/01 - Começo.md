
Erros são um fato de vida em um software, então o Rust possuí inúmeras formas de lidar quando algo da errado. Na maioria dos casos, o Rust quer que você tenha conhecimento da possibilidade de um erro e faça algo antes do código compilar. Fazendo seu programa mais robusto e garantindo que você tenha coberto todos os erros antes de sair para a produção.

O Rust divide os erros em recuperáveis e não recuperáveis. Para erros recuperáveis, como um arquivo não achado, apenas queremos avisar ao usuário e tentar a operação novamente, já para erros irrecuperáveis, são sempre sintomas de bugs, como quando tentamos acessar um índice que passa do tamanho da array, então paramos imediatamente o programa. 

Usamos o tipo ```Result<T, E>``` para erros recuperáveis e o macro ```panic!``` para parar a execução em erros irrecuperáveis. 

