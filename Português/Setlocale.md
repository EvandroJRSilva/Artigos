# O setlocale não funciona, e agora?

Enquanto estudamos programação a maioria de nós se depara com a linguagem C. Para usuários Windows as duas IDEs mais comuns são: **Dev-C++** e **Code-Blocks**. Por causa disso é bastante comum a criação de aplicações de console, ou seja, após a compilação o código será executado em uma janela do prompt de comando.

Então chega o momento do *printf*, onde fazemos com que alguma mensagem seja impressa. Se a mensagem é acentuada (o que é totalmente esperado se estiver em Português), o console vai mostrar no lugar dos acentos alguns caracteres aparentemente aleatórios. Descobrimos, então, que não há suporte 'nativo' para acentuação em C. É necessário pesquisar como fazer isso acontecer.

Na internet rapidamente encontramos a seguinte solução:

```c
#include <locale.h>

int main(){
    setlocale(LC_ALL, "Portuguese");
    //ou
    setlocale(LC_ALL, "");
    ...

    return 0;
}
```

Todos os exemplos mostram essa solução funcionando corretamente. Porém, eu nunca conseguia fazer com que essa solução funcionasse para mim também. Com certeza havia algo faltando, seja na explicação, seja no meu sistema.

Todas as [insira um número grande aqui] vezes que tentei e não consegui, após muita frustração, preferia me conformar. Até que um dia decidi encontrar a solução para isso. Tinha de haver alguma solução, mesmo que fosse apenas uma configuração no meu sistema operacional.

Após algumas horas de pesquisa, finalmente consegui entender o que de fato acontecia. A solução 'setlocale' não funcionava por causa da codificação padrão de texto do console. Para que o 'setlocale' funcione, a codificação de texto deve ser **UTF-8**. Então, uma das soluções seria modificar a codificação padrão do Windows para os consoles, o que, creio eu, não seja uma boa indicação. A possibilidade de aumentar ou até surgirem bugs é enorme. Outra solução é utilizar IDEs, ou editores de código, cujos consoles estejam padronizados com **UTF-8** (nesses casos o 'setlocale' funciona normalmente).

Mas, e se eu for teimoso e quero porque quero fazer a acentuação funcionar no executáveis (**.exe**) gerados pelo DEV-C++/Code-Blocks sem precisar mexer nas configurações padrões do Windows? Chegamos finalmente na solução que [quase ninguém] fala na internet, e que vou deixar aqui registrada.

A nova solução é, também, acrescentar duas linhas de código: A primeira, no cabeçalho, e a segunda dentro da função main.

```c
#include <windows.h>

int main(){
    SetConsoleOutputCP(CP_UTF8);
    ...

    return 0;
}
```

Pronto! Seu executável agora vai imprimir os acentos corretamente!