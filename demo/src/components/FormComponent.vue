<template>
  <div class="container">
    <form action="" class="form">
      <div class="input-group-container">
        <div class="input-group">
          <i></i>
          <input type="text" />
        </div>

        <div class="input-group">
          <i></i>
          <input type="text" />
        </div>
      </div>
      <div class="input-group-container">
        <div class="input-group">
          <i></i>
          <input type="text" />
        </div>

        <div class="input-group">
          <i></i>
          <input type="text" />
        </div>
      </div>
      <div class="input-group-container">
        <div class="input-group">
          <i></i>
          <input type="text" />
        </div>

        <div class="input-group">
          <i></i>
          <input type="text" />
        </div>
      </div>
    </form>
  </div>
</template>

<script setup lang="ts"></script>

<style scoped lang="scss">
.container {
  background-color: #0d183d;
  display: flex;
  align-items: center;
  height: 100%;
  justify-content: center;
}

.form {
  background-color: #d7d1d1;
  border-radius: 30px;
  display: flex;
  gap: 10px;
  flex-direction: column;
  max-width: 760px;
  padding: 20px;
  width: 100%;

  .input-group-container {
    display: flex;
    gap: 10px;
    justify-content: space-between;
  }

  .input-group {
    border: 1px solid gray;
    border-radius: 40px;
    display: flex;
    flex-grow: 1;
    padding: 20px;

    i {
      background: url('@/assets/icons/generic_icon.svg') center/cover no-repeat;
      display: block;
      height: 20px;
      margin-right: 10px;
      width: 20px;
    }
  }

  label {
    display: block;
    margin-bottom: 10px;
  }

  input {
    display: block;
    flex-grow: 1;
  }
}
</style>
