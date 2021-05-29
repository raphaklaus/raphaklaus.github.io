---
title: 'Qualidade de Software'
article: true
date: 2021-05-18 18:00:00
tags: code quality
image: /assets/images/qualidade-de-software.jpeg
layout: post
---

Gostaria de compartilhar aqui o que aprendi nos últimos anos sobre os desafios de construir uma codebase que escala.

<!--more-->

Um software começa geralmente com as especificações básicas e um codebase vazio, ou mesmo iniciado com algum boilerplate.
Tudo parece perfeito... Começamos a adicionar funcionalidades, vamos introduzindo mais camadas de abstração, escrevemos testes e as coisas parecem estar indo lindamente.

Então o primeiro incidente em produção acontece. Ué!? Mas não era para isso ter sido previsto pelas nossas suítes robustas de testes? Você sobe um hotfix com a correção + cenário de teste que garante que a situação não ocorrerá novamente e vai dormir tranquilo...

A sua equipe continua criando novas funcionalidade como previsto, e então, mais um incidente... Você faz o hotfix, escreve os teste e vai dormir tranquilo...

Parece que há um padrão aí.

# Mitigue os riscos de um deploy

Deploys não deveriam ser tarefas complexas que exigem dias reservados na agenda da equipe. Muito menos é para ser temido.

Vamos supor que você tem uma aplicação que, em média, há 3.000 usuários online à todo instante. Alguns devs sentiriam um certo "cagaço" na hora de deployar.

Mas não tem motivo! É insanidade fazer um deploy de uma determinada modificação, para de cara isso ficar disponível para todos os usuários, isso tem um grande potencial catastrófico!

## Blue-green / Canary / Feature Flags para a salvação

O que esses três métodos de deploy tem em comum é muito simples: minimizar o possível impacto de um deploy. Essas estratégias buscam criar "branches" à nível de infraestrutura/código. Vamos para um exemplo.

- Dev fulana criou um novo recurso no sistema de roteamento da aplicação.
- Testes passaram no CI. Tudo lindo! Pronto para enviar para produção.
- Ela inteligentemente seleciona qual grupo irá receber a modificação e qual não, que chamamos de grupo de controle.
- A dev fica atenta aos logs da aplicação e monitora qualquer anomalia ou reclamação.
- Se depois de algum tempo, tudo parece normal, ela passa a alteração para 100% dos usuários.

À depender de qual estratégia a dev tenha escolhido, lhe permitirá mais ou menos granularidade na "passagem" desse código para produção.

Imagina você poder liberar uma modificação para apenas 10% dos seus usuários, e aos poucos ir subindo para 100%, conforme você vai vendo que nenhuma regressão ocorreu.

Isso te dá uma confiança enorme que se algo der errado, isso é facilmente reversível e terá o impacto minimizado. Veja que a palavra que escolhi foi "minimizado", pois sempre haverá impacto. Não existe um sistema 100% à prova de problemas. **Não é sobre se vai acontecer, mas quando**. E nesse quanto, que seja o menos destrutível.

# Testes podem enganar

Alto percentual de coverage não significa muita coisa, até porque a efetividade dele depende de quão abragente seus casos de teste são em primeiro lugar.

Mais um exemplo:

- Dev fulano criou um wrapper para Elastic Search para facilitar a interação com esse serviço
- Vários testes foram adicionados nesse módulo e o relatório diz que 100% das linhas foram cobridas com testes
- Após fazer o deploy e alguns usuários usarem o sistema, relatos aparecem dizendo que buscas com letras maiúsculas e minúsculas misturadas não funcionam

100% de coverage, ainda assim, erros. Provavelmente faltou uma outra camada de teste aí como a de integração.

Entretanto, é bom salientar que embora testes de integração resultem em alta confiabilidade, eles carregam um custo caro: são mais lentos de executar e de implementar. Evite testes de integração desnecessários, em especial se esses já existem dentro da biblioteca que você está usando, afinal, você não quer testar a mesma coisa duas vezes, né?

# Developer Experience

Eu customo dizer que se durante um on-boarding tecnico o novo integrante da equipe precisa de mais de uma hora pra fazer o setup da aplicação, isso está errado.

Já vi várias vezes sistemas que requeriam milhares de etapas pra você apenas começar a desenvolver. Isso é um sintoma de que algo está muito errado na estrutura desse projeto.

Uma aplicação tem que ser iniciada com um comando: `make start`. O script de inicialização tem que abstrair toda a complexidade e preferencialmente tem que ser o mesmo script que rodará em CI e produção. Alinhar a maneira de execução entre ambientes é muito importante para manter a sanidade e razoabilidade do projeto.

Queremos um README.md coeso e direto ao ponto, documentando todo o necessário para instalar, rodar, testar e contribuir com o projeto.

O projeto não pode demorar 5 minutos toda vez que você precisa recompilar algo, isso é totalmente improdutivo! Identifique o gargalo e atue nele o quanto antes. Alguns projetos altamente containerizados podem apresentar muita lentidão em ambientes locais, portanto, crie um script separado para desenvolvimento mas que mantenha a mesma "API" para com os outros ambientes.

Alinhe com sua equipe de todos utilizarem em seus editores as mesmas ferramentas de formatação, linting, e de todo mundo ter o "format on-save" ativado pra isso ser menos uma coisa no caminho.

Se você trabalha com uma linguagem fortemente tipada, você vai querer que todos os possíveis problemas cheguem nem em tempo de execução, nem em tem de compilação mas em **tempo de desenvolvimento**, ou seja, o editor já aponta os erros enquanto você escreve o código.

```
Ótimo          Bom         Mal        Péssimo
    <--------------------------------->
    TD          TC         TED        TEP
```

- TD: Tempo de Desenvolvimento
- TD: Tempo de Compilação
- TE: Tempo de Execução em Desenvolvimento
- TU: Tempo de Execução em Produção

# Observalibity / Monitoring / Logs

Só porque seu sistema se comportou bem nos últimos sete dias, não quer dizer que isso se manterá assim até amanhã ;)

## De olho nas métricas de recursos

Pode parecer bobeira, mas indicadores de RAM, CPU e leitura em disco podem dizer muito sobre
o estado de saude de uma aplicação.

Se a RAM está alta, pode ser o jeito com que sua aplicação gerencia a memória. Você não tá carregando arquivos grandes diretamente, né? Use streams pra isso ;) CPU ineficiente? Talvez faça sentido aproveitar os outros núcleos ociosos.

## Métricas personalizadas

Faz total sentido querer medir quanto tempo leva uma requisição desde quando chega na camada web, até sua resposta. Para isso podemos usar projetos como Open Telemetry para nos ajudar
a utilizar diferentes técnicas para recolher e agregar métricas, traces e logs.

Uma vez de posse desses datos, basta escolher um bom Data Consumption Solution para
poder visualizar tudo isso.
