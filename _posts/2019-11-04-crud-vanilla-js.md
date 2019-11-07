---
layout: post
title:  "Criando um CRUD com Javascript Vanilla"
date:   2019-11-04 21:45:55 -0300
categories: Javascript
---
Vamos lá... primeiramente, o que é "Vanilla"? Simples... Baunilha em Inglês. Mas o que isso tem a ver com Javascript? Honestamente não sei, mas depois de algumas pesquisas, não encontrei significado algum do porque "Vanilla", porém, nada mais é do que o Javascript puro, ou seja, livre de frameworks e bibliotecas.

E quanto a "CRUD"? Bom, essa é fácil, é um acrônimo para **C**reate, **R**ead, **U**pdate, **Delete**, ou seja, uma interface ou serviço que realizará operações de Criação, Leitura, Atualização e Deleção.

Então vamos lá, mãos a obra.

Primeiramente criamos o arquivo html com o nome index. Este é o arquivo principal de nosso CRUD e é ele que é chamado quando digitamos o endereço virtual de nosso site.

> na estrutura de pasta temos então:
> * vanilla-js-crud
>   * index.html

Código:
```html
<!DOCTYPE html>
<html lang="pt">

<head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">

    <title>Products</title>
</head>

<body>
    <div id="container" class="container"></div>

    <script src="js/app/helpers/ArrayUtils.js"></script>
    <script src="js/app/helpers/DateUtils.js"></script>
    <script src="js/app/models/Product.js"></script>
    <script src="js/app/models/ProductList.js"></script>
    <script src="js/app/view/ProductView.js"></script>
    <script src="js/app/controllers/ProductController.js"></script>

    <script>
        let productController = new ProductController();
    </script>
</body>

</html>
```

Agora vamos dar vida ao html com o código JavaScript.

Primeiramente vamos escrever o arquivo que representará nosso Produto:

> na estrutura de pasta temos então:
> * vanilla-js-crud
>   * index.html
>   * js
>       * app
>           * models
>               * Product.js

Código:

```javascript
class Product {

    constructor(id, description, price, quantity,tax) {
        this._id = id;
        this._description = description;
        this._price = price;
        this._quantity = quantity;
        this._tax = tax;
        this._total = this.calculateTotal();
    }

    get id() {
        return this._id;
    }

    get description() {
        return this._description;
    }

    get price() {
        return this._price;
    }

    get quantity() {
        return this._quantity;
    }

    get tax() {
        return this._tax;
    }

    get total() {
        return this._total;
    }

    calculateTotal() {
        return (this._price*this._quantity)*((this._tax/100)+1);
    }
}
```

E então o arquivo que representará nossa Lista de Produtos.

> na estrutura de pasta temos então:
> * vanilla-js-crud
>   * index.html
>   * js
>       * app
>           * models
>               * Product.js
>               * ProductList.js

Código:

```javascript
class ProductList {
    constructor() {
        this._products = [];
    }

    add(product) {
        this._products.push(product);
    }

    get products() {
        //Hack to makes _products imutable;
        // return [].concat(this._products);
        return this._products;
    }

    set products(products) {
        this._products = products;
    }

    findById(id) {
        let productToReturn;

        this._products.forEach(product => {
            if (product.id === id) {
                productToReturn = product;
            }
        });

        return productToReturn;
    }
}
```

Agora a interface que será adicionada por código javascript em nosso index.html contendo o formulário e botões e interações.

> na estrutura de pasta temos então:
> * vanilla-js-crud
>   * index.html
>   * js
>       * app
>           * models
>               * Product.js
>               * ProductList.js
>           * view
>               * ProductView.js

Código:

```javascript
class ProductView {

    constructor(container) {
        this._container = container;
    }

    update(productList) {
        this._container.innerHTML = this.template(productList);
    }

    template(productList) {
        return `<div>
                    <h2>Products</h2>
                </div>
                <div>
                    <form id="productForm" onsubmit="productController.save(event)">
                        <label for="idInput">Code</label>
                        <input type="number" name="idInput" placeholder="Item code">

                        <label for="descriptionInput">Description</label>
                        <input type="text" name="descriptionInput" placeholder="Item description">

                        <label for="priceInput">Price</label>
                        <input type="number" name="priceInput" placeholder="Item Price">

                        <label for="quantityInput">Quantity</label>
                        <input type="number" name="quantityInput" placeholder="Quantity">

                        <label for="taxInput">Tax</label>
                        <input type="number" name="taxInput" placeholder="Tax">

                        <button id="btnSave">Save</button>
                    </form>
                </div>

                <div>
                    <hr/>
                    <table id="productTable" class="table table-striped" >
                        <thead>
                            <tr>
                                <th scope="col">Code</th>
                                <th scope="col">Description</th>
                                <th scope="col">Quantity</th>
                                <th scope="col">Price</th>
                                <th scope="col">Tax</th>
                                <th scope="col">Montant</th>
                                <th scope="col">Edit</th>
                                <th scope="col">Remove</th>
                            </tr>
                        </thead>
                        <tbody>
                        ${
                            productList.products.map(product => `
                                <tr>
                                    <td id="productId" >${product.id}</td>
                                    <td id="productDescription" >${product.description}</td>
                                    <td id="price" >${product.price}</td>
                                    <td id="quantity" >${product.quantity}</td>
                                    <td id="tax" >${product.tax}</td>
                                    <td>${product.total}</td>
                                    <td><img src="/img/edit.svg" width="16px" height="16px" onclick="productController.edit(event)" /></td>
                                    <td><img src="/img/remove.svg" width="16px" height="16px" onclick="productController.remove(event)" /></td>
                                </tr>
                            `).join('')
                        }
                        </tbody>
                        <tfoot>
                        </tfoot>
                    </table>
                </div>`
    }
}
```

