<template>
    <h1>{{ msg }}</h1>
</template>

<script>

export default {
    name: 'MeuComponente',
    props: {
        msg: String,
    }
}
</script>