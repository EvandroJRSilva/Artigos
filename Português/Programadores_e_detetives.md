# Programadores e detetives

Comecei o curso de [Introdução a DataFrames.jl](https://juliaacademy.com/p/introduction-to-dataframes-jl1).

Clonei o repositório deles para seguir, executando na máquina, passo-a-passo das aulas. A primeira aula: **Configurando o ambiente**. Com meu pouco conhecimento em Julia, verifiquei quais pacotes eram utilizados. Baixei todos e parecia estar tudo ok. Fui para o tópico seguinte, onde começamos a manipular alguns dados como exemplo.

Em #Julia, tal como C++, ou Python, para carregar/utilizar uma biblioteca é preciso escrever antes **using** *nome_da_biblioteca*. Até aí, tudo bem. Então mandei executar a célula de código (o material da aula consiste em vários arquivos de Jupyter Notebook) que carregava o DataFrames. E de repente o erro:

```
ERROR: LoadError: ArgumentError: Package PrettyTables does not have LaTeXStrings in its dependencies:
- You may have a partially installed environment. try `Pkg.instantiate()` to ensure all packages in the environment are installed.
- Or, if you have prettytables checked out for development and have added LaTeXStrings as a dependency but haven't updated your primary environment's Manifest file, try `Pkg.resolve()`.
- Otherwise you may need to report an issue with prettytables stacktrace: ...
```

Não coloquei a mensagem completa de erro, mas somente a parte que *mais importante* (pelo menos para esse texto).

Lendo a descrição do erro, parece bem óbvio o que estava acontecendo. Em algum arquivo que lista as dependências, uma delas estava faltando. Para consertar, bastaria utilizar o método instantiate do gerenciador de pacotes de Julia. Simples, não?

Não.

Executei o comando e o erro persistiu. Então, possivelmente, é o arquivo Manifest que precisaria ser atualizado. Executei o comando resolve. E nada do problema ser resolvido.

Próximo passo: copiar a descrição do erro e procurar a solução no Google. Assim como temos por certo o sol nasce todas as manhãs, também temos por certo que alguém no mundo já passou pelo mesmo problema de programação que estamos enfrentando agora e o problema foi resolvido em algum fórum.

Para minha infelicidade, outras [poucas] pessoas já haviam enfrentado um problema quase igual. Porém, nada de solução. Basicamente sempre os mesmos comandos, os quais executavam normalmente, mas o problema persistia.

Em uma das páginas onde eu procurava avidamente por uma solução alguém citou o fato de a pessoa que estava com o problema estar usando o **ambiente global**. Isso me chamou atenção. Afinal, será se o tal do ambiente global não seria a raiz dos meus problemas?

Vou explicar um pouco do ambiente ao qual estou me referindo: no REPL de Julia, assim que pressionamos a tecla ']', o console automaticamente carrega o gerenciador de pacotes. E fica aparecendo algo mais ou menos assim: 

```julia
(@1.8.0) pkg>
```

Isso significa que o ambiente onde os pacotes estão sendo manipulados é o global da versão 1.8.0 de Julia. A partir daí eu posso adicionar ou remover pacotes. Para os mais acostumados com Python, é como se você estivesse utilizando o **pip**, mas sem precisar escrever 'pip'.

Pesquisando mais sobre isso, descobri que na pasta onde ficam os ambientes [default] de Julia, estão dois arquivos: Project.toml e Manifest.toml. Seria aí então nesses arquivos de dependência onde estaria a razão do erro? Talvez.

Perceba que estou descrevendo toda a minha investigação sobre o erro. A investigação conduzida por alguém com pouca familiaridade com a linguagem, e também com pouca experiência em desenvolvimento (o que é diferente de pouca experiência em programar). Alguns de vocês que estão lendo, talvez já tenham matado a xarada sobre o porquê de eu estar recebendo o erro, justamente pela experiência em desenvolvimento e saberem do que se tratam arquivos *.toml*, e toda essa questão de dependências entre bibliotecas.

Voltando à investigação, abri ambos os arquivos, em busca da tal biblioteca PrettyTables que estava me dando tanta dor de cabeça. Obviamente procurei com ctrl+f, e achei logo. Mas lá estava declarado o LaTeXStrings como dependência. Então por quê o erro dizia que não estava? Agora o ctrl+f foi em LaTeXStrings. Vai que é essa biblioteca que está com erro, e o erro propagado é detectado somente na PrettyTables, não?

Aí vi que LaTeXStrings estava como dependência para duas bibliotecas e, baseado em um do fóruns que havia lido antes, me pareceu razoável concluir que a raiz do problema estava no conflito de duas bibliotecas. Resolvido?

Não.

Neste ponto me surgiram as perguntas: como eu vou continuar o curso se não tenho como ter todas as bibliotecas utilizadas pelo instrutor? E por falar nas bibliotecas usadas por ele, como ele conseguiu resolver esse conflito? Ou, pelo menos, não ter esse conflito?

Então finalmente decidi ir ao repositório das aulas no github para abrir uma issue. Mas primeiro, ver se alguém não teve o mesmo problema antes. E sim, tinham tido problemas parecidos, mas aparentemente não idênticos. Até que, de repente, me aparece o termo **environment** de novo. Pelas respostas, até mesmo do próprio instrutor, é possível entender que o ambiente (configurado via Project e Manifest que vêm junto com os demais arquivos) tem algo de muito importante nisso tudo.

Todas as peças se encaixaram quando vi, em uma das issues, alguém comentando sobre a necessidade do comando *instantiate* para aquele ambiente. Então, com as peças todas se encaixando, minha mente parecia explodir de satisfação por estar com o problema 99% resolvido.

Fui atrás de ver melhor sobre como é essa questão de ambiente em Julia. O mais incrível é que está na documentação, como parte básica quando se fala sobre pacotes. Mas eu não havia me atentado nesse detalhe de ambientes até então, mesmo estando tudo lá. Me pergunto, e preciso verificar, se realmente falam com um pouco mais de detalhes sobre o ambiente, ou se apenas citam o básico do básico de como instalar e remover pacotes, desconsiderando o ambiente. Confesso que eu só havia absorvido as instruções sobre manipular pacotes.

Agora que tudo estava fazendo sentido, e finalmente entendendo do que realmente se tratava o ambiente global (onde eu estava), fiz a "configuração" correta. Neste caso específico, era apenas ativar o ambiente que veio junto com as aulas. Mais detalhadamente, explicitar o caminho do arquivo Project.toml das aulas, o qual, juntamente com o Manifest.toml, são as configurações do ambiente necessário para as aulas.

Carregado o ambiente correto, o '**using** *nome_da_biblioteca*', que neste caso era '**using** DataFrames', finalmente executou sem problema algum!

Para concluir, devo ressaltar que decidi escrever todo esse relato por três razões: (1) servir como solução para os próximos que enfrentarem o mesmo problema; (2) reforçar que faz parte do trabalho em programação momentos dedicados à investigação e resolução de problemas no código (em vez de produção 24/7) e (3) reforçar que devemos sempre atentar a todos os detalhes e informações disponíveis em uma documentação.