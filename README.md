# laracasts_Learn_Vue_2

[Ajuda em instanciar vue e vue component](https://uploads.disquscdn.com/images/d3f0fa038c672bade62f30026fee8e83c41aa447ec2290127fbb63447198cadc.png)

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
Assim no Html da pagina utlizamos a tag com o nome do componente criado.
Em Slot passamos o conteudo interno da Tag declarada no HTML

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
## Practical Component Exercise #1: Message

Para passar parametros para o componete, basta passar como atributos do elemento HTML.

```html
<message title="Hello Wold" body="Lorem ipsum dolor sit amet" > </message>
<message title="Hello Wold111" body="sadsadadasdsadsadasd" > </message>
``` 

Sendo que na declaracao do compoente Vue, eles devem ser explicitamente declarados como props

```js
Vue.component('message', {
   	props: ['title', 'body'],
   	template: `
		<article class="message" v-show="isVisible">
		  <div class="message-header">
		    <p>{{title}}</p>
		    <button class="delete" @click='hideModal'></button>
		  </div>
		  <div class="message-body">
		    {{body}}
		  </div>
		</article>
   	`,

   	methods: {
   		hideModal(){
   			this.isVisible = false;
   		}
   	},
   	data() {
   		return{
   			isVisible : true
   		};
   	}
}); 
```

## Practical Component Exercise #2: Modal

Quando se instancia normalmente um objeto da classe do Vue, esta se trata da uma instancia do root, em que ele pode enxergar apenas o escopo criado por ele.
Sendo assim, uma instancia de um evento, no pode enxergar nem modificar nada no model dessa instancia. 

Para auxiliar, um evento pode emitir eventos, que podemos utilizar fora e alterar alguma propriedade do vue model da instancia do root. Ex: $emit('close') - chamaria um evento 'close'.

Componente cridado com:

```js
	Vue.component('modal',{
		template: `
			<div class="modal is-active">
			  <div class="modal-background"></div>
			  <div class="modal-content">
			  <div class="box">
			  <slot> </slot>
			  </div>
			  </div>
			  <button class="modal-close" @click="$emit('close')"></button>
			</div>
	});
```

Chamada no Html do componente, sendo isActive uma propriedade do vue-model e @close o event-listener no evento close criado no componente:

```html
	<modal v-show="isActive" @close="isActive = false"> texto do modal </modal>
```

## Practical Component Exercise #3: Tabs

Dentro do um compónente é possivel exergar os objetos referentes aos componentes filhos usando this.$children, em que é possivel retornar atributos do model do componente por exemplo. Assim, podemos montar um atribuito no model que se refere ao componente criado como filho

Ex:

```js
created() {
	this.tabs = this.$children;
},
```

É possivel adicionar propriedades nas prorpiedades de um componente criado.

```js
	props: {
		name: { required: true}
		selected: {default: false}
	},
```

É possivel passar um argumento na chamada de um metodo no HTML. Caso estivermos chamando um metodo atraves de um evento, podemos passas o proprio objeto evento como em: @click="selectTab($evento)" ou entao passar algo do escopo como no v-for

```html
<li v-for="tab in tabs" :class="{ 'is_active' : }">
	<a href="#" @click="selectTab(tab)">{{tab.name}} </a>
 </li>
```

Nunca se deve alterar atributos passados em 'props', caso seja necessário, deve se utilizar outra variavel a ser criada na model em data como no exemplo:

```js
Vue.component('tab',{
	props: {
		name: { required: true}
		selected: {default: false}
	},
	template:`
		<div> <slot></slot> </div>
	` ,
	data(){
		return {
			isActive: false
		}
	},
	mounted () {
		this.isActive = this.selected;
	}
});
```

## Component Communication Example #1: Custom Events

Comunicao entre pai e filho se tratando de componente, pode ser feito atraves do $emit() ou this.$emit caso seja chamado em um método, em que passamos um evnto a ser disparado em quem fez a delaracao do componente

```js
Vue.component('cupon',{
	template:`
		<input placeholder="Enter your cupon" @blur=apllyCupon> </input>
	` ,
	methods :{
		apllyCupon() {
			this.$emit('applied');
		}
	}
})
```


## Component Communication Example #2: Event Dispatcher

Um forma de compartilhar a comunicacao entre os componentes, é criar um objeto do Vue global como em:

```js
window.Event = new Vue();
```

Em que todos os componentes terão acesso, e poderão chamar disparar e escutar eventos nesse objeto:

Disprando um evento no objeto global
```js
Event.$emit('teste',{conteudo: 'teste111'})
```

Ao instanciar uma classe do VUE, podemos ficar escutando por um evento no objeto global:

```js
created(){
	Event.$on('teste', (cont) => alert(cont.conteudo) );
}
```

## Named Slots in a Nutshell

Os slots que passamos para um componente podem ser nomeados afim de diferenciarmos e usarmos em vários trechos.

Para isto no template do componente utilizamos o atributo name da tag slot 

```html
<slot name="nome"> Conteudo padrao </slot>
```
Em que pode se opcionalmente utilizar um conteudo padrao 

Para passar o conteudo no html usa-se:
```html
 <template slot="header"> texto do header </template>
```

No lugar da tag template pode se passar outro elemente como div, p, h1. Assim será rederizado o compomente com a tag que se passar

## Single-Use Components and Inline Templates

Não é uma pratica comum, mas pode se usar componentes em que todo seu conteudo é passado na declaracao no html. Isto é não criar um template para ele:

```html
<progess-view inline-template> 
<div>
	taxa de completo: {{completo}}
	<p><button @click="completo = completo + 10">Aumentar</button></p>	
</div>
 </progess-view>
```

Nesse caso deve se passar 'inline-template' na chamada do componente.

```js
	Vue.component('progess-view',{
		data() {
			return {
				completo: 50
			};
		}	
	})	
```
Nota se que o componente nao possui o atributo 'template'.
