<script setup>
import {onMounted, ref, computed, watch} from 'vue'
import Messege from '../components/Messege.vue';
import { db } from '../firebase'
import { collection, getDocs, addDoc } from 'firebase/firestore'

let greeting = ref(true)
let isFinish = ref(false)
let isLast = ref(false)
const input_matricula = ref('')
const input_name = ref('')
let items = ref([])
let input_resposta = ref('')
let respostas = ref([])
let input_comentario = ref('')
let currentIndex = ref(0)
let msg = ref('')
const perguntas = ref([])



const nextQuestion = () =>{
     if (input_resposta.value === ('')) {
      return alert("❌ Selecione uma das opções!")
     }
     if(currentIndex.value === perguntas.value.length -1 ){
         isLast.value = true
     }
     respostas.value.push(
        input_resposta.value
    )
    if(currentIndex.value < perguntas.value.length -1 ){   
        currentIndex.value ++
        input_resposta.value = ('')      
        loadQuestion ()      
     }  
    }

const loadQuestion = () =>{
   if(!input_matricula.value.length){
      return alert("❌ É obrigatório informar o número de matrícula!")
   }
     greeting.value = false
     items.value = perguntas.value[currentIndex.value]
     }

const finish = ()=> {
   addDoc (collection(db, 'respostas'), {
         nome: input_name.value,
			matricula: input_matricula.value,
			respostas: respostas.value,
			comentario: input_comentario.value,
         data: new Date()
		})
   

   isFinish.value = true
   msg.value = 'Suas respostas foram enviadas! Obrigado por participar.'
		setTimeout(() => {
		msg.value = (''),
		input_comentario.value = ('')     
      },
      3000)

   }

onMounted(async() => {
   const querySnapshot = await getDocs(collection(db, 'perguntas'));
	const fireBasePerguntas = []
	querySnapshot.forEach((doc) => {
  	console.log(doc.id, " => ", doc.data());
	const perg = {
		id: doc.data().id,
		pergunta: doc.data().pergunta,
		opcoes: doc.data().opcoes
	}
	fireBasePerguntas.push(perg)
});
	perguntas.value = fireBasePerguntas
})
   


</script>

<template> 
<main class="app"> 
   <div v-if="greeting">
   <div class="titulo">Vamos começar?</div>
   <div class="bem-vindo">
   <span>Informe seu nome</span>
   <input 
      type="text" 
      name="nome" 
      id="nome" 
      placeholder="seu nome aqui..."
      v-model="input_name" />

   <span>Informe sua matrícula (Obrigatório)</span>
      <input 
      type="text"
      name="matricula"
      id="matricula"		
      placeholder="sua matrícula aqui..."
      v-model="input_matricula" />
  
   <div class="btn">
      <input 
      type="button" 
      value="Iniciar" 
      @click="loadQuestion()">
   </div>
   
</div>

</div>
</main>		

<main>
<section class="perguntas" v-if="!isLast && !greeting">
   <div>
        <h1>Pergunta {{ currentIndex + 1 }} de {{ perguntas.length }}</h1>
    </div>

    <div>
       <h3>{{ items.pergunta }}</h3> 
    </div>
    <div>
      <h4>Selecione uma opção abaixo (Obrigatório*)</h4>
    </div>  
    <div class="options"  >        
         <label v-for="opcao, index in items.opcoes" :key="index">
           <div>
              <input
                type="radio" 
                name="category" 
                id="category"
                :value= "opcao"
                v-model="input_resposta">
                <span class="bubble"></span>
                </div>              
            <div>{{ opcao }} </div>                    
         </label>            
      </div>

      <section class="input-conteiner">
         <div class="input-resposta" v-show="input_resposta">✅ {{input_resposta}}</div>   
      </section>       

   <div class="btn">
      <input 
      type="button"
      value="Próximo" 
      @click="nextQuestion()"/>
   </div>   
  
</section>
</main>

<main>
<div class="e-mail" v-if="isLast">
   <h1>Obrigado por chegar até aqui!</h1>
   <messege :msg="msg" v-show="msg"/>
   <h3>Deseja nos enviar um comentário?</h3>
   <h4>Enviar um comentário (Opcional*)</h4>
   
   <div class="cmt"> 
         <input 
         type="text" name="comentario" 
         id="cmt" 
         placeholder="Escreva seu comentário aqui..."
         v-model="input_comentario">
    </div>   
     
   <div class="btn" v-show="!isFinish">
      <input 
      type="button" 
      value="Finalizar" 
      @click="finish()">
   </div>

   <div v-show="isFinish">
      <button 
      class="btn-router"
      @click="$router.push('/')">
      Sair</button>
   </div>

</div>
</main>
</template>

<style scoped>
.titulo {
	display: block;
	text-align: center;	
	color: rgb(67, 83, 110);
	margin-bottom: 20px;
   font-weight: bold;
	font-size: 2rem;
	font-weight: 600;
   margin: 0 auto;
   width: 50% auto;
   padding: 30px 30px;

}
.bem-vindo span {
  display: block;
  flex-direction: auto;
  margin-bottom: 1.5rem;
  font-weight: bold;
  color: rgb(89, 89, 95);
  padding: 1.5rem 1.5rem;
  border-left: 4px solid #ffd21c;
  margin: 0 auto;
  width: 80%;
  margin-bottom: .5rem;
}

.bem-vindo input[type="email"] {
   display: block;
	font-size: 1.125rem;
	padding: 1rem 1.5rem;
	color: var(--escuro);
	background-color: #ffffff;
	border-radius: 0.5rem;
	box-shadow: var(--sombra);
   margin: 0 auto;
   margin-bottom: 0.5rem;
   width: 80%
}

.bem-vindo input[type="text"] {
   display: block;
	font-size: 1.125rem;
	padding: 1rem 1.5rem;
	color: var(--escuro);
	background-color: #ffffff;
	border-radius: 0.5rem;
	box-shadow: var(--sombra);
   margin: 0 auto;
   margin-bottom: 0.5rem;
   width:80%
}

h1 {
	display: block;
	text-align: center;	
	color: rgb(63, 79, 109);
	margin-bottom: 1rem;
   font-weight: bold;
	font-size: 1.8rem;
	font-weight: 550;
   margin: 0 auto;
   width: 50% auto;
   padding: 1.5rem;
}

h3 {
   
	color: rgb(75, 74, 74);
	font-size: 1.125rem;
	font-weight: bold 500;
	
   margin: 0 auto ;
   border-left: 4px solid #fdd63c; 
   padding: 5px 10px;
   
   
}
h4 {
	color: var(--cinza-escuro);
	font-size: 0.775rem;
	font-weight: 500;
	padding: 1.225rem .5rem;
   margin: 0 auto;
   
}


 input[type="text"] {
   display: flex;
	width: 98%;
	font-size: 1.125rem;
	padding: 3rem 1.5rem;
	color: var(--escuro);
	background-color: #ffffff;
	border-radius: 1.5rem;
	box-shadow: var(--sombra);
	margin-bottom: 2rem;
   margin: 0 auto;
}
.input-resposta { 
   display: block;
   font-size: 1rem;
   font-weight: 500;
   color: #218625;
   border: 1.5px solid rgba(150, 253, 150, 0.644);
   background-color: #e3ffe5;
   width: auto;
   border-radius: 0.5rem;
   padding: .5rem .5rem;
   text-align: center;
   margin: 0 auto;
   margin-top: 10px;
   margin-bottom: 10px;
   
   
   
}
.input-conteiner {
   display: flex;
   margin: 0 auto;
   width: 100%;
   position: absolute;
}

</style>
