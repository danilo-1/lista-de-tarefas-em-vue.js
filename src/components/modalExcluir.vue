<template>
    <div class="modal fade" id="modalExcluir" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
        <div class="modal-dialog modal-dialog-centered">
            <div class="modal-content">
            <div class="modal-header">
                <h5 class="modal-title text-danger" id="exampleModalLabel">Excluir Tarefa</h5>
                <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
            </div>
            <div class="modal-body">
                Deseja realmente excluir esta tarefa?
            </div>
            <div class="modal-footer">
                <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Close</button>
                <button type="button" class="btn btn-danger" @click="save(evento)" data-bs-dismiss="modal"><img src="@/assets/trash.svg" alt=""></button>
            </div>
            </div>
        </div>
    </div>
</template>
<script>
export default {
    props:{
        evento:{
            type:Object,
            required:false,
        }
    },
    setup(props, {emit}) {
        const save = function(){
            emit("salvar", props.evento )
        }
        return{
            save
        }
    },
}
</script>