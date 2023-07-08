<script setup>
    import { ref } from 'vue'
    import empresas from './empresas.json'

    const enterprises = ref( empresas )
    const formData = ref({
        id: null,
        name: '',
        owner: ''
    })

    const onSubmit = () => {
        // editar
        if( formData.value.id ) {
            console.log('editar')
            return
        }

        // nuevo
        formData.value.id = crypto.randomUUID()
        enterprises.value = [ ...enterprises.value, formData.value ]


        // limpiamos formulario
        formData.value = {
            id: null,
            name: '',
            owner: ''
        }
    }

    const onUpdate = ( enterprise ) => {
        console.log( enterprise );
        formData.value = { ...enterprise }
    }
</script>

<template>
    <div class="container">
        <nav>
            <ul>
                <li>Lista de empresas</li>
            </ul>
            <ul>
                <li>
                    <button role="button">Nueva empresa</button>
                </li>
            </ul>
        </nav>

        <div class="grid">
            <div>
                <table>
                    <thead>
                        <tr>
                            <th>#</th>
                            <th>Name</th>
                            <th>Owner</th>
                            <th>Actions</th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr v-for="(enterprise, index) in enterprises" :key="enterprise.id" :data-id="enterprise.id">
                            <td>{{ index + 1 }}</td>
                            <td>{{ enterprise.name }}</td>
                            <td>{{ enterprise.owner }}</td>
                            <td>
                                <button @click="onUpdate(enterprise)">
                                    <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" style="fill: rgba(255, 255, 255, 1);transform: ;msFilter:;"><path d="m16 2.012 3 3L16.713 7.3l-3-3zM4 14v3h3l8.299-8.287-3-3zm0 6h16v2H4z"></path></svg>
                                </button>
                            </td>
                        </tr>
                    </tbody>
                </table>
            </div>
            <div>
                <form @submit.prevent="onSubmit">
                    <label>
                        Nombre empresa
                        <input
                            type="text"
                            placeholder="Ex. Mi empresa"
                            required
                            v-model="formData.name"
                        />
                    </label>
                    <label>
                        Nombre propietario
                        <input
                            type="text"
                            placeholder="Ex. Luis"
                            required
                            v-model="formData.owner"
                        />
                    </label>
                    <button type="submit">
                        Guardar
                    </button>
                </form>
            </div>
        </div>
    </div>
</template>
