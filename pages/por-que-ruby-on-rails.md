---
layout: page
title: Por que Ruby on Rails?
permalink: /por-que-ruby-on-rails/
---

[Ruby](https://www.ruby-lang.org/) on [Rails](http://rubyonrails.org/) é uma ótima escolha com tudo o que você precisa para criar aplicações web rapidamente.

![Welcome to Rails](/assets/img/posts/rails5.jpg)

# Ruby

É uma linguagem com foco na simplificade e produtividade feita para funcionar da forma que você espera que ela funcione, por exemplo:

```ruby
5.times { print '->' }
```

Para fazer a mesma coisa em outras linguagens você precisaria fazer algo assim:

```javascript
(Array.apply(null, Array(5))).forEach(() => console.log('->') )
```

ou

```javascript
for (var i = 0; i < 5; i++) { print '->'; }
```

Veja que a implementação em Ruby não só é mais simples como também é mais intuitiva.

Criada em 1995 pelo Yukihiro “Matz” Matsumoto, é uma mistura das suas linguagens favoritas como Perl, Smalltalk, Eiffel, Ada e Lisp que formou um balanço de programação funcional com programação imperativa.

Veja mais no [site](https://www.ruby-lang.org) de linguagem.

# Rails

É um framework que já tem tudo o que você precisa para criar fantásticas aplicações web.

Você provavelmente já usou várias apps que foram feitas com Ruby on Rails como por exemplo: GitHub, Gitlab, Shopify, Airbnb, Twitch, SoundCloud, Zendesk, Iugu e Magnetis.

[David Heinemeier Hansson](https://twitter.com/dhh) criou o Rails em 2003 ao extrair parte do código do [Basecamp](https://basecamp.com/) e hoje ele continua liderando o desenvolvimento do framework que é open source.

Veja um exemplo de uma página que lista blog posts na tela, começando pelo arquivo de rota:

```ruby
Rails.application.routes.draw do
  root 'blog#index'
end
```

Agora podemos dar uma olhada no controller que chama o model que acessa o banco de dados e retorna os nossos posts:

```ruby
class BlogController < ApplicationController
  def index
    @posts = Post.all
  end
end
```

E agora a view apenas lista os nossos posts:


```erb
<%= @posts.each do |post| %>
  <h3><%= post.title %></h3>
  <p><%= post.content %></p>
<% end %>
```

Não espero que você entenda completamente como criar páginas usando Rails, esses trechos de código servem a apenas para você ter uma noção de como fazer uma página que lista informações vindas do banco de dados.

O Rails otimiza a felicidade do programador com a Convenção sobre Configuração que é um dos motivos que faz Rails ser tão aceito, onde você não precisa fazer diversas configurações para ter sua app pronta para você trabalhar no que realmente importa.

Ruby on Rails tem popularizado uma variedade de pontos controversos desde o início. Para saber mais sobre por que o Rails é tão diferente de muitos outros frameworks e paradigmas de aplicação web, veja [The Rails Doctrine](http://rubyonrails.org/doctrine/).

# Happy Coding! =)

