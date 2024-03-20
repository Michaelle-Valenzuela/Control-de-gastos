<script setup>
    import {ref} from 'vue'
    import Alerta from './Alerta.vue';
    import cerrarModal from '../assets/img/cerrar.svg'

    const error = ref('')

    const emit = defineEmits(['ocultar-modal', 'guardar-gasto', 'update:nombre', 'update:cantidad', 'update:categoria'])

    const props = defineProps({
        modal: {
            type: Object,
            required: true
        },
        nombre: {
            type: String,
            required: true
        },
        cantidad: {
            type: [String, Number],
            required: true
        },
        categoria: {
            type: String,
            required: true
        },
        disponible: {
            type: Number,
            required: true
        },
        id: {
            type: [String, null],
            required: true
        }
    })    

    const gastoprevio = props.cantidad

    const agregarGasto  = () => {
        //Validacion de datos
        //Validar que los campos no esten vacios
        const { cantidad, categoria, nombre, disponible, id } = props
        if([nombre, cantidad, categoria].includes('')) {
        // if([nombre, cantidad, categoria]) {            
            error.value = 'Todos los campos son obligatorios'
            setTimeout(() => {
                error.value = ''
            }, 3000);
            return
        }

        //Validar cantidad
        if(cantidad <= 0 ) {
            error.value = 'Cantidad ingresada no valida'
            setTimeout(() => {
                error.value = ''
            }, 3000);
            return
        }

        //Validar q el usuario no gaste mas de lo disponible
        if(id){//Si hay un id significa que se esta editando el arreglo
            //Es necesario tomar ne cuenta el gasto realizado al momento de editar
            if(cantidad > gastoprevio + disponible) {
                error.value = 'Gastaste mas de lo que tienes. Que irresponsable 😡'
                setTimeout(() => {
                    error.value = ''
                }, 3000);
                return
            }
        }else{            
            if(cantidad > disponible ) {
                error.value = 'Gastaste mas de lo que tienes. Que irresponsable 😡'
                setTimeout(() => {
                    error.value = ''
                }, 3000);
                return
            }
        }


        emit('guardar-gasto')
    }

</script>

<template>
    <div class="modal">
        <div class="cerrar-modal">
            <img
                :src="cerrarModal"
                @click="$emit('ocultar-modal')"
            />
        </div>

        <div 
            class="contenedor contenedor-formulario"
            :class="[modal.animar ? 'animar' : 'cerrar']"
        >            
            <form 
                class="nuevo-gasto"
                @submit.prevent="agregarGasto"
            >
                <legend>Añadir gasto</legend>
                <Alerta v-if="error">{{ error }}</Alerta>
                <div class="campo">
                    <label for="nombre">Nombre del gasto:</label>
                    <input
                        type="text"
                        id="nombre"
                        placeholder="Añade el nombre del gasto"
                        :value="nombre"
                        @input="$emit('update:nombre', $event.target.value)"
                    />
                </div>    

                <div class="campo">
                    <label for="cantidad">Cantidad:</label>
                    <input
                        type="number"
                        id="cantidad"
                        placeholder="Añade la cantidad del gasto, ejem. 300"
                        :value="cantidad"
                        @input="$emit('update:cantidad', +$event.target.value)"
                    />
                </div>          

                <div class="campo">
                    <label for="categoria">Categoria:</label>
                    <select
                        id="categoria"
                        :value="categoria"
                        @input="$emit('update:categoria', $event.target.value)"
                    >
                        <option value="">-- Selecciones --</option>
                        <option value="ahorro">Ahorro</option>
                        <option value="comida">Comida</option>
                        <option value="casa">Casa</option>
                        <option value="gastos">Gastos varios</option>
                        <option value="ocio">Ocio</option>
                        <option value="salud">Salud</option>
                        <option value="suscripciones">Suscripciones</option>
                    </select>
                </div>
                
                <input
                    type="submit"
                    value="Añadir gasto"
                />
            </form>
        </div>
    </div>
</template>


<style scoped>
    .modal{
        position: absolute;
        /* El 0.9 significa transparencia y va del 0.1 al 1. 0.9 = 90%  */
        background-color: rgb(0 0 0 / 0.9);
        top: 0;
        left: 0;
        right: 0;
        bottom: 0;
    }
    .cerrar-modal{
        position: absolute;
        right: 3rem;
        top: 3rem;
    }
    .cerrar-modal img {
        width: 3rem;
        cursor: pointer;
    }
    .contenedor-formulario {
        transition-property: all;
        transition-duration: 300ms;
        transition-timing-function: ease-in-out;
        opacity: 0;
    }
    .contenedor-formulario.animar {
        opacity: 1;
    }
    .contenedor-formulario.cerrar {
        opacity: 0;
    }
    .nuevo-gasto {
        margin-top: 10rem auto 0 auto;        
        display: grid;
        gap: 2rem;
    }
    .nuevo-gasto legend {
        text-align: center;
        color: white;
        font-size: 3rem;
        font-weight: 700;
    }
    .campo {
        display: grid;
        gap: 2rem;
    }
    .nuevo-gasto input,
    .nuevo-gasto select {
        background-color: var(--gris-claro);
        border-radius: 1rem;
        padding: 1rem;
        border: none;
        font-size: 2.2rem;
    }
    .nuevo-gasto label {
        color:  var(--blanco);
        font-size: 3rem;
    }
    .nuevo-gasto input[type="submit"] {
        background-color: var(--azul);
        color: white;
        font-weight: 700;
        cursor: pointer;
    }
</style>