<template>
  <transition name="modal-fade">
    <div class="modal-backdrop">
      <div class="modal">
        <header class="modal-header">
          <slot name="header">
            <span>Правила игры</span>

            <button type="button" class="btn-close" @click="close">x</button>
          </slot>
        </header>
        <section class="modal-body">
          <slot name="body"
            ><ul class="list">
              <li>Жми начать</li>
              <li>Следи за секторами которые загораются</li>
              <li>Повтори последоватлеьность</li>
              <li>С каждым раундом последовательность увеличивается на 1</li>
            </ul></slot
          >
        </section>
        <footer class="modal-footer">
          <slot name="footer">
            <button type="button" class="btn-green" @click="close">
              Close me!
            </button>
          </slot>
        </footer>
      </div>
    </div>
  </transition>
</template>

<script>
export default {
  name: "regulations",

  methods: {
    close() {
      this.$emit("close");
    },
  },
};
</script>

<style lang="scss" scoped>
.modal-backdrop {
  position: fixed;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  background-color: rgba(0, 0, 0, 0.3);
  display: flex;
  justify-content: center;
  align-items: center;
}

.modal {
  background: #1f212a;
  overflow-x: auto;
  display: flex;
  flex-direction: column;
}

.modal-header,
.modal-footer {
  display: flex;
  align-items: center;
  padding: 15px 10px;
}

.modal-header {
  border-bottom: 1px solid #eeeeee;
  color: #22b2ea;
  justify-content: space-between;
  span {
    padding: 6px 0px 0px 0px;
  }
}

.modal-footer {
  border-top: 1px solid #eeeeee;
  justify-content: flex-end;
}

.modal-body {
  position: relative;
  padding: 20px 10px;
}

.btn-close {
  border: none;
  font-size: 20px;
  cursor: pointer;
  font-weight: bold;
  color: #22b2ea;
  background: transparent;
}

.btn-green {
  color: white;
  background: #22b2ea;
  border-radius: 2px;
  padding: 10px 15px;
  border-radius: 34px;
}

.modal-fade-enter,
.modal-fade-leave-active {
  opacity: 0;
}

.modal-fade-enter-active,
.modal-fade-leave-active {
  transition: opacity 0.5s ease;
}

.list {
  li {
    padding: 0px 0px 10px 0px;
    text-align: left;
    &:before {
      content: "- ";
    }
  }
}
</style>