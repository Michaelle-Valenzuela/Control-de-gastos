<script setup>
    import { ref } from 'vue'
    import Alerta from './Alerta.vue'

    //Es necesario crear una variable y apuntar al ref 
    const presupuesto = ref(0)
    const error = ref('')

    const emit = defineEmits(['definir-presupuesto'])

    const definirPresupuesto = () => {
        if(presupuesto.value <= 0 || presupuesto.value === '') {
            error.value = 'Presupuesto no valido'

            setTimeout(() => {
                error.value = ''
            }, 3000);

            return
        }

        emit('definir-presupuesto', presupuesto.value)
    }
</script>

<template>

    <form
        class="presupuesto"
        @submit.prevent="definirPresupuesto"
    >

        <!--Al ser un componente simple no es necesario usar props. Se usa slots.--> 
        <!--La etiqueta del componente es de apertura y cierre. Y renderiza todo lo que pases dentro de las etiquetas-->
        <Alerta
            v-if="error"
        >

            {{ error }}

        </Alerta>

        <div class="campo">

            <label for="nuevo-presupuesto">Definir presupuesto</label>

            <input
                id="nuevo-presupuesto"
                class="nuevo-presupuesto"
                placeholder="Ingresar presupuesto"
                type="number"
                min="0"
                v-model.number="presupuesto"
            />
                
            <input
                type="submit"
                value="Definir presupuesto"
            />
        </div>
    </form>
</template>

<style scoped>    
    .presupuesto{
        width: 100%;
    }
    .campo{
        display: grid;
        gap: 2rem;
    }
    .presupuesto label {
        font-size: 2.2rem;
        text-align: center;
        color: var(--azul);
    }
    .presupuesto input[type="number"]{
        background-color: var(--gris-claro);
        border-radius: 1rem;
        padding: 1rem;
        border: none;
        font-size: 2.2rem;
        text-align: center;
    }
    .presupuesto input[type="submit"]{
        background-color: var(--azul);
        border: none;
        padding: 1rem;
        text-align: center;
        font-size: 2rem;
        margin-top: 2rem;
        color: var(--blanco);
        font-weight: 900;
        width: 100%;
        border-radius: 1rem;
        transition: background-color 300ms ease-out;
    }
    .presupuesto input[type="submit"]:hover {
        background-color: #39A300;
        cursor: pointer;
    }

</style>