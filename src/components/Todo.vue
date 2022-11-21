<template>
  <div class="card text-white bg-primary" id="banner">
    <div class="card-body">
      <div class="row">
        <div class="col"></div>
        <div class="col">
          <i class="fa fa-bars" style="font-size: 18px"></i>
          FRAMEWORKS
        </div>
        <div class="col">
          <button
            type="button"
            class="btn btn-primary float-end"
            data-bs-toggle="modal"
            data-bs-target="#exampleModal"
          >
            <i class="fa-solid fa-circle-plus"></i> Add
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Todo',
  props: {
    msg: String,
  },
};
</script>

<style scoped>
#banner {
  position: relative;
  background-color: #1666bf;
  top: 0;
}
</style>
