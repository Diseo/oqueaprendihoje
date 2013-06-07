---
title: Como utilizar LESS para CSS Dinâmicos
author: joaofelix
date: 2013-06-07 10:25
template: article.jade
---

Sei que eu tinha escrito sobre utilizar o [Pure.css][1] na estilização do Blog, porem, decidi fazer algo mais ousado do que utilizar uma biblioteca pronta! Decidi aprender [LESS][2]!

<span class="more"></span>

### A motivação
Sempre sofri na hora de escrever guias de estilos bem organizadas, otimizadas e redundantes, porem, quando eu achava que estava melhorando, surgiu o tal do [Design Responsivo][4] (O [link][4] é um artigo bem legal do [Tableless][7]), que me fez ficar ainda mais preocupado com estes arquivos .css, pois os escopos [@media][3] almentaram muito a quantidade de códigos; Por estes motivos, decidi aprender sobre o [LESS][2], a linguagem do css dinâmico (Existem outras opçõs, como o [SASS][5], porem, denovo, não queria usar [Ruby][6])!

### O [LESS][2]
O [LESS][2], que está na sua versão 1.4.0 beta, é uma linguagem que extende ao CSS o comportamento dinâmicos de variáveis, operações, mixins e funções; O legal da linguagem é que ela roda tanto em server-side (com o [Node.js][8] e [Rhino][9]) quanto client-side (apenas browsers modernos). Para instala-lo, basta ter o [Node.js][8] instalado e com o [npm][10] ativo:

```
npm install -g less
```

### [LESS][2] no [Wintersmith][11]
Como o blog foi criado com o [Wintersmith][11] (se quiser saber como, [leia aqui][12]), utilizei o [plugin][13] do Johan Nordberg que já faz todo o trabalho de integrar as duas belezinhas. Olha o quanto é fácil:

```
$ npm install wintersmith-less -g
```
Com o pacote instalado, basta acessar o diretório do seu projeto [Wintersmith][11] e instalar o plugin:
```
$ cd <path>
$ npm install wintersmith-less -g
```
Simples, não!?

### Escrevendo [LESS][2]
Como não terei tempo para criar um bom layout em 2 horas de estudo, vou inicialmente transformar o css padrão do [Wintersmith][11] para o [LESS][2], deixando bem simplificado o aprendizado.

O primeiro passo, foi criar o arquivo "contents/css/main.less" (Que quando compilado, se torna o main.css), onde codificaremos todo o css do antigo arquivo main.css já existente na pasta "contents/css". Ai, foi só começar a escrever códigos;

Criei um arquivo para as variaveis de cores (Ótimo para a reutilização):

```css
@body-background-color: #f8f8f8;
@body-text-color: #171717;
@a-hover-color: #ff8000;
@hr-color: #d2d2d2;
```
(Confesso que cometi um erro besta; Gerei o arquivo, fiz o import e fui para a visualização para ver o que aconteceu, só que nada aparecia, devido às variaveis não serem compiladas)

Depois, separei os estilos em contextos,  criando um conjunto de:
	*	header.less
	*	commons.less
	*	footer.less
	*	article.less
	*	archive.less
	*	code.less

Que serão ao final da implementação, importados no arquivo main.css:
```css
@import "colors";
@import "functions";
@import "commons";
@import "header";
@import "footer";
@import "article";
@import "archive";
@import "code";
```
Uma das coisas mais divertidas do [LESS][2] é poder usar o encadeamento de códigos, o que economiza bastante código e facilita o entendimento do escopo, como o exemplo abaixo:

No css:
```css
footer {
  margin: 3em 0;
}

footer .nav {
  text-align: center;
  margin-top: 5em;
  margin-bottom: 3.5em;
}

footer .nav a {
  padding: 0 0.5em;
  font-size: 1.2em;
  text-decoration: none;
}
```

No [LESS][2]:
```css
footer {
    margin: 3em 0;

    .nav {
      text-align: center;
      margin-top: 5em;
      margin-bottom: 3.5em;

      a {
        padding: 0 0.5em;
        font-size: 1.2em;
        text-decoration: none;
      }
    }
}
```

Para as media queries, eu inseri diretamente nos contextos, para que ficasse mais facil o entendimento:

Como era no CSS:
```css
@media (min-width: 1600px) {
  body { font-size: 26px; }
}

@media (max-width: 900px) {
  body { font-size: 18px; }
}
```

No [LESS][2]:
```css
body {
  @media (min-width: 1600px) { font-size: 26px; }
  @media (max-width: 900px) { font-size: 18px; }
}
```

(Confesso que ai surgiu uma dúvida; Seria melhor separar por elemento as media queries, ou a media query deveria ser um contexto com a definição dos seus elementos?)

Para finalizar e aprender sobre os mixins, criei um arquivo "mixins.less" para armazenalos:
```css
.column-count(@count: 1) {
  -webkit-column-count: @count;
  -moz-column-count: @count;
  -ms-column-count: @count;
  column-count: @count;
}

.column-gap(@gap: 1em) {
  -webkit-column-gap: @gap;
  -moz-column-gap: @gap;
  -ms-column-gap: @gap;
  column-gap: @gap;
}
```
Estas funções ajudam muito para prever todos os prefixos proprietários!

### Acabamos

Feito as alterações, bastou compilar tudo e correr pro abraço! O que aprendi hoje não foi tão aprofundado, pelo tempo corrido, mas espero que tenham gostado! Um abraço!

Todo o código atualizado está disponível no Github: [https://github.com/jr8116/oqueaprendihoje][14]


[1]: http://purecss.io/
[2]: http://lesscss.org/
[3]: http://www.w3.org/TR/CSS2/media.html
[4]: http://tableless.com.br/design-responsivo-na-pratica-do-rascunho-ao-digita/
[5]: http://sass-lang.com/
[6]: http://www.ruby-lang.org/
[7]: http://tableless.com.br/
[8]: http://nodejs.org
[9]: https://developer.mozilla.org/en-US/docs/Rhino
[10]: https://npmjs.org
[11]: http://wintersmith.io/
[12]: /articles/como-criar-um-blog-usando-wintersmith/
[13]: https://github.com/jnordberg/wintersmith-less
[14]: https://github.com/jr8116/oqueaprendihoje