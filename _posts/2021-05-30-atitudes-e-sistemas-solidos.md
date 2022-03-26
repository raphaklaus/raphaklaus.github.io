---
title: 'Atitudes e Sistemas Sólidos'
article: true
date: 2021-05-30 16:00:00
tags: code quality
image: /assets/images/qualidade-de-software.jpeg
layout: post
---

Gostaria de compartilhar aqui o que aprendi nos últimos anos sobre como construir um sistema sólido.

<!--more-->

Um software começa geralmente com as especificações básicas e um codebase vazio, ou mesmo um boilerplate.
Tudo parece perfeito... Começamos a adicionar funcionalidades, vamos introduzindo mais camadas de abstração, escrevemos testes e as coisas parecem estar indo lindamente.

Então o primeiro incidente em produção acontece. Ué!? Mas não era para isso ter sido previsto pelas nossas suítes robustas de testes? Você sobe um hotfix com a correção + cenário de teste que garante que a situação não ocorrerá novamente e vai dormir tranquilo...

A sua equipe continua criando novas funcionalidade como previsto, e então, um incidente grave... Você faz o hotfix, escreve os teste e vai dormir tranquilo...

Parece que há um padrão aí.

## Mitigando os riscos de um deploy

Deploys não deveriam ser tarefas complexas que exigem dias reservados na agenda da equipe. Muito menos é para ser temido.

É insanidade fazer um deploy de uma determinada modificação, para de cara isso ficar disponível para todos os usuários, isso tem um grande potencial catastrófico!

Vamos ver à seguir opções para diminuir o alcance de um incidente.

### Blue-green / Canary / Feature Flags para a salvação

O que esses três métodos de deploy tem em comum é muito simples: minimizar o possível impacto. Essas estratégias buscam criar "branches" à nível de infraestrutura/código. Vamos para um exemplo.

- Dev fulana criou um novo recurso no sistema de roteamento da aplicação.
- Testes passaram no CI. Tudo lindo! Pronto para enviar para produção.
- Ela inteligentemente seleciona qual grupo irá receber a modificação e qual não, que chamamos de grupo controle.
- A dev fica atenta aos logs da aplicação e monitora qualquer anomalia ou reclamação.
- Se depois de algum tempo, tudo parece normal, ela passa a alteração para 100% dos usuários.

À depender de qual estratégia a dev tenha escolhido, lhe permitirá mais ou menos granularidade na "passagem" desse código para produção.

Imagina você poder liberar uma modificação para apenas 10% dos seus usuários e ir aumentando conforme vai vendo que nenhuma regressão ocorreu até chegar em 100%. Isso te habilita a publicar novas modificações com maior segurança.

Se algo der errado, isso é facilmente reversível, quer dizer, você não precisa fazer um revert no Git e esperar um novo deploy rolar. Isso implica em uma velocidade de resposta maior ao incidente.

Em software, um código ruim sempre poderá traduzir-se em um potencial impacto. Não existe um sistema 100% à prova de problemas. **Não é sobre se vai acontecer, mas quando**. E nesse quando, que seja o menos destrutível.

