---
title: Como criar um blog utilizando Wintersmith
author: joaofelix
date: 2013-06-06 10:00
template: article.jade
---

Já que o objetivo é compartilhar o aprendizado de todo dia, o primeiro artigo será sobre a minha descoberta dos geradores de conteúdos estáticos.

<span class="more"></span>

### A Vontade
A pouco tempo atrás, acompanhando o [Zofe 6][1], começei a dar atenção aos geradores de conteúdos estáticos como alternativa facilitadora ao desenvolvimento de sites que não necessitassem de paineis adiministrativos e etc., o que se encaixam nos padrões dos meus sites pessoais.

### O que são os Geradores de Conteúdos Estáticos? (O que eu entendi)

Diferente de plataformas como Wordpress, Joomla e etc., os geradores de conteúdo estáticos servem para criar páginas de conteúdo sem a utilização de qualquer persistência ou dependência server-side, onde ao final, você possui um pacote de arquivos html e seus companheiros (css,javascript,imagens...).

### O [Wintersmith][6] [(código)][7]

O porto de partida foi a escolha de qual gerador utilizar. Eu já possuia uma queda pelo [Jekyll][2] (devido à diversos blogs que eu acompanho o utilizarem) e uma lista de [32 Geradores de Conteúdo Estáticos][3], porem, eu não queria utilizar [Ruby][4] (simplesmente não queria) e decidi procurar algo em [Node.js][5], eis que surge o [Wintersmith][6]!

O projeto [Wintersmith][6] MIT; É open-source e utiliza outros projetos open-source para criar um ambiente bem simples! Foi criado utilizando o [Markdown][8] como conversor de texto para HTML, [Jade][9] para criação de templates e [Coffescript][10] para os plugins! (Muita coisa legal para aprender!!)

### Instalando o [Wintersmith][6]

Neste artigo eu vou adotar como precedência que você já tenha o [Node.js][5] instalado e saiba do que se trata o [npm][11], mas nada impede de aprendermos mais sobre eles em um outro momento; Segue o comando de instalação:

```
$ npm install wintersmith -g
```

### Criando o projeto

Estando com o pacote instalado, basta criar o seu projeto na pasta que desejar. 

```
$ wintersmith new <path>
```

O template padrão do projeto é o de blog (existem também basic e webapp), o que atende bem à minha necessidade, mas você pode criar outros. Junto com a estrutura de arquivos, o [Wintersmith][6] já criará um conjunto de artigos bem explicativos que mostram o potencial da ferramenta, para visualizar como está o seu projeto, basta coloca-lo em peview:


```
$ cd <path>
$ wintersmith preview
```

Ai pronto! Você já pode acessar http://localhost:8080 e ver o que está criado (Vale lembrar que enquanto estiver visualizando, qualquer alteração é refletida na pré-visualização sem a necessidade de executar o comando novamente).

### Personalizando o blog

Como a estrutura criada pelo projeto já é bem próxima à de um blog bem enxuto, precisei apenas fazer algumas mudanças de parametros, alterar os autores (dos artigos, claro!) e criar um template jade para os comentários (utilizando [Disqus][12]).

Na raiz do projeto existe um arquivo chamado config.json e o padrão dele é:
```json
{
  "locals": {
    "url": "http://localhost:8080",
    "name": "The Wintersmith's blog",
    "owner": "Someone",
    "description": "Ramblings of an immor(t)al demigod"
  },
  "plugins": [
    "./plugins/paginator.coffee"
  ],
  "require": {
    "moment": "moment",
    "_": "underscore",
    "typogr": "typogr"
  },
  "jade": {
    "pretty": true
  },
  "markdown": {
    "smartLists": true,
    "smartypants": true
  },
  "paginator": {
    "perPage": 3
  }
}
```
E após as minhas alterações, eu o deixei assim:
```json
{
  "locals": {
    "url": "http://oqueaprendihoje.com.br",
    "name": "O Que Aprendi Hoje",
    "owner": "João Felix",
    "description": "A saga de um aprendiz encantado com o universo do Front-end",
    "keywords" : ["javascript","css","front-end","web","development"],
    "disqus": {
      "id" : "oqueaprendihoje"
    }
  },
  "plugins": [
    "./plugins/paginator.coffee"
  ],
  "require": {
    "moment": "moment",
    "_": "underscore",
    "typogr": "typogr"
  },
  "jade": {
    "pretty": true
  },
  "markdown": {
    "smartLists": true,
    "smartypants": true
  },
  "paginator": {
    "perPage": 10
  }
}

```

Basicamente alterei as propriedades do blog, como nome, proprietário, descrição e palavras-chaves, alem de alterar o número de paginas e inserir o objeto "disqus" para utilizarmos no controle de comentários; Vale ressaltar que o objeto "locals" fica acessivel aos templates também.

