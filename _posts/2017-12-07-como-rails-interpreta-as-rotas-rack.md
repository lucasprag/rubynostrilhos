---
layout: post
title: Como Rails interpreta as rotas? Rack
tags: [rack, intermediario]
thumbnail: "assets/img/posts/rack.jpg"
feature-img: "assets/img/posts/rack.jpg"
author: lucasprag
---

Se você já criou alguma coisa com Rails sabe que você pode definir rotas no arquivo `routes.rb`, mas o que será que acontece antes?

Existe um certo caminho que uma requisição percorre antes de checar as suas rotas e é esse caminho que você verá nesse post.

Uma aplicação feita com Rails é baseada em [rack](https://rack.github.io/), que funciona como uma estante onde a requisição passa de prateleira em prateleira de cima para baixo e em cada prateleira acontece alguma coisa com a requisição que pode ser como validar headers, lidar com cookies, parsear o corpo dela, checar se existem migrações de banco de dados pendentes do Rails, etc. Essas prateleiras são chamadas de middlewares.

O rack por si só já tem diversos middlewares, mas em nossa aplicação Rails conseguimos definir alguns customizados. Veja um exemplo de um middleware que apenas adiciona um header "Hello":

```ruby
class HelloHeader
  def initialize(app)
    @app = app
  end

  def call(env)
    status, headers, body = @app.call(env)
    headers['Hello'] = 'World'
    [status, headers, body]
  end
end
```

Veja que você só precisa de uma classe com um método inicializador e outro método call, então você faz o que você quiser com o status, headers e body e então você os retorna para serem usados pelo próximo middleware. Veja outro exemplo de middleware que checa por migrações de banco de dados pendentes:

```ruby
module ActiveRecord
  class Migration
    class CheckPending
      def initialize(app)
        @app = app
      end

      def call(env)
        ActiveRecord::Base.logger.silence do
          ActiveRecord:: Migration.check_pending!
        end
        @app.call(env)
      end
    end
  end
end
```

Não precisamos fazer isso porque o Rails já faz por nós, mas é um bom exemplo.

Para a nossa app Rails usar o nosso middleware devemos definir isso no arquivo de configuração da aplicação em `config/application.rb`:

```ruby
module MeuSite
  class Application < Rails::Application
    # ...
    config.middleware.use = HelloHeader
  end
end
```

Nós também conseguimos definir a ordem em que os nossos middlewares serão executamos, remover middlewares e trocar um pelo outro assim:

```ruby
module MeuSite
  class Application < Rails::Application
    # ...
    config.middleware.use = HelloHeader

    # insere o NovoMiddleware DEPOIS do MiddlewareExistente
    config.middleware.insert_after(MiddlewareExistente, NovoMiddleware)

    # insere o NovoMiddleware ANTES do MiddlewareExistente
    config.middleware.insert_before(MiddlewareExistente, NovoMiddleware)

    # remove o MiddlewareExistente da lista de middlewares para serem executados
    config.middleware.delete(MiddlewareExistente)

    # troca o MiddlewareExistente pelo NovoMiddleware mantendo sua posição na lista
    config.middleware.swap(MiddlewareExistente, NovoMiddleware)
  end
end
```

Para saber quais middlewares a sua app Rails use basta rodar esse comando:

```bash
$ rake middleware
```

Você verá um retorno assim:

```bash
use Rack::Sendfile
use ActionDispatch::Static
use ActionDispatch::Executor
use ActiveSupport::Cache::Strategy::LocalCache::Middleware
use Rack::Runtime
use Rack::MethodOverride
use ActionDispatch::RequestId
use ActionDispatch::RemoteIp
use Sprockets::Rails::QuietAssets
use Rails::Rack::Logger
use ActionDispatch::ShowExceptions
use WebConsole::Middleware
use ActionDispatch::DebugExceptions
use ActionDispatch::Reloader
use ActionDispatch::Callbacks
use ActiveRecord::Migration::CheckPending
use ActionDispatch::Cookies
use ActionDispatch::Session::CookieStore
use ActionDispatch::Flash
use Rack::Head
use Rack::ConditionalGet
use Rack::ETag
run MyApp.application.routes
```


Para saber mais sobre middlewares e Rails veja o [Rails on Rack](http://guides.rubyonrails.org/rails_on_rack.html).

Depois que todos os middlewares foram executados, nós finalmente chegamos no arquivo `config/routes.rb`, veja um exemplo dele:

```ruby
Rails.application.routes.draw do
  scope :admin do
    resources :pages do
      resources :sections
    end
    resources :posts
    resources :categories
    resource :settings

    root 'admin#index', as: 'admin'
  end

  root 'blog#index'

  get '/:slug' => 'blog#page'
  get '/p/:slug' => 'blog#show'
  get '/c/:slug' => 'blog#category'
end
```

Veremos sobre esse arquivo no próximo post sobre como funcionam as rotas no Rails.