E finalmente o arquivo que será o controlador de toda interação entre nossa interface html e as funcionalidades escritas em javascripts.

> na estrutura de pasta temos então:
> * vanilla-js-crud
>   * index.html
>   * js
>       * app
>           * models
>               * Product.js
>               * ProductList.js
>           * view
>               * ProductView.js
>           * controllers
>               * ProductController.js

Código:

```javascript
class ProductController {

    constructor() {
       //setting querySelector function to $
       let $ = document.querySelector.bind(document);
       let container = $("#container");

       this._productView = new ProductView(container);
       this._productList = new ProductList();
       this._productView.update(this._productList);
       
    }

    save(event) {
        event.preventDefault();
        let editedProduct = this._createProduct();
        let productOld = this._productList.findById(editedProduct.id);

        if (productOld != null) {
            let index = this._findProductById(productOld.id);
            this._productList.products = ArrayUtils.removeIndex(this._productList.products,index);
        }

        this._productList.add(editedProduct);
        this._productView.update(this._productList);
    }

    _findProductById(productId) {
        let indexToRemove;
        this._productList.products.forEach((element, index) => {
            if (element.id === productId) {
                indexToRemove = index;
            }
        });

        return indexToRemove;
    }

    edit(event) {
        console.log(event);
        let form = document.querySelector("#productForm");
        let row = event.target.parentNode.parentNode;

        form.idInput.value = row.querySelector("#productId").textContent;
        form.descriptionInput.value = row.querySelector("#productDescription").textContent;
        form.priceInput.value = row.querySelector("#price").textContent;
        form.quantityInput.value = row.querySelector("#quantity").textContent;
        form.taxInput.value = row.querySelector("#tax").textContent;
    }

    remove(event) {
        let row = event.target.parentNode.parentNode;
        let productId = row.querySelector("#productId").textContent;
        let indexToRemove;

        this._productList.products.forEach((element, index) => {
            if (element.id === productId) {
                indexToRemove = index;
            }
        });

        event.target.parentNode.parentNode.remove();
        this._productList.products = ArrayUtils.removeIndex(this._productList.products, indexToRemove);
    }

    _createProduct() {
        let form = document.querySelector("#productForm");
        let product = new Product(form.idInput.value,
        form.descriptionInput.value,
        form.priceInput.value,
        form.quantityInput.value,
        form.taxInput.value);

        form.reset();

        return product;
    }
}
```

Foram utilizados 2 arquivos a mais com a funcionalidade de Formatar Datas e outro de Controlar conjuntos de itens.

> na estrutura de pasta temos então:
> * vanilla-js-crud
>   * index.html
>   * js
>       * app
>           * models
>               * Product.js
>               * ProductList.js
>           * view
>               * ProductView.js
>           * controllers
>               * ProductController.js
>           * helpers
>               * ArrayUtils.js
>               * DateUtils.js

Código ArrayUtils.js:

```javascript
class ArrayUtils {

    constructor() {
        throw new Error("ArrayUtils cannot be instantiated.");
    }

    static removeIndex(array, index) {
        if (ArrayUtils.isArrayValid(array)) {
            let newArray = [];
            array.forEach((element,elementIndex) => {
                if (elementIndex != index) {
                    newArray.push(element);
                }
            });

            return newArray;
        } else {
            return array;
        }
        
    }

    static isArrayValid(array) {
        return (typeof array != "undefined"  
                && array != null  
                && array.length != null  
                && array.length > 0);
    }
}
```

Código DateUtils.js:

```javascript
class DateUtils {

    constructor() {
        throw new Error("DateUtils cannot be instantiated.");
    }

    static textToDate(textToConvert) {
        if (/\d{4}-\d{2}-\d{2}/.test(date)) {
            //the "index % 2" will return 0 for the first and third position of the date array and 1 
            //for the second and then it will be subtract from the item value. The result is the month -1
            //because the Date class counts months from the 0
            return new Date(...textToConvert.split('-').map((item, index) => item - index % 2));
        } else {
            throw new Error("The date format must be AAAA-MM-DD");
        }
    }

    static dateToText(date) {
        return `${date.getDate()}/${date.getMonth() + 1}/${date.getFullYear()}`;    }
}
```

Utilizamos também 2 imagens no formato svg para representar os botões de editar e remover.

> na estrutura de pasta temos então:
> * vanilla-js-crud
>   * index.html
>   * js
>       * app
>           * models
>               * Product.js
>               * ProductList.js
>           * view
>               * ProductView.js
>           * controllers
>               * ProductController.js
>           * helpers
>               * ArrayUtils.js
>               * DateUtils.js
>   * img
>       * edit.svg
>       * remove.svg