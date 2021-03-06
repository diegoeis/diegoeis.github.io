---
title: Email Marketing a tortura - Parte 2
date: 2015-04-07 00:00 UTC
tags: desenvolvimento, front-end
---

Não é só porque você precisa fazer seus emails utilizando tabelas, você vai desdenhar das boas práticas que você aprendeu na escola. O código final de uma Newsletter vai ficar muito maior e tão complexo quanto o código de um site institucional simples. Outro ponto importante, é que você vai fazer muitas e muitas alterações até esse email ser publicado e muito provavelmente vai precisar duplicá-lo quando alguém ter a necessidade de enviar novos emails.

A estratégia que eu uso aqui é usar as tabelas como usamos os divs. As tabelas são necessárias e não tem por onde fugir. Isso já é um desconforto muito grande, por causa da quantidade de código gerada. Abaixo, seguem duas maneiras que uso aqui na Locaweb e nos projetos do Tableless.

## CSS Inliner
Alguns clientes de email (estou olhando pra você, Gmail) simplesmente ignoram a tag style no HEAD ou no BODY. Eu não os culpo: como você mostraria um layout dentro do seu próprio layout, sem que o CSS e o código desse layout pirata não quebrasse o layout do seu sistema? Esse é um desafio para galera dos clientes de email baseados em web, como é o caso do Yahoo! Mail e do Gmail. Por isso, para acabar com o problema pela raiz, eles simplesmente estripam a tag style do HTML do email marketing, aceitando apenas o atributo style diretamente nos elementos.

O problema é que o seu trabalho é fazer newsletters ou você faz muitas newsletters por semana, a necessidade de reutilizar código é importante. Por isso o fluxo de desenvolvimento de emails precisa ser muito parecido com o desenvolvimento em ambientes normais, usando um CSS externo, provavelmente dividido por módulos e etc. Embora o HTML fique um horror de fazer manutenção por causa das tabelas, o CSS é a parte onde você vai passar mais tempo, por isso precisa ser o lugar mais organizado.

Para resolver esse problema, existem serviços online que pegam seu código CSS externo e colocam diretamente nas tags do seu HTML. Dependendo do seu layout o HTML final vai ficar um horror, mas isso não importa aqui. O HTML final você não vai se preocupar, porque a utilidade dele é simplesmente ser bem renderizado nos clientes.

