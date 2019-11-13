---
layout: post
title:  "Escrevendo conteúdo com Markdown"
date:   2019-10-28 23:33:41 -0300
categories: Markdown
---
Como primeiro conteúdo, nada melhor que escrever sobre como criar o próprio, então, vamos criar algumas notas aqui:

- Markdown foi criado para facilitar a forma de escrever conteúdo para web, porém como tudo por aqui é HTML, então nada mais justo que este conteúdo escrito de forma fácil e legível também ser convertido facilmente em HTML através de um processador.

## Sintaxe

### Destacando conteúdo

##### Negrito

Para marcar o texto com **negrito** basta digitar o conteúdo entre quatro asteristicos assim: `**negrito**` ou entre quatro underlines assim: `__negrito__`.

##### Itálico

Para *itálico* basta digitar o conteúdo entre asterísticos `*itálico*` ou entre underlines `_itálico_`.

### Título

Para trabalhar com títulos temos a seguinte código:
```html
# Título <h1>
## Título <h2>
### Título <h3>
#### Título <h4>
##### Título <h5>
###### Título <h6>
```
Que produz especificamente este resultado
# Título
## Título
### Título
#### Título
##### Título
###### Título

### Lista de itens

Para lista de itens e sub-item não ordenados:

```html
* item
* item
* item
  * item
    * item
```

Resultado:

* item
* item 
* item
  * item
    * item

Para lista de itens ordenados:

```html
1. item
2. item
3. item
```

Resultado:

1. item
2. item 
3. item

### Links

Para criar links devemos escrever o conteudo entre couchetes, e em seguida entre parênteses a referência.

```markdown
[isto é um link](https://thiagoft.github.io/testando-post)
```

Resultado: 

[isto é um link](https://thiagoft.github.io/testando-post)

### Código no texto

Para referenciar um código no meio do texto podemos digitar:

```markdown
`conteudo`
``` 
ou
```markdown
~conteudo~
``` 

ou criar uma caixa de codigo para um conteúdo de mais de uma linha.

~~~markdown
```markdown
codigo
```
~~~

ou

```markdown
~~~markdown
codigo
~~~
```

*Um detalhe é que no caso da caixa de código você pode especificar a linguagem do conteúdo substituindo **markdown** pela linguagem que você quiser.*

### Imagens

Para adicionar uma imagem:

```markdown
![conteúdo do highlight da imagem](link da imagem)
```
Resultado:
![conteúdo do highlight da imagem](https://github.blog/wp-content/uploads/2013/04/074d0b06-a5e3-11e2-8b7f-9f09eb2ddfae.jpg?resize=1234%2C701)

### Citações

Para criar uma citação:
```markdown
> texto da citação
```

Resultado:

> Testando uma citação.

### Tabelas

Para criar uma tabela:

```markdown
| Cabeçalho 1 | Cabeçalho 2 | Cabeçalho 3 |
| --- | --- | --- |
| Conteúdo 1 | Conteúdo 2 | Conteúdo 3 |
| Conteúdo 4 | Conteúdo 5 | Conteúdo 6 |
```

Resultado:

| Cabeçalho 1 | Cabeçalho 2 | Cabeçalho 3 |
| --- | --- | --- |
| Conteúdo 1 | Conteúdo 2 | Conteúdo 3 |
| Conteúdo 4 | Conteúdo 5 | Conteúdo 6 |

### Conclusão

Então é isso, o markdown simplifica muito as coisas para criação de conteúdo em páginas estáticas. Agora você pode criar seu "README" em seu repositório no [github](https://www.github.com) e escrever seu blog em processadores de páginas estáticas.