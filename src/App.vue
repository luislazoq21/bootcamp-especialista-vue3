<script setup>
    import empresas from './empresas.json'
</script>

<template>
    <h1>Primer challenge</h1>

    <table>
        <thead>
            <tr>
                <th>#</th>
                <th>Name</th>
                <th>Owner</th>
            </tr>
        </thead>
        <tbody>
            <tr v-for="empresa in empresas" :key="empresa.id">
                <td>{{ empresa.id }}</td>
                <td>{{ empresa.name }}</td>
                <td>{{ empresa.owner }}</td>
            </tr>
        </tbody>
    </table>
</template>

<style>
    #app {
        padding: 50px 30px;
    }
</style>