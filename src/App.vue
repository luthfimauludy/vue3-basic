<template>
    <div>{{message}}</div>

    <span :title="message">blablabla</span>
</template>

<script>
export default {
    data() {
        return {
            message: 'hi world'
        }
    }
};
</script>
