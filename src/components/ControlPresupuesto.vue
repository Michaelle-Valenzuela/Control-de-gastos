<script setup>
    import imagen from '../assets/img/grafico.jpg'
    import {formatearCantidad} from '../helpers'

    const props = defineProps({
        presupuesto: {
            type: Number,
            required: true
        },
        disponible: {
            type: Number,
            required: true
        },
        gastado: {
            type: Number,
            required: true
        }
    })

</script>

<template>

    <div class="dos-columnas">

        <div class="contenedor-grafico">

            <!--El :src es contenido dinamico-->
            <img
                :src="imagen"
            />

        </div>

        <div class="contenedor-presupuesto">

            <button class="reset-app">
                Reiniciar aplicacion
            </button>

            <p>
                <span>Presupuesto:</span>
                 {{ formatearCantidad(presupuesto) }}
            </p>

            <p>
                <span>Disponible:</span>
                 {{ formatearCantidad(presupuesto) }}
            </p>

            <p>
                <span>Gastado:</span>
                {{ formatearCantidad(gastado) }}                
            </p>

        </div>

    </div>

</template>

<style scoped>

    .dos-columnas{
        display: flex;
        flex-direction: column;
    }

    .dos-columnas > :first-child {
        margin-bottom: 3rem;
    }

    @media (min-width: 768px) {
        .dos-columnas {
            flex-direction: row;
            gap: 4rem;
            align-items: center;
        }

        .dos-columnas > :first-child {
        margin-bottom: 0rem;
        }
    }

    .reset-app {
        background-color: #db2777;
        border: none;
        padding: 1rem;
        width: 100%;
        color: var(--blanco);
        font-weight: bold;
        text-transform: uppercase;
        border-radius: .5rem;
        transition-property: background-color;
        transition-duration: 300ms;
    }

    .reset-app:hover {
        cursor: pointer;
        background-color: #291302;
    }
    .contenedor-presupuesto{
        width: 100%;
    }

    .contenedor-presupuesto p{
        font-size: 2.4rem;
        text-align: center;
        color: var(--gris-oscuro);
    }

    @media (min-width: 768px) {
        .contenedor-presupuesto p {
            text-align: left;
        }
    }

    .contenedor-presupuesto span{
        font-weight: 900;
        color: var(--azul);
    }

</style>