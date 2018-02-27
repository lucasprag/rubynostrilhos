---
layout: post
title: "Curiosidade: Você sabe o motivo dos nomes action e active no rails?"
tags: [curiosidade]
thumbnail: "assets/img/posts/action-active-rails.png"
feature-img: "assets/img/posts/action-active-rails.png"
author: lucasprag
---

Se você já fez alguma coisa com rails você sabe que ele consiste em vários frameworks unidos. Esses frameworks são:

- Action Cable
- Action Mailer
- Action Pack
- Action View
- Active Job
- Active Model
- Active Record
- Active Storage
- Active support

Percebeu que existe uma distinção nos nomes desses frameworks? Alguns deles são **action** e outros são **active**.

## Action
Os frameworks usando **Action** no nome representam a camada do controller e da view que é responsável por lidar com as requisições HTTP/Socket e sua devida resposta.

## Active
Os frameworks usando **Active** no nome representam a camada do model que representa o accesso ao bando de dados e as regras de negócio da applicação.

Uma exceção é o Active Support que é um junção de classes e helpers que abstraem lógicas usadas na camada do model do rails.

É uma diferença bem simples e sutil que serve como uma curiosidade interessante do rails.