Outra alteração necessária foi a criação de um autor, para isso, removi os autores padrões e adicionei o meu na pasta "contents/authors"; O identificador de autor no artigo é o nome do arquivo e o conteúdo é um objeto JSON:

```json
{
    "name":   "João Felix",
    "email":  "jr8116[at]gmail[dot]com",
    "bio":    "Aprendiz, desenvolvedor e empreendedor!"
}
```

### Comentários nos artigos

Todo blog que se preze, precisa aceitar comentários. Para isso, utilizei a solução [Disqus][12], que me permite apenas pela referência do utilizador e da página atual, disponibilizar para os usuários uma forma de comentar os artigos.

Para manter o padrão de organização do projeto, eu criei um [template jade][9] chamado 'disqus.jade" e o coloquei na pasta "templates" (Também o dinamizei para utilizar o id do objeto disqus que determinamos); Este arquivo nada mais é do que a transposição do código fornecido pelo [Disqus][12]:
```xml
    <div id="disqus_thread"></div>
    <script type="text/javascript">
        var disqus_shortname = 'oqueaprendihoje'; 
        (function() {
            var dsq = document.createElement('script'); dsq.type = 'text/javascript'; dsq.async = true;
            dsq.src = '//' + disqus_shortname + '.disqus.com/embed.js';
            (document.getElementsByTagName('head')[0] || document.getElementsByTagName('body')[0]).appendChild(dsq);
        })();
    </script>
    <noscript>Please enable JavaScript to view the <a href="http://disqus.com/?ref_noscript">comments powered by Disqus.</a></noscript>
    <a href="http://disqus.com" class="dsq-brlink">comments powered by <span class="logo-disqus">Disqus</span></a>
```
Para o padrão de escrita [Jade][9]:
```python
div(id="disqus_thread")
script.
       var disqus_shortname = "#{disqus.id}";
        (function() {
            var dsq = document.createElement('script'); dsq.type = 'text/javascript'; dsq.async = true;
            dsq.src = '//' + disqus_shortname + '.disqus.com/embed.js';
            (document.getElementsByTagName('head')[0] || document.getElementsByTagName('body')[0]).appendChild(dsq);
        })();
noscript
    Habilite o Javascript do seu browser para ver os
    a(href="http://disqus.com/?ref_noscript") comentários mantidos pelo Disqus.
a.dsq-brlink(href="http://disqus.com")
    comments powered by
    span.logo-disqus
        Disqus
```
(Este foi meu primeiro contato com o [Jade Template][9], acredito que em breve escreverei sobre um aprendizado mais aprofundado)

Com o template pronto, basta inclui-lo no template "article.jade" que também está no diretório "templates":

```python
  include disqus
```

Resultado final do template:
```python
extends layout

block append vars
  - bodyclass = 'article-detail'

block prepend title
  | #{ page.title + " - "}

block header
  include author
  h1= page.title
  p.author
    | #{ "Escrito por " }
    mixin author(page.metadata.author)

block content
  article.article
    section.content!= typogr(page.html).typogrify()


block prepend footer
  include disqus
  div.nav
    a(href=contents.index.url) « Lista
```

Também fiz algumas alterações por menores, como: Exibir o Twitter e não o E-mail do autor, links para compartilhamento utilizando o [Addthis][15], inserção do [Google Analytics][16] e tradução dos links de avançar, ver mais e etc. que estão disponíveis no código fonte do blog.

### Publicando

Pronto! Nossa estrutura de blog está criada! Agora falta apenas gerar os arquivos finais para a publicação, que estarão disponíveis na pasta "build":
```
$ wintersmith build
```

### Acabamos!

 Neste momento, temos um blog pronto, porem, sem customização e apenas com o layout padrão do [Wintersmith][6]. Provavelmente no artigo de amanhã eu falarei sobre a customização visual do [Wintersmith][6], onde pretendo utilizar o [Pure.css][14]. Espero que tenham gostado do artigo e agradeço qualquer crítica, elogio ou comentário! Espero continuar com novos conteúdos e conto com a colaboração de todos! Até breve!

Todo o código está disponível no Github: [https://github.com/jr8116/oqueaprendihoje][13]


[1]: http://zofe.com.br/posts/zofe-6-o-que-voce-fez-hoje/
[2]: http://jekyllrb.com
[3]: https://iwantmyname.com/blog/2011/02/list-static-website-generators.html
[4]: http://www.ruby-lang.org/
[5]: http://nodejs.org
[6]: http://wintersmith.io/
[7]: https://github.com/jnordberg/wintersmith
[8]: http://daringfireball.net/projects/markdown/
[9]: https://github.com/visionmedia/jade
[10]: http://coffeescript.org/
[11]: https://npmjs.org/
[12]: http://disqus.com/
[13]: https://github.com/jr8116/oqueaprendihoje
[14]: http://purecss.io/
[15]: http://www.addthis.com
[16]: https://www.google.com/analytics/