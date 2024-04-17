<script setup>
import {ref, computed, onMounted} from 'vue'




</script>

<template>


<form id="teste">


<h1 class="titulo"> 
	<h5 style="
	font-size: 80px;
	font-weight: bold; 
	color: rgb(238, 222, 3);
	padding: 1rem 1rem;
	text-shadow: 0px 1px 1px rgb(116, 116, 116);
	">
	olá,</h5>
	Seja bem-vindo, queremos saber sua opnião!</h1>

    <h4>Olá aluno de Sistemas de Informação, sua opinião é muito importante para para que possamos melhorar nossos serviços, click abaixo para participar!</h4>
	<img src="@/img/app-avaliacao.png" alt="foto-inicio" srcset="">
			
</form>
<div class="router">
  
<button 
	type="button" 
	class="btn-router" 
	@click="$router.push('Perguntas')"
	>Entrar
</button>

<!--<button @click="$emit('enviarDados', {nome: input_name, email: input_email})">evento</button>-->
 
</div>
<RouterView/>

  <main>
 
  </main>
</template>
<style scoped>
img {
	display: flex;
	width: 200px;
	height: 140px;
	margin: 0 auto;
}
h4 {
	color: rgb(124, 124, 124);
	font-size: 0.822rem;
	font-weight: 500;
	margin-bottom: 0.5rem;
    margin: 1.225rem;
	text-align: center;
}
.titulo {
	display: block;
	text-align: center;	
	color: rgb(67, 83, 110);
	margin-bottom: 20px;
  	font-weight: bold;
	font-size: 1.8rem;
	font-weight: 600;
 	margin: 0 auto;
  	width: 50% auto;
  	padding: 30px 30px;

}

</style>
