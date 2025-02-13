<template>
  <div class="app-container">
    <button class="basket-button" @click="toggleBasket">
      <i class="fas fa-shopping-cart"></i>
      <span
        v-if="basket.length"
        class="basket-count"
        :class="{ 'bounce-effect': buttonEffect }"
        >{{ totalBasketItems }}</span
      >
    </button>
    <div class="lists-wrapper">
      <notebooks-list :notebooks="notebooks" @add="addToBasket" />
      <basket-list
        v-if="isBasketVisible"
        :basket="basket"
        @clear="clearBasket"
        @delete="deleteFromBasket"
        @increase="onIncrease"
        @decrease="onDecrease"
        @close="closeBasket"
      />
    </div>
  </div>
</template>

<script>
import { notebooksList } from "./constants/notebooks.js";
import NotebooksList from "./components/NotebooksList.vue";
import BasketList from "./components/BasketList.vue";

export default {
  name: "App",
  components: {
    NotebooksList,
    BasketList,
  },
  data() {
    return {
      notebooks: notebooksList,
      basket: [],
      isBasketVisible: false,
      buttonEffect: false,
    };
  },
  methods: {
    addToBasket(notebook) {
      const item = this.basket.find((item) => item.id === notebook.id);
      if (item) {
        item.number++;
      } else {
        this.basket.push({ ...notebook, number: 1 }); 
      }
    },
    deleteFromBasket(id) {
      const bIndex = this.basket.findIndex((notebook) => notebook.id === id);
      this.basket.splice(bIndex, 1);
      const nIndex = this.notebooks.findIndex((notebook) => notebook.id === id);
      this.notebooks[nIndex].number = 1;
    },
    clearBasket() {
      this.basket = [];
      this.notebooks.forEach((notebook) => (notebook.number = 1));
    },
    onIncrease(id) {
      const basketItem = this.basket.find((item) => item.id === id);
      if (basketItem) {
        basketItem.number++;
      }

      const notebook = this.notebooks.find((n) => n.id === id);
      if (notebook) {
        notebook.number = basketItem.number;
      }

    },

    onDecrease(id) {
      const basketItem = this.basket.find((item) => item.id === id);
      if (basketItem && basketItem.number > 1) {
        basketItem.number--;
      }

      const notebook = this.notebooks.find((n) => n.id === id);
      if (notebook) {
        notebook.number = basketItem.number;
      }
    },
    toggleBasket() {
      this.isBasketVisible = !this.isBasketVisible;
    },
    closeBasket() {
      this.isBasketVisible = false;
    },
  },
  computed: {
    totalBasketItems() {
      return this.basket.reduce((acc, item) => item.number + acc, 0);
    },
  },
  watch: {
    basket: {
      handler(value) {
        localStorage.setItem("basket", JSON.stringify(value));
        this.buttonEffect = true;
        setTimeout(() => {
          this.buttonEffect = false;
        }, 1000);
      },
      deep: true,
    },
    isBasketVisible(value) {
      localStorage.setItem("isBasketVisible", value);
    },
  },
  mounted() {
    const savedBasket = JSON.parse(localStorage.getItem("basket")) ?? [];
    this.basket = savedBasket.map((item) => {
      const notebook = this.notebooks.find((n) => n.id === item.id);
      return notebook ? { ...notebook, number: item.number } : { ...item }; 
    });
    this.isBasketVisible =
      JSON.parse(localStorage.getItem("isBasketVisible")) ?? false;
  },
};
</script>

<style scoped>
.app-container {
  padding-bottom: 130px;
  position: relative;
  background-color: #202d3a;
}
@keyframes bounce {
  0%,
  100% {
    transform: translateY(0);
  }

  50% {
    transform: translateY(-5px);
  }
}

.basket-button {
  position: absolute;
  bottom: 100px;
  right: 500px;
  background-color: #ff6b6b;
  color: white;
  border: none;
  width: 80px;
  height: 80px;
  padding: 10px 15px;
  font-size: 18px;
  border-radius: 50%;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
}

.basket-button .basket-count {
  background: white;
  color: #6a1e1e;
  font-size: 16px;
  font-weight: bold;
  border-radius: 50%;
  padding: 10px 10px;
  margin-left: 5px;
  transition: transform 0.3s ease-in-out;
}

.basket-count.bounce-effect {
  animation: bounce 0.3s ease-in-out;
}

.lists-wrapper {
  display: flex;
}
</style>