Existem três serviços que fazem a mesma coisa: o [Inliner do PutsMail](https://putsmail.com/inliner), o [CSS Inliner Tool do MailChimp](http://templates.mailchimp.com/resources/inline-css/) e o [Inliner do Campaing Monitor](http://inliner.cm/).

Estas ferramentas já te ajudam um bocado. Mas ainda não é o fluxo ideal. Se você tem 10 emails para fazer, sua produtividade já fica prejudicada com estes serviços. Por isso eu passei a usar um plugin de Gulp no build local dos emails para fazer esse trabalho: é o [Gulp Inline CSS](https://www.npmjs.com/package/gulp-inline-css).

Se você prefere Grunt, tem um plugin que faz a mesma coisa, chamado [Grunt Inline CSS](https://www.npmjs.com/package/grunt-inline-css). O problema é que até a escrita deste artigo, esse plugin dependia de outro chamado [Juice](https://www.npmjs.com/package/juice), que faz a mágica por baixo dos panos, mas que estava dando uns paus loucos no Mac. Se você tiver paciência para testar, existem vários outros lá no NPM.

Como usamos o Middleman aqui, coloquei pra chamar a task do Gulp para ser executada depois do build do Middleman e pronto! O Build já me devolve os emails com o código direto no atributo style dos emails. Para comparar, o código de desenvolvimento fica assim:

<pre class=“lang-html”>
&lt;table cellpadding=&quot;10&quot;&gt;
  &lt;tr&gt;
    &lt;td class=&quot;lw-text&quot;&gt;

      &lt;h3&gt;&lt;strong&gt;O per&amp;iacute;odo de teste est&amp;aacute; chegando ao fim!&lt;/strong&gt;&lt;/h3&gt;

      &lt;p&gt;Ol&amp;aacute; &lt;/p&gt;

      &lt;p&gt;N&amp;atilde;o perca tempo e mantenha o seu site no ar agora mesmo com 10% de desconto no primeiro m&amp;ecirc;s. &lt;/p&gt;

      &lt;br&gt;
        &lt;center&gt;
          &lt;a href=&quot;###&quot;&gt;&lt;img alt=&quot;Quero manter meu site&quot; src=&quot;2014-11-21-c.png&quot;&gt;&lt;/a&gt;
        &lt;/center&gt;

    &lt;/td&gt;
  &lt;/tr&gt;
&lt;/table&gt;
</pre>

E o código buildado fica assim:

<pre class=“lang-html”>
&lt;table class=&quot;lw-wrapper&quot; bgcolor=&quot;#e7e2dc&quot; style=&quot;background-color: #e7e2dc; font-family: arial, helvetica, sans-serif; padding: 20px; width: 100%;&quot;&gt;
&lt;tr&gt;
  &lt;td&gt;

    &lt;table align=&quot;center&quot; class=&quot;lw-container&quot; style=&quot;background-color: #fff; border: none; border-collapse: collapse; margin: 0 auto; width: 630px;&quot; bgcolor=&quot;#ffffff&quot;&gt;
      &lt;tr&gt;
        &lt;td class=&quot;lw-content&quot; style=&quot;background-color: #fff; margin: 0 auto; width: 630px;&quot;&gt;
            &lt;img src=&quot;2014-11-21-5.jpg&quot; style=&quot;border: none; text-decoration: none;&quot; width=&quot;630&quot;&gt;

            &lt;table cellpadding=&quot;10&quot;&gt;
  &lt;tr&gt;
    &lt;td class=&quot;lw-text&quot; style=&quot;margin: 0 20px;&quot;&gt;

      &lt;h3&gt;&lt;strong&gt;O per&amp;#xED;odo de teste est&amp;#xE1; chegando ao fim!&lt;/strong&gt;&lt;/h3&gt;

      &lt;p style=&quot;font-family: arial, helvetica, sans-serif; font-size: 14px;&quot;&gt;Ol&amp;#xE1; &lt;/p&gt;

      &lt;p style=&quot;font-family: arial, helvetica, sans-serif; font-size: 14px;&quot;&gt;N&amp;#xE3;o perca tempo e mantenha o seu site no ar agora mesmo com 10% de desconto no primeiro m&amp;#xEA;s. &lt;/p&gt;

      &lt;br&gt;
        &amp;lt;center&amp;gt;
          &amp;lt;a href=&amp;quot;###&amp;quot;&amp;gt;&amp;lt;img alt=&amp;quot;Quero manter meu site&amp;quot; src=&amp;quot;2014-11-21-c.png&amp;quot;&amp;gt;&amp;lt;/a&amp;gt;
        &amp;lt;/center&amp;gt;

    &amp;lt;/td&amp;gt;
  &amp;lt;/tr&amp;gt;
&amp;lt;/table&amp;gt;
</pre>

Além disso tudo, nós usamos SASS para escrever o CSS. Lembre-se: a ideia é tentar ganhar velocidade no desenvolvimento, aproximando o ambiente que você está acostumado para a criação de emails. O problema é que embora você consiga produzir código rapidamente, você vai precisar testar esse layout e você não poderá fazer isso o tempo inteiro olhando para seu browser comum. Os usuários verão isso em diversos clientes de email e você precisa testar nesses clientes. É aí que entra outra ferramenta: Litmus.

## Testando com o Litmus
O [Litmus](http://litmus.com/) é um ambiente de testes e análise de email marketing que surgiu em 2005. Além de testar seus emails, você consegue ter uma série de informações sobre taxa de abertura, forwards, deleções e outras análises. Basicamente o que os outros caras do mercado fazem, ele faz também. O grande diferencial dele é que você consegue submeter o código do seu email e ele mostra como o layout fica em praticamente todos os clientes de email do mercado, inclusive nos clientes mobile.

Além disso, você consegue criar ou modificar o código do seu email diretamente pelo sistema deles, facilitando MUITO a adequação do código, principalmente porque eles tem um syntax checker que verifica se você está usando alguma regra que não funciona nos clientes de email do mercado.

Há uma feature deles chamada Interactive Testing. Esse negócio conecta em uma máquina virtual para você, com o seu layout aberto no cliente de email e um lugar para você modificar seu código. Quando você muda alguma coisa no código, ele muda no cliente de email! Pensa na utilidade desse negócio.

### PutsMail
O [PutsMail](https://PutsMail.com/) é um sisteminha de testes avulso. Ele é muito fácil de usar e nem precisa de cadastro. Basta jogar seu código no campo e mandar testar. Ele foi desenvolvido por um cara chamado Pablo Cantero, aí o Litmus comprou o sistema dele em 2014 e continuou o desenvolvimento.

## Concluindo
Email bom é email em Plain Text. Não escondo isso de ninguém. Quando fazemos emails, voltar a trabalhar como nos tempos antigos e isso não é bom. Você precis alinhar muito bem as expectativas do designer, precisa testar muito bem cada cliente de email importante, precisa escrever código ruim para que funcione em todo lugar. Mas não é impossível. Com a experiência e as ferramentas que surgiram nos últimos anos, conseguimos passar pelo vale da sombra e da morte mais facilmente. Mas ainda assim nos machucamos bastante. (ó, fiz até um final filosófico! ;-D )