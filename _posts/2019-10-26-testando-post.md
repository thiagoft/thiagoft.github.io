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
