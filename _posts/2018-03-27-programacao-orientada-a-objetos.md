---
layout: post
title: Programação orientada a objetos
tags: [ruby, POO]
---

A programação orientada a objetos (POO) é um paradigma de programação muito útil para organizar, reutilizar e manter código e tem o foco na manipulação de informações através de objetos.

Para entender esse paradigma precisamos entender o que são objetos, classes e instâncias.

## Classe

A classe é um lugar onde a gente pode definir comportamentos que serão usados para criar objetos. Esses objetos são muitas vezes representados por objetos da vida real. Por exemplo, podemos ter uma classe que vai ser usada para representar `vídeo games`.

## Objeto

O objeto é o produto da classe e que herda os seus comportamentos. Se na nossa classe `vídeo game` nós definirmos um comportamento que pode ser `jogar` determinado jogo, todos os objetos criados a partir dessa classe poderão herdar esse comportamento de `jogar`. Por exemplo, um objeto da classe `vídeo game` poderia ser um Play Station 4 ou um Xbox One.

## Instância

A instância é um ponteiro para o nosso objeto, é como um controle remoto que está nas suas mãos para você interagir com o objeto. Por exemplo, esse seria o nosso controle do PS4.

## Código

```ruby
# essa é a nossa classe
class VideoGame

  # a nossa classe implementa um comportamento chamado play
  def play
    print 'Playing Horizon Zero Dawn...'
  end
end

# ao fazermos VideoGame.new estamos criando um objeto a partir da classe acima
# e estamos mantendo um controle para interagir com o objeto
# veja que podemos criar muito objetos a partir da mesma class

ps4 = VideoGame.new
xbox = VideoGame.new

# ps4 e xbox são os controles (instâncias) para interagirmos com o objeto criado
ps4.play
xbox.play
```

Uma vantagem de usar esse paradigma é que você só precisa implementar certos comportamentos apenas uma vez e pode usá-los em vários objetos, como o comportamento de jogar que é comum entre ambos os vídeo games.

## Herança

Assim como funciona na genética, se uma classe mãe tem um comportamento, uma classe filha vai ter também e isso se chama herança.

Voltando ao nosso exemplo, podemos ter uma classe `eletrônico` que pode ter um comportamento de ligar-se e a classe `vídeo game` pode herdar esse comportamento e todos os objetos criados a partir dessa classe também herdarão esse comportamento.

```ruby
class Electronic
  def turn_on
    print 'Turning on...'
  end
end

class VideoGame < Electronic
  def play
    print 'Playing Horizon Zero Dawn...'
  end
end

ps4 = VideoGame.new
xbox = VideoGame.new

# ambos os objetos herdaram o comportamento da class Electronic
ps4.turn_on
xbox.turn_on

# assim como o comportamento da class Electronic
ps4.play
xbox.play
```

Dessa forma também podemos usar a classe `eletrônico` em outras classes de aparelhos como por exemplo um `notebook`.


```ruby
class Electronic
  def turn_on
    print 'Turning on...'
  end
end

class VideoGame < Electronic
  def play
    print 'Playing Horizon Zero Dawn...'
  end
end

class Notebook < Electronic
end

ps4 = VideoGame.new
xbox = VideoGame.new
my_notebook = Notebook.new

ps4.turn_on
xbox.turn_on
my_notebook.turn_on
```

Existem muitos outros conceitos em cima da programação orientada a objetos, mas isso é o principal para ter uma noção sobre o tema, então lembre-se a classe é o vídeo game, o objeto é o PS4 e/ou Xbox One e a instância é o controle.

Qualquer dúvida pode comentar abaixo que eu respondo.
