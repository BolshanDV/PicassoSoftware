<template>
  <section class="form">
    <section class="from__column">
      <Form/>
    </section>
  </section>

</template>

<script>
export default {
  name: 'IndexPage',
}
</script>

<style scoped>
  .form{
    margin-top: 10vh;
    /*width: 100vw;*/
  }
  .from__column{
    display: flex;
    flex-direction: column;
    justify-content: flex-start;
    align-items: center;
  }
</style>
