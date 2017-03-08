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

## Attribute and Class Binding

Para mapear atributos basta usar a diretiva 'v-bind' seguido de qual atributo vc deseja alterar. Exemplo 'v-bind:title' sendo que esse formato pode ser substituido para :title=, que o valor atribuido está no vue model.

Ex: 
```html
<button v-bind:title="title">Button</button>
```

```js
	var app = new Vue({
		el:'#root',
		data: {
			 title: 'Title via Javascript'
		}
	})
```

É possivel condicionar valores em atributos, tendo como exemplos classes que podem ser atribuidas dependendo de uma ação.
Podemos utilizar por exemplo: :class= "{ 'is-loading':isLoading }" sendo isLoading atributo do Vue que se pode alterar via um metodo do Vue. Que podemos chamar via um evento como @click.


```html
<button :class= "{ 'is-loading':isLoading }" @click="toggleClass">Button</button>
```

```js
var app = new Vue({
	el:'#root',
	data: {
		 isLoading: false
	},
	methods: {
		toggleClass(){
			this.isLoading = true;
		}
	}
})
```

## The Need for Computed Properties

No objeto do vue, podemos inserir um atributo computed:, todos atributos que inserirmos la, serão enxergados como atributos de vue model. Porem eles serão atributos computados a partir de atributos ja inseridos no vue model.

Neste exemplo, o array tasks é utilizado como origem para se criar o atributo computado incompleteTasks().

```js
var app = new Vue({
	el:'#root',
	data: {
		 tasks: [
		 	{ description: 'Go to school', completed: true },
		 	{ description: 'Try to be succeded', completed: false },
		 	{ description: 'Don\'t follow leaders ' , completed: true },
		 	{ description: 'Watch the parking meters', completed: false },
		 	{ description: 'Look out', completed: false },
		 	{ description: 'Stay away!', completed: true },
		 	{ description: 'Get sick', completed: true },
		 	{ description: 'Get jail', completed: false }
		 ]
	},
	computed: {
		incompleteTask(){
			return this.tasks.filter( (task) => task.completed )
		}
	}
})
```

Assim ele pode ser utilizado como nesse exemplo

```html
<h1>Incompleted Tasks</h1>
<ul>
	<li v-for="task in incompleteTask" v-text="task.description"></li>
</ul>
```

## Components 101

Para criar componentes, deve ser definir um Vue.component()

Em que no atributo template define o html do componente quando ele for instanciado.

```js
	Vue.component('task',{
		template: '<li><slot></slot></li>'
	});
```

```html
	<ul>
		<task> And all God's angels beware </task>
		<task> And all you judges beware </task>
		<task> Sons of chance, take good care </task>
		<task> For all the people not there </task>
		<task> I'm not afraid anymore </task>

	</ul>
```


## Components Within Components

Ao se definir um template de um componente, não se pode retornar multiplos elementos, sendo assim, pode-se colocar uma div para agrupar multiplas tags.

Pode se incluir componentes dentro de componentes. Usando a tag criada no template de um componente quando se esta criando o template de outro componente.
Ex:

```js
	Vue.component('task-list',{
		template: ` <div>
						<task v-for="task in tasks" v-text="task.description"> </task>
					</div>
		`
	,
	data() {
		return{
			tasks: [
			 	{ description: 'Go to school', completed: true },
			 	{ description: 'Try to be succeded', completed: false },
			 	{ description: 'Don\'t follow leaders ' , completed: true },
			 	{ description: 'Watch the parking meters', completed: false },
			 	{ description: 'Look out', completed: false },
			 	{ description: 'Stay away!', completed: true },
			 	{ description: 'Get sick', completed: true },
			 	{ description: 'Get jail', completed: false }
			 ]
		};
	}

	})

	Vue.component('task',{
		template: '<li><slot></slot></li>'
	});
```
