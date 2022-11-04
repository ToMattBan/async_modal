<template>
  <div class="modal" :class="{ active: modalActive }" id="modalDemonstracao">
    <div class="background" @click="fechar"></div>
    <div class="modal-container">
      <div class="modal-content">
        <h1>Remover jogador?</h1>
        <p>Deseja mesmo remover este jogador?</p>
      </div>

      <div class="modal-footer _mla _df">
        <a class="_c-btn _c-btn--deny _mrxs _mla">Cancelar</a>
        <a class="_c-btn _c-btn--confirm _mrxs" @click="continuarPromise"
          >Confirmar</a
        >
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref } from "vue";

export default defineComponent({
  setup() {
    const modalActive = ref<boolean>(false);
    const resolucaoPromise = ref();

    const abrir = () => {
      modalActive.value = true;

      return new Promise((res) => {
        resolucaoPromise.value = res;
      });
    };

    const fechar = () => {
      modalActive.value = false;
    };

    const continuarPromise = () => {
      resolucaoPromise.value();
      fechar();
    };

    return {
      modalActive,
      abrir,
      fechar,
      continuarPromise,
    };
  },
});
</script>

<style scoped lang="scss">
.modal {
  position: fixed;
  display: flex;
  height: 0;
  width: 0;
  overflow: hidden;
  left: 50%;
  top: 50%;
  transform: translateX(-50%) translateY(-50%);
  transition: all 0.3s;

  &.active {
    height: 100vh;
    width: 100vw;
  }

  .background {
    background: black;
    width: 100%;
    height: 100%;
    opacity: 30%;
    position: absolute;
    left: 50%;
    transform: translateX(-50%) translateY(-50%);
    top: 50%;
    transition: all 0.4s ease;
  }

  .modal-container {
    position: relative;
    background: white;
    border-radius: 5px;
    width: 70%;
    padding: 16px;
    margin-left: auto;
    margin-right: auto;
    margin-top: auto;
    margin-bottom: auto;
    z-index: 1;
    box-shadow: 10px 10px 5px 0px rgba(0, 0, 0, 0.28);
    -webkit-box-shadow: 10px 10px 5px 0px rgba(0, 0, 0, 0.28);
    -moz-box-shadow: 10px 10px 5px 0px rgba(0, 0, 0, 0.28);
  }
}
</style>
