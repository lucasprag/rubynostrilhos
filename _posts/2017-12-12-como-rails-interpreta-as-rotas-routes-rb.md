---
layout: post
title: Como Rails interpreta as rotas? routes.rb
tags: [routes]
thumbnail: "assets/img/posts/routes.png"
feature-img: "assets/img/posts/routes.png"
author: lucasprag
---

A gente viu no post sobre [rails on rake](http://rubynostrilhos.com.br/2017/12/07/como-rails-interpreta-as-rotas-rack.html) que uma requisição passa pelos middlewares do rack antes de chegar nas regras definidas no arquivo `config/routes.rb` da nossa aplicação rails. Agora vamos falar mais sobre essas regras e como defini-las.

Essas regras são checadas na order de definição, ou seja, de cima para baixo nesse aqruivo. Esse é um exemplo do `routes.rb`:

```ruby

Rails.application.routes.draw do
  root 'blog#index'
end
```

Veja que nesse arquivo nós passamos um bloco para o método `Rails.application.routes.draw`, e esse bloco será o lugar onde nós vamos `desenhar` as nossas rotas. A única rota desenhada no exemplo acima é a rota principal, a rota que vai responder para o caminho `/` que definimos com o método `root`. Essa rota é encaminhada para a action `index` do controller `blog` (`blog_controller.rb`).

Existem diferentes formas de definir rotas no rails:

## rota por rota

```ruby

Rails.application.routes.draw do
  root 'blog#index'

  get '/about' => 'blog#about'
end
```

Nós podemos definir uma rota com esse padrão: `verbo-http '/rota' => 'controller#account', {hash: 'com opções'}`

Quando nós definimos uma rota, estamos também criando um método para usar essa rota na nossa view. A rota do exemplo acima disponibiliza o método `about_path` para ser usado em links, forms, etc como no exemplo abaixo:

```erb
<%= link_to 'About', about_path %>
```

Também é possível mudar o nome desse helper que usamos nas views usando um hash de opções no final da definição da rota:

```ruby

  get '/about' => 'blog#about', as: :about_the_company
```

Usando o helper criado acima:

```erb
<%= link_to 'About', about_the_company_path %>
```

Perceba que esses helpers são baseados na rota que você criou. Por exemplo se você criar uma rota `/company` o helper será `company_path` e se você criar a rota `/awesome` o helper será `awesome_path`.


Outros exemplos de rotas:

```ruby

Rails.application.routes.draw do
  root 'blog#index'

  get    '/posts'          => 'posts#index'
  get    '/posts/new'      => 'posts#new'
  get    '/posts/:id'      => 'posts#show'
  get    '/posts/:id/edit' => 'posts#edit'
  post   '/posts'          => 'posts#create'
  put    '/posts'          => 'posts#update'
  delete '/posts/:id'      => 'posts#destroy'

  # helpers que essas rotas criam:
  # - posts_path
  # - post_path(@post)
  # - new_post_path
  # - edit_post_path(@post)
end

```

No exemplo acima nós temos um CRUD (Create, Update, Retrive, Delete) bem comum em aplicações web e por serem tão comuns nós podemos definir todas essas rotas através dos `resources`.

## resources

Com os resources nós podemos definir todas as rotas que um modelo precisa para ter seu CRUD básico funcionando, veja um exemplo:

```ruby
Rails.application.routes.draw do
  root 'blog#index'

  # fazer o resource rota por rota
  get    '/posts'          => 'posts#index'
  get    '/posts/new'      => 'posts#new'
  get    '/posts/:id'      => 'posts#show'
  get    '/posts/:id/edit' => 'posts#edit'
  post   '/posts'          => 'posts#create'
  put    '/posts'          => 'posts#update'
  delete '/posts/:id'      => 'posts#destroy'

  # declarando todas as rotas acima em uma única linha
  resources :posts
end

```

Nós não precisamos usar todas as rotas que sao construĩdas usando resources, nós também podemos definir apenas algumas delas usando opções:


```ruby
Rails.application.routes.draw do
  root 'blog#index'

  resources :posts, only: [:index, :show]
  resources :posts, except: [:create, :update, :destroy]
end

```

Também podemos criar resources que são dependentes de resources como por exemplo posts que tem muitos comentários (relacionamento de banco de dados 1 pra N):

```ruby
Rails.application.routes.draw do
  root 'blog#index'

  resources :posts do
    resources :comments
  end

  # as rotas ficam assim:
  # get /posts
  # get /posts/1/comments
  # get /posts/1/comments/1
end

```

Existe muito mais conteúdo sobre rotas como `scopes` e `namespaces` para quando você quer agrupar rotas (por exemplo criar um painel admin), `constraints` para quando sua rota não é tão simples como um `/about` vai para `page#about` e outros diversos assuntos interessantes.

Você pode se aprofundar mais sobre rotas [aqui](http://guides.rubyonrails.org/routing.html).

Se você tiver qualquer dúvida sobre rotas você pode criar uma issue [aqui](https://github.com/rubynostrilhos/forum) que eu vou te ajudar assim que possível.
