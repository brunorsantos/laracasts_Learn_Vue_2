# laracasts_Learn_Vue_2

## Basic Databinding

Após incluir o script do VUE em seu codigo, pode se utilizar o data-binding.

Para uma instancia de VUE é necessário sempre atribuir um no atributo 'el' a superficie em que o Vue irá trabalhar. (Uma div por exemplo). Só nesse escopo podemos mapear tudo do Vue
No atributo data, incluímos atributos ou objeto que serão os models do vue (vue-models). Esse elementos podem ser associados via data-binding atraves da diretiva v-model a ser incluida em elementos HTML.

```html
<!DOCTYPE html>
<html>
<head>
	<title></title>
</head>
<body>
<div id="root">
	<input type="text" id="input" v-model="message">
	<p>Value is: {{message}}</p>

</div>
<script src="https://unpkg.com/vue@2.2.1"></script>
<script type="text/javascript">

	new Vue({
		el:'#root',
		data: {
			message : 'Hello World!'
		}
	})
</script>
</body>
</html>
```


## Lists

Diretiva v-for faz um loop em uma lista de um elemento do vue:

```html
<ul>
	<li v-for="name in names" v-text=name></li>
</ul>
```

Em que name passa a ser um elemento a ser mapeado pelo vue. 
E a diretiva v-text usa a variavel do vue como o texto do elemento html.

Quando criamos a instancia do vue, a podemos utilizar o metodo 'mounted ()' que é o trecho que sera disparado pelo Vue quando a instancia for montada. (Quando tudo estiver pronto).

```js
	var app = new Vue({
		el:'#root',
		data: {
			 names : ['Peter', 'Paul', 'Mary']
		},
		mounted(){
			document.querySelector('#button').addEventListener('click', () =>{
				let name = document.querySelector('#input');

				app.names.push(name.value);

				name.value = ''
			});
		}
	})
```

## Event Listener

Para utilizar eventos podemos utilizar a clausula 'v-on', inserindo :[nome do evento] após:

- v-on:click
- v-on:key-up

Considerando os eventos padrões do javascript. pode ser tambem abreviar para:
- @click
- @key-up

No valor da diretiva do evento, podemos fazer atribuicoes ou utilizar metodos do vue. Que são definidos no objeto do vue em methods().

Em qualquer metodo do vue, para acessar um valor do data(Vue model) basta utilizar o this. Lembrando do binding automatico em todos os lugares que estão utilizando o atributo.

```html
<div id="root">
<ul>
	<li v-for="name in names" v-text=name></li>
</ul>

<input v-model="newName">
<button v-on:click="addName">Add Name</button>
</div>
```

```js
	var app = new Vue({
		el:'#root',
		data: {
			 newName: '',
			 names : ['Peter', 'Paul', 'Mary']
		},
		methods:{
			addName(){
				this.names.push(this.newName);
				this.newName = '';
			}
		}
	})
```
