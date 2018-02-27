---
layout: post
title: Como importar dados de uma planilha com Ruby
tags: [csv]
thumbnail: "assets/img/posts/planilha.png"
feature-img: "assets/img/posts/planilha.png"
author: lucasprag
---

Vamos dizer que você tem um modelo chamado `Customer` e você precisa ler uma planilha `CSV` e salvar esses clientes no seu banco de dados. O Ruby tem uma [classe](http://ruby-doc.org/stdlib-2.0.0/libdoc/csv/rdoc/CSV.html) que nos ajuda nesse trabalho e torna ele muito simples, veja só esse exemplo:

Considere essa planilha csv:

<img src="/assets/img/posts/planilha-para-importar-customers.png" alt="planilha com headers email, name e age" width="100%">

```ruby

require 'csv'

file = File.new('example.csv', 'r')
csv = CSV.parse(file, { col_sep: ';', headers: true })

csv.each do |row|
  customer = Customer.find_or_initialize_by(email: row['email'])
  customer.name = row['name']
  customer.age  = row['age']
  customer.save
end

```

Veja que nesse script nós estamos seguindo esses passos;
- lendo o arquivo CSV que vamos importar
- Parseando o arquivo com a class `CSV`
- iterando em casa linha da planilha
- buscando na base de dados pelo email, se não houver então inicializando um objeto `Customer`
- atribuindo o nome e a idade ao objeto customer
- salvando no banco de dados

Também é possível usar uma forma mais curta para ler o arquivo assim:

```ruby

require 'csv'

CSV.foreach('example.csv', { col_sep: ';', headers: true }) do |row|
  customer = Customer.find_or_initialize_by(email: row['email'])
  customer.name = row['name']
  customer.age  = row['age']
  customer.save
end

```

Você também pode salvar a lista das linhas da sua planilha que são inválidas assim:

```ruby

require 'csv'

invalid_customers = []

CSV.foreach('example.csv', { col_sep: ';', headers: true }) do |row|
  customer = Customer.find_or_initialize_by(email: row['email'])
  customer.name = row['name']
  customer.age  = row['age']

  if customer.valid?
    customer.save
  else
    row << customer.errors
    invalid_customers << row
  end
end

puts invalid_customers

```

Se você tiver qualquer dúvida sobre como importar dados de planilhas você pode criar uma issue [aqui](https://github.com/rubynostrilhos/forum) que eu vou te ajudar assim que possível.
