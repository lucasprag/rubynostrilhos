---
layout: post
title: Aprenda a diferença entre proc, lambda e block 
tags: [iniciante, ruby]
thumbnail: "assets/img/posts/blocks-procs-lambdas.png"
feature-img: "assets/img/posts/blocks-procs-lambdas.png"
---

Nesse post você vai aprender tudo o que você precisa saber sobre blocks, procs e lambdas.

# Block

Pense em block (ou bloco) como uma função anônima que você define e executa logo em seguida. Se você já usou o `Array#each` você já usou block.

```ruby
# exemplo de um block

[1, 2, 3].each do |number|
  puts number
end

# também podemos definir blocks usando {} 

[1, 2, 3].each { |number|
  puts number
}
# 1
# 2
# 3

```

Veja que a variável entre `|` (pipes) é o parâmetro do bloco.

Vamos montar um método que recebe um bloco como argumento e para fazer isso precisamos usar o `yield`, veja o exemplo:

```ruby
def answer_me
  yield
end

answer_me do
  puts 42
end
# 42
```

Veja que o bloco será executado todas as vezes que chamarmos o `yield`:

```ruby
def answer_me_twice
  yield
  yield
end

answer_me_twice do
  puts 42
end
# 42
# 42
```

O bloco que passamos será executado pelo `yield` e nós também podemos passar parâmetros para o bloco através do `yield` assim:

```ruby
def answer_me
  yield 42
end

answer_me do |number|
  puts number
end
# 42
```

Mas isso não nos impede de também passarmos parâmetros para o método acima dessa forma:

```ruby
def answer_me(to_add)
  yield 40 + to_add
end

answer_me(2) do |number|
  puts number
end
# 42
```

Agora, por curiosidade, vamos implementar um método como o `Array#each`:

```ruby
def my_each(list)
  i = 0
  
  while i < list.size do
    yield list[i]

    i += 1
  end
end

my_each([1, 2, 3]) do |number|
  puts number
end
# 1
# 2
# 3
```

# Lambdas x Procs 

Lambdas e procs são bem similares, na verdade não existe uma classe dedicada para `Lambda`, ou seja, uma lambda é apenas um objeto `Proc` especial.

```ruby
# Na verdade se você olhar nos métodos de instância de um
# objeto Proc você encontrará um método `lambda?`

p = Proc.new {}
p.methods 
p.lambda?
# => false
```

Você pode definir uma lambda de duas formas:

```ruby
say_my_name = -> { puts "Heisenberg" }
say_my_name = lambda { puts "Heisenberg" }
```

Definindo uma lambda não executa o código dentro dela. Para executar o código de uma lambda a gente precisa usar o método `#call`.

```ruby
say_my_name = -> { puts "Heisenberg" }
say_my_name.call
# Heisenberg
```

A gente também consegue passar argumentos para lambdas assim:

```ruby
say_my_name = ->(name) { puts name }
say_my_name.call('Alex')
# Alex
```

Se uma lambda não recebe todos os argumentos necessários ela vai levantar um erro enquanto procs não ligam para argumentos.

```ruby
p = Proc.new { |a, b| puts "Eu não estou nem aí" }
p.call
# "Eu não estou nem aí"
```

Outra diferença importante é como ambas se comportam com o `return`.

```ruby
def call_proc
  puts "Antes da proc"
  my_proc = Proc.new { return 2 }
  my_proc.call
  puts "Depois da proc"
end
 
call_proc
# "Antes da proc"


def call_lambda
  puts "Antes da labmda"
  my_lambda = -> { return 2 }
  my_lambda.call
  puts "Depois da lambda"
end
 
call_lambda
# "Antes da lambda"
# "Depois da lambda"

```

Veja que o `return` da proc vai retornar no contexto to método enquanto que o `return` da lambda vai retornar no contexto dela mesma.

## Resumo
- Lambdas são definidas com `-> {}` e procs com `Proc.new {}`.
- Procs retornam do método atual, enquanto lambdas retornam from delas mesmas.
- Procs não ligam para os argumentos mesmo podendo usá-los, enquanto lambdas ligam e ainda levantam um erro.

Você conhece alguma outra particularidade sobre blocks, procs e lambdas e gostaria de compartilhar? Se tiver comente abaixo. =)
