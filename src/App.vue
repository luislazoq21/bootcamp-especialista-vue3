<script setup>
    import { computed, ref } from 'vue'
    import empresas from './empresas.json'

    const enterprises = ref( empresas )
    const formData = ref({
        id: null,
        name: '',
        owner: ''
    })
    const showModal = ref( false )
    const enterpriseToDelete = ref( null )
    const txtName = ref()

    const title = computed( () => formData.value.id ? 'Editando...' : 'Nueva empresa' )

    const onSubmit = () => {
        const { id, name, owner } = formData.value

        // editar
        if( id ) {
            const idx = enterprises.value.findIndex( e => e.id === id )

            enterprises.value[idx].id = id
            enterprises.value[idx].name = name
            enterprises.value[idx].owner = owner
        } else {
            // nuevo
            formData.value.id = crypto.randomUUID()
            enterprises.value = [ ...enterprises.value, formData.value ]
        }

        // limpiamos formulario
        formData.value = {
            id: null,
            name: '',
            owner: ''
        }
    }

    const onUpdate = ( enterprise ) => {
        formData.value = { ...enterprise }
    }

    const onPrevDelete = ( enterprise ) => {
        enterpriseToDelete.value = enterprise
        showModal.value = true
    }

    const onDelete = () => {
        const { id } = enterpriseToDelete.value 
        enterprises.value = enterprises.value.filter( e => e.id !== id )
        showModal.value = false
    }

    const onNewCompany = () => {
        formData.value = ref({
            id: null,
            name: '',
            owner: ''
        })

        txtName.value.focus()
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
                    <button role="button" @click="onNewCompany">Nueva empresa</button>
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
                        <tr v-if="enterprises.length <= 0">
                            <td colspan="4">No hay registros</td>
                        </tr>
                        <tr v-for="(enterprise, index) in enterprises" :key="enterprise.id" :data-id="enterprise.id">
                            <td>{{ index + 1 }}</td>
                            <td>{{ enterprise.name }}</td>
                            <td>{{ enterprise.owner }}</td>
                            <td>
                                <div class="flex">
                                    <button @click="onUpdate(enterprise)">
                                        <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" style="fill: rgba(255, 255, 255, 1);transform: ;msFilter:;"><path d="m16 2.012 3 3L16.713 7.3l-3-3zM4 14v3h3l8.299-8.287-3-3zm0 6h16v2H4z"></path></svg>
                                    </button>
                                    <button @click="onPrevDelete(enterprise)">
                                        <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" style="fill: rgba(255, 255, 255, 1);transform: ;msFilter:;"><path d="M6 7H5v13a2 2 0 0 0 2 2h10a2 2 0 0 0 2-2V7H6zm10.618-3L15 2H9L7.382 4H3v2h18V4z"></path></svg>
                                    </button>
                                </div>
                            </td>
                        </tr>
                    </tbody>
                </table>
            </div>
            <div>
                <h2>
                    {{ title }}
                </h2>
                <form @submit.prevent="onSubmit">
                    <label>
                        Nombre empresa
                        <input
                            type="text"
                            placeholder="Ex. Mi empresa"
                            required
                            v-model="formData.name"
                            ref="txtName"
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

        <dialog :open="showModal">
            <article>
                <h3>Confirm your action!</h3>
                <p>
                    ¿Estás seguro de eliminar esta empresa?
                </p>
                <footer>
                    <a
                        href="#cancel"
                        role="button"
                        class="secondary"
                        @click="showModal = false"
                    >
                        Cancel
                    </a>
                    <a
                        href="#confirm"
                        role="button"
                        @click="onDelete"
                    >
                        Confirm
                    </a>
                </footer>
            </article>
        </dialog>
    </div>
</template>

<style scoped>
    .flex {
        display: flex;
        gap: .5rem;
    }
    .flex button:last-child {
        background-color: red;
    }
</style>
