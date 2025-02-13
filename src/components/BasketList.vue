<template>
    <section class="basket-section">
        <button class="close-button" @click="$emit('close')">&times;</button>
        <h2 class="basket-title">Кошик <i class="fas fa-shopping-cart"></i></h2>
        <p v-if="basket.length === 0">Кошик поки порожній...</p>
        <template v-else>
            <ul class="basket-list">
                <notebook-item v-for="notebook in basket" :key="notebook.id" :notebook="notebook" type="basketList"
                    @delete="$emit('delete', notebook.id)" @increase="$emit('increase', notebook.id)"
                    @decrease="$emit('decrease', notebook.id)" />
            </ul>
            <p class="total-price">
                Сума: {{ basket.reduce((acc, item) => item.price * item.number + acc, 0) }} ₴
            </p>
            <div class="buttons-wrapper">
                <button class="action-button clear-button" type="button" @click="$emit('clear')">
                    Очистити кошик
                </button>
                <button class="action-button pay-button" type="button" @click="handlePayment">
                    Оплатити замовлення
                </button>
            </div>
        </template>
    </section>
</template>

<script>
import NotebookItem from "./NotebookItem.vue";
export default {
  name: "BasketList",
  components: {
    NotebookItem,
  },
  props: {
    basket: {
      type: Array,
      default: () => [],
    },
  },
  methods: {
    handlePayment() {
      alert("Дякуємо за замовлення!");
      this.$emit("clear");
      this.$emit("close");
      },
      handleEscape(event) {
          if (event.key === "Escape") {
              this.$emit("close");
          }
      },
    },
    mounted() {
        document.addEventListener("keydown", this.handleEscape);
    },
    beforeUnmount() {
        document.removeEventListener("keydown", this.handleEscape);
    },
};
</script>

<style lang="scss" scoped>
.basket-section {
  width: 30%;
  margin-top: 20px;
  padding: 20px;
  border: 5px solid #104724;
  border-radius: 30px;
  background: #91d079;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  height: 70vh;
  overflow-y: scroll;
  position: relative;
}

.basket-list {
  list-style: none;
  padding: 0;
  display: flex;
  flex-wrap: wrap;
  gap: 20px;
}

.close-button {
  position: absolute;
  top: 10px;
  right: 10px;
  background: transparent;
  border: none;
  font-size: 48px;
  cursor: pointer;
  color: #104724;
  transition: color 0.3s;
}

.close-button:hover {
  color: #ff0000;
}
.total-price {
  font-size: 24px;
  font-weight: bold;
  margin-top: 20px;
}

.buttons-wrapper {
  display: flex;
  justify-content: сenter;
  gap: 20px;
  margin-top: 20px;
}

.action-button {
  padding: 10px 20px;
  font-size: 16px;
  border: none;
  border-radius: 10px;
  cursor: pointer;
  transition: background-color 0.3s, transform 0.2s;
}

.clear-button {
  background: #ff4d4d;
  color: white;
}

.clear-button:hover {
  background: #cc0000;
  transform: scale(1.05);
}

.pay-button {
  background: #28a745;
  color: white;
}

.pay-button:hover {
  background: #1e7e34;
  transform: scale(1.05);
}
</style>