Quem tiver curioso sobre as diferenças entre Blue-green, Canary e Feature Flags, pode checar [essa](https://github.com/raphaklaus/feature-flag-talk) talk que apresentei ano passado. Spoiler: Feature Flags ftw!!!

## Testes

Acho que hoje em dia escrever testes tá ficando uma prática padrão para a maioria dos desenvolvedores. Pessoalmente, prefiro trabalhar com *TDD (Test-driven development)* sempre que possível. A vantagem disso é clarear os requisitos e interfaces, e melhorar o design do código antes de qualquer implementação. E claro, você garante de sempre ter testes para as suas implementações.

Use ferramentas para coletar o percentual de cobertura de testes e coloque um limite mínimo para esse percentual, que deverá ser seguido estritamente. Exemplo: Cobertura mínima de 80% ou não poderá fazer merge. 

Testes é um mundo à parte, então procure entender todo o cinturão de ferramentas da sua linguagem. Em geral, as ferramentas mais comuns são:

* Um gerenciador de fixtures/factories com suporte à bancos de dados e facil criação de relacionamentos. Tipo um [ExMachina](https://github.com/thoughtbot/ex_machina).
* Um bom utilitário mocker/stubber quando você precisar "forjar" a resposta de um servidor ou chamada à um serviço qualquer.
* Um *test runner* eficiente e de preferência que tenha uma boa integração com seu editor. Mundo perfeito: alterou uma linha, o *test runner* roda o teste associado e já trás um feedback.

Um detalhe imprescindível é ter em conta que código de teste **deve** ser limpo e de fácil entendimento. Tem que ser o mais óbvio possível, pois é nele que outros desenvolvedores irão para entender como o código funciona.

### Fragilidade

Alto percentual de coverage não significa muita coisa, até porque a efetividade dele depende de quão abragente seus teste são em primeiro lugar.

Mais um exemplo:

- Dev fulano criou um wrapper para Elastic Search para facilitar a interação com esse serviço.
- Vários testes foram adicionados nesse módulo e o relatório diz que 100% das linhas foram cobertas com testes.
- Após fazer o deploy e alguns usuários usarem o sistema, relatos aparecem dizendo que buscas com letras maiúsculas e minúsculas misturadas não funcionam

100% de coverage, ainda assim, erros. Provavelmente faltou aí uma outra camada de teste, como a de integração.

Entretanto, é bom salientar que embora testes de integração resultem em alta confiabilidade, eles carregam um alto custo: são mais lentos de executar e de implementar. Evite testes de integração desnecessários e foque exatamente no ponto de troca das aplicações.

## Developer Experience

Eu customo dizer que se durante um on-boarding técnico o novo integrante da equipe precisa de mais de uma hora pra fazer o setup da aplicação, isso está errado.

Já vi várias vezes sistemas que requeriam milhares de etapas pra você apenas começar a desenvolver. Isso é um sintoma de que algo está muito errado na estrutura desse projeto.

Uma aplicação tem que ser iniciada com um comando: `make start`. O script de inicialização tem que abstrair toda a complexidade e preferencialmente tem que ser o mesmo script que rodará em CI e produção. Alinhar a maneira de execução entre ambientes é muito importante para manter a sanidade e razoabilidade do projeto.

Queremos um README.md coeso e direto ao ponto, documentando todo o necessário para instalar, rodar, testar e contribuir com o projeto.

O projeto não pode demorar 5 minutos toda vez que você precisa recompilar algo, isso é totalmente inaceitável. Identifique o gargalo e atue nele o quanto antes. Alguns projetos altamente containerizados podem apresentar muita lentidão em ambientes locais, portanto, crie um script separado para desenvolvimento mas que mantenha a mesma "API" para com os outros ambientes. Um exemplo é usar o [ASDF](https://asdf-vm.com/) localmente.

Alinhe com sua equipe de todos utilizarem em seus editores as mesmas ferramentas de formatação, linting, e de todo mundo ter o "format on-save" ativado pra isso ser menos uma coisa no caminho. A maneira mais agnóstica é usar o [EditorConfig](https://editorconfig.org/) para formatação básica e depois usar formatadores e linters específicos.

Se você trabalha com uma linguagem fortemente tipada, você vai querer que todos os possíveis problemas cheguem nem em tempo de execução, nem em tempo de compilação mas em **tempo de desenvolvimento**, ou seja, o editor já aponta os erros enquanto você escreve o código.

```
Ótimo          Bom         Mal        Péssimo
    <--------------------------------->
    TD          TC         TED        TEP
```

- TD: Tempo de Desenvolvimento
- TD: Tempo de Compilação
- TE: Tempo de Execução em Desenvolvimento
- TU: Tempo de Execução em Produção

## Observalibity / Monitoring / Logs

Só porque seu sistema se comportou bem nos últimos sete dias, não quer dizer que isso vai se manter assim até amanhã ;)

Pode parecer bobeira, mas indicadores de RAM, CPU e leitura em disco podem dizer muito sobre o estado de saude de uma aplicação.

Se a RAM está alta, pode ser o jeito com que sua aplicação gerencia a memória. Você não tá carregando arquivos grandes diretamente, né? Use streams pra isso ;) CPU ineficiente? Talvez faça sentido aproveitar os outros núcleos ociosos. Talvez uma implementação do Actor Model venha à calhar.

APM são soluções completas para inspecionar a saúde do seu sistema da requisição às camadas mais internas como banco de dados e cache. É imperativo ter essa visibilidade, mas eu entendo que alguns APMs são muito caros e portanto ficam fora do orçamento de muitas equipes.

O que se pode fazer nesses casos é coletar as métricas individualmente através do [Open Telemetry](https://opentelemetry.io/) e agregar-las em um *Data Platform* e aí criar os dashboards e alarmes. Muitas equipes fazem isso, e funciona muito bem, só não é tão *out of the box* como os APMs.

Importante é você conseguir ter uma visão macro, mas também quando necessário, uma visão micro. Tenha certeza que o *Data Platform* escolhido te permite
realizar consultas no estilo "SQL". Tenha em mente que tais consultas podem ser lentas dependendo da quantidade de informação, então considere usar *sampling* (amostragem, ao invés de ter que consultar toooodos os dados) quando te convier.

Vou linkar aqui alguns serviços interessantes que já usei e recomendo por ordem de facilidade de uso (mais fácil primeiro): AppSignal, New Relic, DataDog, HoneyComb, Splunk

### Alarmes

Você provavelmente não quer ficar olhando o dia todo para um dashboard de métricas, né? 

Crie alarmes inteligentes que vão te notificar e sua equipe caso ultrapasse um determinado valor-limite. Mas mais importantemente, escolha um valor-limite junto com a janela de tempo da ocorrência, pois você não quer que falsos positivos
fiquem te tirando a concentração constantemente. Quero dizer, quando um alarme tocar, algo está próximo de ficar ruim e você ainda tem tempo de agir.

O pior sistema de alarme possível é aquele que os membros da equipe silenciam porque toda hora é algum evento irrelevante. Se esse é o caso na sua equipe, repense como esses alarmes estão configurados.

## Gerenciamento de Incidentes

É final de Dezembro e no dia anterior você teve uma festa de confraternização da empresa e acordou com uma baita ressaca, mas
você tem que trabalhar! Então você pega o notebook e trabalha de casa, deitado na cama. Eis que chega a mensagem do CEO dizendo que os sistemas pararam e que isso não pode acontecer pois eles acabaram de investir MUITO dinheiro no lançamento de um recurso novo. Qualquer semelhança com a realidade é mera coincidência.

Nesse caso a primeira coisa que você faz é abrir o sistema para ver "com os próprios olhos" o que está pegando.

Os dados não carregam. Aí você começa uma série de verificações.

O ponto principal aqui é se perguntar:

* Quando começou a acontecer?
* Houve algum evento que desencadeou isso? Como um deploy, mudança de servidor, migração, etc...
* Consigo simular a situação localmente?
* É crítico?

Fazendo essas perguntas, você terá um algo mais tangível para trabalhar, independente do tipo de problema que você vai enfrentar.

Em Gerenciamento de Incidentes há diferentes estratégias para se aplicar. A mais comum é você ter a figura de um
*Incident Manager* que pode ser qualquer pessoa da equipe. Ela será responsável por coordenar os esforços para
resolver a situação, seja resolvendo ela mesma, ou delegando para outros. Além disso, é seu papel atualizar todos as pessoas
afetadas e interessadas, incluindo outros desenvolvedores, gerentes de produto, representantes de outros departamentos, CEO, CTO, etc...

### Post-Mortem

Após a resolução do incidente, a equipe se reúne e cria um *Post-Mortem* que é basicamente um relatório que descreve todo o ocorrido, solução e aprendizados. Esse relatório geralmente é assim:

```
Linha do tempo:

08:55: Deploy de nova funcionalidade ocorreu.

09:02: Suporte começou a receber ligações de clientes relatando que a página de produtos não estava carregando.

09:10: Equipe dev ficou sabendo e começou análise.

09:14: Foi notado um alto número de requisições com resposta 500. Todas do endpoint GET /products

09:20: Verificado que o CI não rodou as migrações que deveriam, portanto o erro de banco de dados "relation not found".

09:22: Migração rodada com sucesso.

09:25: Notificamos o suporte que a situação já estava corrigida e que poderia verificar com os usuários também

10:25: Incidente fechado, nenhuma reclamação relatada na janela de 1 hora.

Incidente ocorrido:

Produtos não carregavam para todos usuários.

Resposta:

Recebemos um aviso da equipe de suporte de que nossos clientes não conseguiam ver a página de produtos. Começamos a investigar
nossos APM's buscando por mais detalhes.

Detecção:

Identificamos o erro "relation not found" no stack trace do ciclo de vida da requisição, indicando que o servidor de CI não rodou as migrações.

Recuperação:

Rodamos as migrações manualmente e o erro parou de ocorrer.

Causa raíz:

Não há nenhuma etapa automatica para rodar as migrações.

Ações corretivas:

Adicionaremos uma etapa para rodar migrações toda vez que houver um deploy.
```

Esse template é fortemente baseado no da [Atlassian](https://www.atlassian.com/incident-management/handbook/postmortems#postmortem-meetings) que eu recomendo muito dar uma checada.

Uma coisa importante, essas sessões de Port-Mortem não devem ser orientadas a encontrar culpados. Ela deve focar em fazer a equipe como um todo tomar
ciência do ocorrido e aprender juntos maneiras de evitar que isso volte a ocorrer.

## Segurança

Eu não sou nenhum especialista no assunto, mas o básico que todo mundo tem que ter em mente é:

* Injections (SQL, No-SQL, etc)
* XSS (Cross-site Scripting)
* CSRF (Cross-site Request Forgery)
* Má configuração de segurança (não uso do HTTPS/SSL em comunicações em geral, cloud storage público, etc...)
* Execução de código remoto
* Uso de bibliotecas/módulos com vulnerabilidades conhecidas

Geralmente os dois primeiros itens da lista (Injections e XSS) os frameworks modernos de web já sabem corrigir. Já os outros você na maioria das vezes precisará lidar especificamente.

Recomendo olhar o [top 10 vulnerabilidades web da OWASP](https://owasp.org/www-project-top-ten/), que é uma organização sem fins lucrativos para a disseminação do conhecimento de segurança de software.

Sobre o último item (Uso de bibliotecas/módulos com vulnerabilidades conhecidas), é super facil de contornar, basta instalar o [Dependabot](https://dependabot.com/) no seu repositório e o serviço irá gerar *Pull Requests* automaticamente com as atualizações das dependências da sua aplicação. Isso aqui é **obrigatório** se você se importa com segurança minimamente.

Por fim, **sempre use two-factor authentication!**

## Conclusão

Há uma série de práticas que podem ser aplicadas para transformar a equipe e o produto. Basta haver espaço para essas transformações.

O que você acha? Alguma outra prática que você não viu aqui e merece ser mencionada? Coloca aí nos comentários!