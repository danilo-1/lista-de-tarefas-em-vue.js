<template>
    <div class="container-xxl pb-5">
    <div class=" card p-3 shadow-lg p-3 mb-5 mx-5 bg-body rounded">
      <div class="row d-flex justify-content-center">
          <div class="col-6">
                <div class="form-floating mb-3">
                    <input type="text" class="form-control" v-model="campoInserir" id="floatingInput" placeholder="Insira uma tarefa">
                    <label for="floatingInput">Insira uma tarefa</label>
                </div>
          </div>
          <div class="col-2">
            <div class="form-floating">
              <select class="form-select" id="floatingSelect" v-model="prioridade" aria-label="Floating label select example">
                <option selected disabled>Selecione</option>
                <option value="1" class=" text-white bg-dark">Muito alta = A+</option>
                <option value="2" class="text-white bg-danger">Alta = A</option>
                <option value="3" class="text-white bg-warning">Média = M</option>
                <option value="4" class="text-white bg-primary">Baixa = B+</option>
                <option value="5" class="text-white bg-info">Muito baixa B</option>
              </select>
              <label for="floatingSelect">Prioridade</label>
            </div>
          </div>
          <div class="col-2">
              <button class="btn btn-primary p rounded mt-2" @click="inserirTarefa()">Adicionar</button>
          </div>
      </div>
      <div class="row d-flex justify-content-evenly">
        <div class=" row d-flex justify-content-center col-3" >
          <div class="col-12"><span class="h4 ">A fazer</span></div>
          <div class="col-12 row border my-2" v-for="(faz,index) in fazer" :key="index" @click="executar(faz)" id="styleH" >
          <div class="col-10">
          <span class=" h5">{{faz.campo}}</span> 
          </div>
          <div class="col-2 my-auto">
          <span class="badge " :class="faz.classePrioridade">{{faz.prioridade}}</span>
          </div>
          </div>
        </div>
        <div class="row d-flex jusity-content-center col-3">
          <div class="col-12"><span class="h4 ">Executando</span></div>
          <div class="col-12 row border my-2" v-for="(faz,index) in executando" :key="index" @click="finalizar(faz)" id="styleH" >
          <div class="col-10">
          <span class=" h5">{{faz.campo}}</span> 
          </div>
          <div class="col-2 my-auto">
          <span class="badge " :class="faz.classePrioridade">{{faz.prioridade}}</span>
          </div>
          </div>
        </div>
        <div class="d-flex justify-content-center row col-3">
          <div class="col-12"><span class="h4 ">Finalizado</span></div>
          <div class="col-12 border my-2 row" v-for="(faz,index) in finalizado" :key="index" @dblclick="openModal(faz)" id="styleH">
          <div class="col-10">
          <span class=" h5">{{faz.campo}}</span> 
          </div>
          <div class="col-2 my-auto">
          <span class="badge " :class="faz.classePrioridade">{{faz.prioridade}}</span>
          </div>
          </div>
        </div>
      </div>
    </div>
  </div>
  <modal-excluir :evento="eventoModal" @salvar="remover($event)"/>
</template>
<script>
import { reactive, ref, toRefs } from "vue"
import bootstrap from "bootstrap/dist/js/bootstrap.bundle.min.js";
import modalExcluir from './modalExcluir.vue';
export default {
  components: { modalExcluir },
    name:'ListaTarefa',
    setup() {
        const campoInserir = ref('')
        const prioridade = ref(0)
        const fazer = ref([])
        const eventoModal = ref({})
        const executando = ref([])
        const finalizado = ref([]) 
       const methods = reactive({
           inserirTarefa(){
             let valor = campoInserir.value
             let auxC = ''
             let auxP = ''
             if(campoInserir.value.length > 0 ){
              if(prioridade.value == 0){
                prioridade.value = 5
              }
              
              if(prioridade.value == 1){
                auxC = 'bg-dark'
                auxP = 'A+'
              }
              else if(prioridade.value == 2){
                auxC ='bg-danger' 
                auxP = 'A'
              }
              else if(prioridade.value == 3){
                auxC ='bg-warning'
                auxP = 'M'
              }
              else if(prioridade.value == 4){
                auxC ='bg-primary'
                auxP = 'B+'
              }
              else if(prioridade.value == 5){
                auxC ='bg-info';
                auxP = 'B'
              }
              
              fazer.value.push({ campo:valor, classePrioridade:auxC, prioridade:auxP})
             }
             campoInserir.value = ''
             prioridade.value = 0
           },
           executar(event){
             console.log(event)
             fazer.value = fazer.value.filter((x)=>{return x.campo != event.campo})
             executando.value.push(event)
           },
           finalizar(event){
             console.log(event)
             executando.value = executando.value.filter((x)=>{return x.campo != event.campo})
             finalizado.value.push(event)
           },
           remover(event){
             console.log(event)
             finalizado.value = finalizado.value.filter((x)=>{return x.campo != event.campo})
           },
           openModal(event){
              console.log(event)
              eventoModal.value = event
              var modal = new bootstrap.Modal(
                document.getElementById("modalExcluir"),
                {
                  keyboard: false,
                  
                }
              );
              var modalToggle = document.getElementById(
                "modalExcluir"
              );
              modal.show(modalToggle);
           }
       })
       
       return{
          ...toRefs(methods),
          fazer,
          executando,
          finalizado,
          campoInserir,
          prioridade,
          eventoModal
       }
    },
}
</script>
<style>
#styleH:hover{
  background-color: aqua;
}
#styleH{
    animation: slide-out 0.5s forwards;
  -webkit-animation: slide-out 0.5s forwards;
  transition-property: font-size;
}
@keyframes slide-in {
   0% { -webkit-transform: translateX(0%); }
   100% { -webkit-transform: translateX(-100%); }
}
@-webkit-keyframes slide-in {
     0% {transform: translateX(0%); }
    100% { transform: translateX(-100%); }
}
@keyframes slide-out {
    0% { transform: translateX(-100%); }
    100% { transform: translateX(0%); }
}
@-webkit-keyframes slide-out {
    0% { -webkit-transform: translateX(-100%); }
    100% { -webkit-transform: translateX(0%); }
}
</style>