---
title: "Bulletproof Code"
article: true
date: 2018-10-29 18:00:00
tags: code, bulletproof, quality, lslint, eslint, testing
cover: /assets/images/bullets.jpg
layout: post
---

Nesse exato momento estou voando para Barcelona, e meio que fiquei com tédio e resolvi escrever sobre como minimizar erros em uma aplicação através de boas práticas de código e infraestrutura.

## Pensamento adversarial

Essa é o melhor **linter** ou **test** que você pode criar. Seja crítico quanto à sua criação. Duvide que ela vá funcionar, pense como um usuário poderia estragar suas expectativas de sistema/requisitos ao colocar um valor string em um campo numérico. 

Ouse pensar como um cracker; quais portas fechar, como passar o token de outra sessão na sessão atual, como realizar uma força bruta no seu próprio sistema.

## Linter

![](/assets/images/vscode2.gif)

Linters são ferramentas que analisam código e identificam possíveis variáveis não usada, expressões que poderiam ser montadas de formas a facilitar a manutenção, formatação inconsistente, possíveis erros de execução como o erro de **1 milhão de dólares** `Cannot read property of undefined`, entre outras maravilhas. Recomendo ir de ESLint ou TSLint, pois são os linters mais bem mantidos pela comunidade.

## Tests 

![](/assets/images/vscode3.gif)

Inegável que uma boa bateria de testes automatizados garante uma saude para o sistema e para nós desenvolvedores. Portanto, invista um tempo implementando testes para diferentes partes do sistema; **Testes de Unidade** (aqueles que testam pequenas funções que trabalham de maneira mais isolada, como uma rotina de calculo por exemplo), **Testes de Integração** aquele que testa seu sistema como um humano/browser o faria, **Testes de Sobrecarga** para testar se seu sistema consegue escalar numa boa. Isso já vai te dar mais segurança para fazer mudanças.

## Tipagem

Durante bastante tempo eu subestimei linguagens fortemente tipadas por achar que era "coisa boba" ficar declarando tipo... **"Perda de tempo"**, pois bem, após trabalhar em um projeto grande com TypeScript, consegui vivenciar as benécias de utilizar esse tipo de linguagem de programação (mesmo que o TypeScript seja apenas uma transpilação). Primeira vantagem é você de cara especificar as suas expectativas e deixa-las bem claras no seu código. Uma forma de documentação viva.

Caso você declare uma varíavel de nome **idade** e enviar um dado que está **sinalizado** como tipo diferente, ele irá te apontar que poderá haver um problema ao comparar essas informações em tempo de execução.

Você pode construir tipos complexos, com profundidades, com membros opcionais (undefined), e sub-tipos, o que vai garantir a consistência mais ainda durante a sua experiência de desenvolvimento.

Uma coisa é certa, o JavaScript não recebe expectativa em termos de tipagem, sendo assim, fica dificil ele conseguir tentar te ajudar. Prefira linguagens fortemente tipadas, pois existirão ferramentas de análise estática que verificarão possíveis problemas em seu código.

## IDE/Text Editor Integration

![](/assets/images/vscode4.gif)

Ter um feedback rápido disso tudo que mencionei acima enquanto se desenvolve é essencial. Por isso, escolha **Editores de Texto** ou **IDE's** que forneçam suporte **Intelisense** através dos Linters e **Suíte de Testes** rodando frequentemente direto no editor.