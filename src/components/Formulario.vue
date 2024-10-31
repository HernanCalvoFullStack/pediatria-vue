<script setup>
    import { reactive, computed } from "vue"
    import Alerta from "./Alerta.vue"

    const alerta = reactive({
        tipo: "",
        mensaje: ""
    })

    const emit = defineEmits(["update:nombre", "update:responsable", "update:email", "update:alta", "update:sintomas", "guardar-paciente"])

    const props = defineProps({
        id: {
            type: [String, null],
            required: true
        },
        nombre: {
            type: String,
            required: true
        },
        responsable: {
            type: String,
            required: true
        },
        email: {
            type: String,
            required: true
        },
        alta: {
            type: String,
            required: true
        },
        sintomas: {
            type: String,
            required: true
        }
    })

    const validar = () => {
        if(Object.values(props).includes("")) {
            alerta.mensaje = "Todos los campos son obligatorios"
            alerta.tipo = "error"
            return
        }

        emit("guardar-paciente")
        alerta.mensaje = "Paciente Almacenado Correctamente"
        alerta.tipo = "exito"

        setTimeout(() => {
            Object.assign(alerta, {
                tipo: "",
                mensaje: ""
            })
        }, 2000)
    }

    const editando = computed(()=> {
        return props.id
    })

</script>

<template>
    <div class="w-1/2">
        <h2 class="font-black text-3xl text-center">Seguimiento Pacientes</h2>

        <p class="text-lg mt-5 text-center mb-10">
            Agrega Pacientes y
            <span class="text-cyan-600 font-bold">Adminístralos</span>
        </p>

        <Alerta 
            v-if="alerta.mensaje"
            :alerta="alerta"
        />
        <form 
            class="bg-white shadow-xl rounded-lg py-10 px-5 mb-10"
            @submit.prevent="validar"
        >
            <div class="mb-5">
                <label 
                    for="nombre" 
                    class="block text-slate-700 uppercase font-bold"
                >
                    Nombre Paciente
                </label>

                <input 
                    id="nombre"
                    type="text"
                    placeholder="Nombre del Paciente"
                    class="border-2 w-full p-2 mt-2 placeholder-slate-400 rounded-md"
                    :value="nombre"
                    @input="$emit('update:nombre', $event.target.value)"
                >
            </div>

            <div class="mb-5">
                <label 
                    for="responsable" 
                    class="block text-slate-700 uppercase font-bold"
                >
                    Nombre Responsable
                </label>

                <input 
                    id="responsable"
                    type="text"
                    placeholder="Nombre del Responsable"
                    class="border-2 w-full p-2 mt-2 placeholder-slate-400 rounded-md"
                    :value="responsable"
                    @input="$emit('update:responsable', $event.target.value)"
                >
            </div>

            <div class="mb-5">
                <label 
                    for="email" 
                    class="block text-slate-700 uppercase font-bold"
                >
                    Email de Contacto
                </label>

                <input 
                    id="email"
                    type="email"
                    placeholder="Email del Responsable"
                    class="border-2 w-full p-2 mt-2 placeholder-slate-400 rounded-md"
                    :value="email"
                    @input="$emit('update:email', $event.target.value)"
                >
            </div>

            <div class="mb-5">
                <label 
                    for="alta" 
                    class="block text-slate-700 uppercase font-bold"
                >
                    Alta del Paciente
                </label>

                <input 
                    id="alta"
                    type="date"
                    class="border-2 w-full p-2 mt-2 placeholder-slate-400 rounded-md"
                    :value="alta"
                    @input="$emit('update:alta', $event.target.value)"
                >
            </div>

            <div class="mb-5">
                <label 
                    for="sintomas" 
                    class="block text-slate-700 uppercase font-bold"
                >
                    Síntomas
                </label>

                <textarea 
                    id="sintomas"
                    placeholder="Síntomas del Paciente" 
                    class="border-2 w-full p-2 mt-2 placeholder-slate-400 rounded-md h-40"
                    :value="sintomas"
                    @input="$emit('update:sintomas', $event.target.value)"
                />
            </div>

            <input 
                type="submit" 
                class="bg-cyan-600 w-full p-3 text-white uppercase font-bold hover:bg-cyan-700 cursor-pointer transition-colors rounded-lg"
                :value="[editando ? 'Guardar Cambios' : 'Registrar Paciente']"
            />
        </form>
    </div>

</template>

