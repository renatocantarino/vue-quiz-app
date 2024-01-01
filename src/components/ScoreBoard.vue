<template>
    <section class="score">
        Player <span> {{ me }}</span> X <span> {{ pc }}</span> Computer
    </section>
</template>

<script>
export default {
    props: [ "me" , "pc"]
};
</script>

<style lang="scss">
section.score {
    border-bottom: 1px solid black;
    padding: 24px;
    font-size: 18px;

    span {
        padding: 18px;
        font-weight: bold;
        border: 1px solid black;
    }
}
</style>
