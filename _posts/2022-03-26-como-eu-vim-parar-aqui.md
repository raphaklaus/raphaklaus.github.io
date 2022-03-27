---
title: 'Como eu Vim parar aqui'
article: true
date: 2021-05-30 19:40:00
tags: Vim productividade editor de código
image: /assets/images/como-eu-vim-parar-aqui.jpg
layout: post
---

Nesse artigo vou compartilhar um pouco sobre minha jornada utilizando a ferramenta Vim para edição de código.

<!--more-->

## Mundo comum

Meu primeiro contato com Vim foi em 2009 quando comecei a mexer com Linux. Naquela época, eu entrava no [Viva o Linux](https://www.vivaolinux.com.br) pra
poder copiar uns scripts e configurar minha distro do jeito que precisava. O que notei é que a maioria dos tutoriais orientavam a
edição de arquivos usando o Vim ou o Nano.

Pra quem não sabe, o Vim é um editor de texto baseado em terminal que tem como principal requisito a não utilização do mouse, navegação através de "atalhos" pelo teclado. Isso vai ficar mais claro ao longo desse artigo.

Lembro como se fosse ontem, eu abrindo o Vim pela primeira vez e não conseguindo editar nada, e "aleatoriamente" sobrescrevendo alguns caracteres.

Aquela frustração inicial me levou à utilização do Nano ou Gedit (em modo gráfico), editores que apenas fazia o que eu precisava.

Fiquei alguns anos sem precisar usar o Vim de novo pois tinha alternativas que me bastavam. Foi até chegar 2014 quando comecei a trabalhar com
as [VPSs](https://pt.wikipedia.org/wiki/Servidor_virtual_privado) da Digital Ocean que em seus tutoriais de configuração só usavam o Vim como
editor. Lá fui eu de novo abrir o Vim e não conseguir sair dele! 

Dessa vez eu fiquei tão frustrado que falei pra mim mesmo que entenderia que raios era aquele tal de Vim e porque eu estava preso nele, sem conseguir salvar e nem editar nada!

Algumas buscas na internet me explicaram o que eu precisava, o Vim era composto de dois modos básicos: Modo normal e modo de inserção. Quando se está no modo normal, não é possível editar o texto, apenas navegar por ele. Para editar o código, eu deveria apertar a tecla "i" entrando no modo de inserção.

Ao finalizar minha edição eu deveria voltar pro modo normal, apertando ESC, depois apertar `:wq` que significa "salvar e sair".

Ok! Pelo menos naquele momento eu era capaz de fazer as alterações nos *scripts* que eu precisava sem muita dor de cabeça.

Embora esse lance de dois modos não fizesse o menor sentido pra mim, eu conseguia **"get work done"** e assim seguia.

## Recusa do chamado 

Em 2016, eu me mudei pro Rio de Janeiro pra trabalhar numa startup e haviam dois caras lá que usavam esses editores esotéricos.

Um usava o Vim e o outro Emacs e ambos pareciam bem amantes dos seus editores. Para ser sincero, chegava a ser engraçado porque eles 
eram do tipo "evangelizadores", então volta e meia comentavam como era bom usar esses editores enquanto o resto da galera usava
o Atom ou VSCode.

Nessa época eu fui aprendendo alguns comandos por osmose. Coisas básicas como por exemplo, fazer buscas, ir pro início do arquivo, final, ir para linha, e a famigerada navegação hjkl.

Honestamente, por mais que o pessoal falasse bem do Vim, não entrava na minha cabeça que aquilo era intuitivo ou rápido. Afinal de contas,
eu conseguia ser rápido com as navegações padrões *arrow keys + home/end/page up/page down* combinado com *alt*, *shift* e *control*.

Eu tentei algumas vezes aprender o Vim, mas simplesmente aquilo não fazia sentido pra mim! Pra que apertar "gg" se eu podia apertar apenas "home"?
É o mesmo efeito, só que "home" pra mim era ainda mais semantico que "gg". Sem contar que eu não precisava ter a carga cognitiva de ter que lidar com dois modos. Mais simples!

## Encontro com o mentor

Pulamos pra 2022, já em Barcelona, eu comecei a me questionar porque certas pessoas escolhiam o Vim. E pior, como essas pessoas conseguiam ser TÃO rapidas não só editando código, mas também durante seu fluxo de trabalho.

Assisti uns vídeos na internet sobre Vim, incluindo um excelente pelo [Fábio Akita](https://twitter.com/AkitaOnRails) e outros pelo [ThePrimeagen](https://twitter.com/ThePrimeagen) e fiquei animado pra tentar mais uma vez.

Então comecei primeiro pelo [Vim Adventures](https://vim-adventures.com/) que através de um jogo te ensina os comandos básicos. Também tem o `vimtutor` através da linha de comando que sinceramente não tentei, mas falam muito bem dele.

Após isso, decidi que iria tentar o Vim mode no VSCode primeiro pra evitar cair no *configuration hell* e poder focar no que acreditava ser o core do VIM que é navegação. Essa decisão foi crucial pra minha experiência no VIM ser menos frustrante, pois eu pude focar 100% no que eu precisava aprender ainda tendo um editor capaz de me ajudar em todas as outras tarefas.

Os dois primeiros dias foram 100% improdutivos. Pra fazer coisas básicas eu tinha que olhar na documentação, era extremamente frustrante!
Já no terceiro e quarto dia, foi um processo de consolidação do que eu havia aprendido e as coisas foram começando a melhorar.

No final da semana eu já tava relativamente confiante e resolvi colocar outra meta pra próxima semana. Deixar o VSCode pra trás e seguir
*full* Vim.

## Estrada de provas

Instalei o NeoVim que é uma versão mais extensível do VIM original. O problema era que meu fluxo de trabalho consistia de alguns detalhes além
de apenas editar código, como:

* Encontrar arquivos;
* Navegar pelo diretório, criar pastas, renomear arquivos, etc...;
* Ter um terminal acessível a qualquer instante;
* Busca global de strings e expressões regulares;
* Integração com Git para visualização de diff e resolução de conflitos;
* Autocomplete, snippets e intelisense.

Isso era o mínimo que eu precisava para estar no mesmo nível de produtividade que o VSCode entregava.

Depois de ler muito sobre o Vim, e tentar diferentes configurações e plugins, o que foi algo que se estendeu pela minha segunda semana inteira usando VIM, descobri algo bem interessante: há uma comunidade muito ativa criando plugins para as mais diversas tarefas.
Depois de uma grande peneiragem, consegui encontrar alguns plugin/configurações que fizesse cada ponto da lista anterior:

* Encontrar arquivos: [fzf.vim](https://github.com/junegunn/fzf.vim) (famoso ctrl/cmd + P do VSCode)
* Navegar pelo diretório, criar pastas, renomear arquivos, etc: [Nerdtree](https://github.com/preservim/nerdtree)
* Ter um terminal acessível a qualquer instante: Não é um plugin de Vim mas resolvi isso usando Tmux, explicarei no final.
* Busca global de strings e expressões regulares: fzf + ripgrep através do comando `:Ag` 
* Integração com Git para visualização de diff e resolução de conflitos: [vim-fugitive](https://github.com/tpope/vim-fugitive) e [vim-gitgutter](https://github.com/airblade/vim-gitgutter)
* Autocomplete, snippets e intelisense: [coc.nvim](https://github.com/neoclide/coc.nvim)

Ok, então eu tinha as minhas necessidades básicas atendidas, a partir desse ponto, minha real busca por aprimoramento começava.

No final da minha segunda semana, notei uma coisa muito estranha: minha mão estava doendo **muito**.

Aquilo foi um *red flag* total pra mim. E essa sensação não parecia certa, pois uma das vantagens do Vim é a ergonomia que ele trás consigo.

Comecei a ler posts no Reddit, StackOverflow, vídeos no YouTube e descobri algumas coisas péssimas que eu tava fazendo

### Navegação ineficiente

Como disse anteriormente, o Vim advoga por uma edição de texto sem mouse, então 99% do tempo você vai estar usando o teclado.

O grande problema é que eu estava habituado a "scrolar" com a rodinha do mouse ou clicando no minimap do VSCode. No Vim essa não é a melhor
forma de se navegar, portanto, eu usava as teclas hjkl para me movimentar. Quando precisava me movimentar horizontalmente até alguma termo
no meio da linha, ia pulando de palavra em palavra através da tecla `w` ou `b`. Fazia algo semelhando quando precisava me deslocar verticalmente,
apertando `j` ou `k`.

Esse tipo de movimentação gera muita tensão nos tendões tanto por pressão quanto por esforço repetitivo. Descobri duas coisas fascinantes
para esses tipos de deslocamentos.

Vamos imaginar o código:

```js
  var data = JSON.parse(result[0].response);
```

Vamos supor que queremos mudar o `result[0]` para `result[index]`. Ao invés de apertar o `w` 9 vezes, podemos simplesmente `ci[` (change inside), 
isso vai achar a primeira abertura de colchetes, remover o que há dentro dele e entrar em modo de inserção. O melhor
de tudo é que seu cursor pode estar em qualquer lugar nessa linha antes da abertura do colchete. Só com esse truque eu reduzi em 66% dos 
movimentos necessários para fazer essa modificação.

Vamos para outra situação, queremos remover o `.response`. Simples, `f.;vt)x`

Se você não está familiarizado com Vim é nesse momento que você ri alto! Mas calma, tudo isso é muito semântico e basta você fazer
a associação dos termos. Vamos desconstruir esse hieroglifo!

Vamos separar por duas partes, a de posicionar o cursor e a de selecionar e remover.
Dado que a gente está em qualquer lugar antes do `.parse` a gente quer que o cursor chegue até o ponto do `response`.

```
Primeira parte: f.; 
f = find  (para encontrar algo na linha)
. = o que queremos encontrar
; = ir para a próxima ocorrência pois o primeiro ponto é do `.parse` e não é isso que queremos
```

Ótimo, agora nosso cursor está exatamente onde queremos. Nesse momento, precisamos selecionar tudo até antes do fechamento de parenteses
e deletar isso

```
Segunda parte vt)x
v = entra em modo de seleção
t = semelhante ao f, mas ele para o cursor antes do elemento
) = qual caracter o cursor deve buscar
x = deleta o que está selecionado
```

Esse gif mostra essas duas etapas:

![](/assets/images/vim-1.gif)

Existem maneiras até mais eficientes de fazer isso mas para efeitos de demonstração está perfeito. Não faça como eu no início
que fiquei apertando `wwwwwwwwll` pra chegar onde queria.  

Outra dica valiosa que recebi foi setar o número das linhas pra serem relativas. Isso diminui no mínimo pela metade a quantidade de teclas
que você precisa digitar ao ter que fazer pulos curtos no arquivo.

Veja esse exemplo: 

![](/assets/images/vim-2.gif)

A linha do cursor é a de número 4, e todas as linhas são números relativos a linha atual partindo de 1 tanto pra cima quanto pra baixo.

Se eu apertar `5j` eu chego exatamente na função abaixo e posso voltar usando o mesmo raciocínio, porém apertando `k`.

Mais uma coisa que comecei a aplicar foi o uso do CTRL+I/O. Isso me permite ir pra frente e pra trás no histórico de "pulos" dentro do arquivo
e através de arquivos. Assim eu posso rapidamente ir pra um arquivo que estava editando anteriormente, fazer a alteração e pular de volta aonde
eu estava.

O que mais me anima no Vim é que cada movimento novo que você aprende é "combável", ou seja, você pode aplicar ele com outros que você já sabe pra
criar movimentos ainda mais precisos.

### Tensão na musculatura

Alguns comandos que eu estava usando com **muita** frequência era CTRL+B (prefixo do Tmux) para navegação entre panes, gerenciamento de janelas, etc CTRL+W pra navegar entre splits do Nerdtree.

Pode não parecer mas fazer isso com certa frequência causa um cansaço muito grande na musculatura da mão. Mais algumas leituras na internet e 
descobri uma galera que faz algo bem controverso no teclado para diminuir essa fadiga: troca o capslock pelo control.

Ao posicionar as mãos no chamado "home row" do teclado, ou seja, a mão esquerda em cima das teclas asdf e a mão direita em cima das hjkl você
tem a tecla control bem do lado do seu mindinho esquerdo, criando menos tensão ao teclar os atalhos.

Outra coisa que adotei e também vejo o pessoal recomendar é mapear as teclas `jj` para ESC, assim você não precisa se deslocar pra sair do
modo de inserção. 

Como dois j's são muito incomuns na maioria das linguas, é uma solução muito viável.

### Tenha um teclado confortável

Esse ponto pode parecer óbvio mas ter um teclado que seja confortável é fundamental. Se você tiver tempo, eu te recomendo ir a lojas de informática especializada para testar os teclados. O gosto varia de pessoa pra pessoa, então vou compartilhar aqui o que acho que funciona bem pra mim em um teclado:

* Fomato full ou TKL (teclado completo incluindo o teclado numérico e sem o teclado numérico, por isso ,*"Ten KeyLess"*, respectivamente);
* Teclado mecânico de teclas lineares mais suave possível. Quero dizer, mínimo esforço para acionar as teclas;
* Ter suporte inferior para controlar altura;
* Layout ANSI (mas pode ser ISO também a depender da sua preferência).

## Apoteose

Nessa etapa, eu consegui superar os principais problemas em termos de navegação, configuração e ergonomia. Daqui em frente eu sinto como
se eu tivesse uma interface entre meu cérebro e o código, na qual eu consigo traduzir mais rapidamente meus movimentos.

Quando você entende os benefícios dessa ferramenta, você passa a querer ampliá-la para outros fluxos de trabalho. Foi o meu caso usando
o [Tmux](https://github.com/tmux/tmux), que permite gerenciar diversos terminais usando atalhos bem intuitivos. Geralmente um programador precisa ter aberto não só um editor de
código, mas também um terminal executando a aplicação, outro com o acesso ao banco de dados, rodar *admin tasks* e por aí vai.

Além do mais, conforme você vai entendendo a filosofia do Vim, você mesmo passa a ter sacadas que na verdade alguém já pensou, pois é do tipo de 
movimento que faz sentido existir. Como por exemplo a habilidade de rodar testes que estão no cursor apenas digitando `\t`, ou todos os testes do arquivo `\T`. Isso trás muita rapidez em fazer verificações em código testado. Além do mais, é comum você sair de um código de teste e mergulhar em um código de implementação pra poder fazer o teste passar. Em dado momento você precisa voltar exatamente pro último teste que rodou. É, já pensaram 
nesse caso de uso! Basta digitar `\g`. Isso tudo é possível ao instalar o plugin [vim.test](https://github.com/vim-test/vim-test). E detalhe, funciona pra qualquer linguagem de programação.

Pra ser sincero, acho que tenho um monte de coisas pra aprender nesse universo do Vim, mas só de revolucionar a maneira com que eu vinha fazendo código 
nos últimos 15 anos, já é uma grande elevação. Somando-se a isso, a performance do Vim é incomparável com qualquer editor gráfico. Ele abre em milissegundos já pronto para trabalhar, com autocomplete, snippets e o que mais quiser.

Vou deixar aqui o link pro meu `.vimrc` que estarei atualizando constantemente com as configurações que acredito funcionarem pra mim. Fique à vontade
para dar fork e fazer a sua própria versão! [Clique aqui](https://github.com/raphaklaus/dotfiles)

Pretendo escrever em 1 ano uma parte 2 contando o resto da jornada! Até lá, estou muito empolgado com o que poderei seguir aprendendo :D

Abaixo você encontrará uma foto da Torrada, a gatinha que adotamos que chegou ontem aqui em casa!

![](/assets/images/Torrada-Recém-Chegada.png)

Tadinha! Ela ainda tem muito medo... Passou por coisas terríveis. Mas tenho certeza que ela vai superar isso tudo e vai se tornar uma gatinha feliz!

Aqui alguns vídeos e artigos que consumi e que me ajudaram imensamente:

[https://www.youtube.com/watch?v=Iid1Ms14Om4](https://www.youtube.com/watch?v=Iid1Ms14Om4) (sobre Vim, em Inglês)
[https://www.youtube.com/watch?v=E7NBhSsZouc](https://www.youtube.com/watch?v=E7NBhSsZouc) (sobre Vim, em Inglês)
[https://www.youtube.com/watch?v=-I1b8BINyEw](https://www.youtube.com/watch?v=-I1b8BINyEw) (sobre Vim, em Inglês)
[https://www.youtube.com/watch?v=lm7y2hI6zME](https://www.youtube.com/watch?v=lm7y2hI6zME) (sobre Vim, BR)
[https://www.youtube.com/watch?v=78aw1muYWQM](https://www.youtube.com/watch?v=78aw1muYWQM) (sobre teclados, BR)
[https://www.youtube.com/watch?v=epiyExCyb2s](https://www.youtube.com/watch?v=epiyExCyb2s) (sobre Vim, Tmux e outras coisas interessantes, BR)

