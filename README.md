# laracasts_Learn_Vue_2

## Basic Databinding

Após incluir o script do VUE em seu codigo, pode se utilizar o data-binding.

Para uma instancia de VUE é necessário sempre atribuir um no atributo 'el' a superficie em que o Vue irá trabalhar. (Uma div por exemplo).
No atributo data, incluímos atribotos ou objeto que serão os models do vue (vue-models). Esse elementos podem ser associados via data-binding atraves da diretiva v-model a ser incluida em elementos HTML.

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


