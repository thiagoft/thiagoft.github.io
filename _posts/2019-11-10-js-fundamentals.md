Este post é um guia prático abordando tópicos fundamentais da sintaxe atual do javascript.

## Const

Constantes como o próprio nome ja diz, matem um valor constante, ou seja, que não muda.

- O valor de uma constante não pode ser alterado, tentar faze-lo resultará em erro.

```javascript
const age = 31;
age = 29; //error: age is read-only
```

- Se alterar uma constante gera um erro, então não inicializa-la também gerará.

```javascript
const age; //error: Unexpected token
```

## let e var - Declarando variáveis

`let` - keyword para declaração de variáveis com escopo de bloco.

`var` - keyword para declaração de variáveis com escopo global.

Vejamos alguns exemplos:

- Para utilizar uma variável com escopo de bloco ela deve ser declarada antes do uso pois códigos Javascript são lidos de cima para baixo.

```javascript
console.log(age); //error:  age is not defined
let age = 31; 
```
- Uma variável com escopo global pode ser utilizada antes da declaração explicita. E isso é muito estranho dada a regra de `let`, porém sim, funciona.

```javascript
console.log(age); //imprime undefined
var age = 31; 
```

- Outro exemplo de declaração com var onde o uso da variável age é permitido fora do escopo de bloco de `if`.

```javascript
if (true) {
    var age = 31;
}
console.log(age); //imprime 31
```

- Já no exemplo abaixo `age` declarada com let, existirá somente dentro do escopo do bloco de `if`, gerando erro ao ser utilizada fora.

```javascript
if (true) {
    let age = 31;
}
console.log(age); //error: age is not defined
```

> Uma boa prática é sempre utilizar let a var, para evitar bagunça em seu escopo global.

## Rest Parameters

Rest parameters são utilizados para enviar multiplos parâmetros para uma função, sem que seja necessária a declaração explicita.

```javascript
function carModels(...carModels) {
    carModels.forEach(carModel => console.log(carModel)); // Uno Versa Fusca
}

carModels('Uno', 'Versa', 'Fusca');
```

Um outro exemplo é que outros parâmetros podem ser utilizados antes do rest parameter:

```javascript
function carModels(brand, ...carModels) {
    carModels.forEach(carModel => console.log(carModel)); // Uno Versa Fusca

    console.log(brand); // Fiat
}

carModels('Fiat', 'Uno', 'Toro', 'Linea');
```

> OBS: Não são permitidos parâmetros após a declaração de um rest parameter, dado seu funcionamento. 

## Destructuring Arrays

A desconstrução de arrays atribuirá os elementros de um array para as variáveis declaradas entre couchetes como no exemplo abaixo:

```javascript
let carModels = ['Uno', 'Versa', 'Fusca'];
let [model1, model2, model3] = carModels;

console.log(model1, model2, model3); // 'Uno', 'Versa', 'Fusca'
```

outro exemplo:

```javascript
let carModels = ['Uno', 'Versa', 'Fusca'];
let [model1, ...carModels] = carModels;

console.log(model1, carModels); // 'Uno', ['Versa', 'Fusca']
```

## Destructuring Objects

A regra para a desconstrução de objetos é a mesma que as arrays, porém, agora estamos falando de objetos:

```javascript
let car = { model: 'Uno', year: 2019 };
let { model, year } = car;

console.log(model, year); // Uno 2019
```

Neste próximo exemplo há uma exceção lançada quando tento desconstruir o objeto `car` para as variáveis `model` e `year`, isto porque o uso de `{}` é utilizado para blocos de código e neste caso existe a confusão se isto é um bloco ou uma desconstrução. Utilizando `()`,  existe agora a distinção e tudo funciona normalmente. 

```javascript
let car = { model: 'Uno', year: 2019 };
let model, year;

{ model, year } = car; // Unexpected token
({ model, year } = car); // OK

console.log(model, year); //// Uno 2019
```

## Spread Syntax

*Spread syntax* funciona da forma oposta a *rest parameter*, exemplo:

```javascript
function setCarModels(model1, model2, model3) {
    console.log(model1, model2, model3); // Uno Versa Gol
}

let carModels = ['Uno', 'Versa', 'Gol'];
setCarModels(...carModels);
```

outro exemplo:

```javascript
function setCarModels(model1, model2, ...carModels) {
    console.log(model1, model2, carModels); // Uno Versa [Gol]
}

let carModels = ['Uno', 'Versa', 'Gol'];
setCarModels(...carModels);
```

## typeof()

Imprime o tipo do parametro enviado.

```javascript
typeof(0); // number
typeof(false) // boolean
typeof('YeeeY') // string
typeof(function(){}) // function
typeof({}) // object
typeof(null) // object   - WHAT?!?!?!?!
typeof(undefined) // undefined
typeof(NaN) // number
```