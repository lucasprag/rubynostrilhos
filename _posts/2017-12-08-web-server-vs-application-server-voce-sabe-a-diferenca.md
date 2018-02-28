---
layout: post
title: Web Server vs Application Server - Você sabe a diferença?
tags: [devops, iniciante]
---

Qual o caminho que uma requisição faz para acessar a sua applicação Rails?

Você sabe a diferença entre um web server e um application server? Você sempre vai precisar dos dois?

# Web Server

Servidor web é um software que recebe requisições HTTP de clientes, como navegadores, e serve respostas HTTP, incluindo opcionalmente dados, que geralmente são páginas web, tais como documentos HTML com objetos embutidos (imagens, etc.) e documentos JSON.

No web server não existe código ruby, rack, nem `routes.rb`, aqui nós estamos falando de requisições HTTP transferindo documentos, imagens, etc.

Os web servers mais comuns são o [Apache](https://httpd.apache.org/) e [Nginx](https://www.nginx.com/). Para você servir um site estático (apenas HTML, CSS e/ou JS) um web server é o suficiente, veja um exemplo de configuração [aqui](https://www.nginx.com/resources/wiki/start/topics/examples/full/).

# Application Server

Servidor de aplicação é o software que vai rodar a sua aplicação Rails. O caminho da requisição é esse: a requisição chega no web server, ele processa ela e manda para o application server, que tem o seu código ruby carregado na memória para ser interpretado e então a requisição chega no Rails.

A conversa entre o application server e a sua aplicação Rails é feita atravéis do [Rack](/2017/12/07/como-rails-interpreta-as-rotas-rack.html) que possui uma série de middlewares para tratar a requisição para finalmente chegar no seu `config/routes.rb`.

Alguns application servers bem comuns são o [puma](http://puma.io/) e o [unicorn](https://bogomips.org/unicorn/).

# Preciso dos dois?

Para rodar código em produção sim, pois é no web server que ficam as instruções de como a sua requisição vai ser processada, por exemplo quando o `server name` for `rubynostrilhos.com.br` mandar a requisição para a aplicação em `localhost:3000` e quando o server name for `labs.rubynostrilhos.com.br` mandar a requisição para a aplicação em `localhost:4000`.

Mas quando você não precisa desse tipo de instrução você também não precisa do web server. Quando você roda o comando `rails server` para desenvolver na sua máquina você está rodando o application server e acessando ele diretamente através do endereço `localhost:3000`


Se você tiver qualquer dúvida você pode perguntar nos comentários abaixo.
