<template>
    <li class="notebook-item" :class="{ 'basket-item': type === 'basketList' }">
        <div class="notebook-item__wrapper">
            <img :src="notebook.imgSrc" alt="notebook.title" class="notebook-item__image" />
            <div class="notebook-item__info">
                <h3 class="notebook-item__title">{{ notebook.title }}</h3>
                <template v-if="type === 'ordinaryList'">
                    <p class="notebook-item__discount">
                        Ваша економія:
                        <span class="discount-price">{{ notebook.discount }}</span>
                    </p>
                    <p class="notebook-item__oldPrice">{{ notebook.oldPrice }} ₴</p>
                    <p class="notebook-item__price">{{ notebook.price }} ₴</p>
                    <div class="button-wrapper">
                        <a :href="notebook.link" target="_blank" class="buy-button">Купити</a>
                        <button type="button" class="cart-button" @click="$emit('add')">
                            <i class="fas fa-shopping-cart"></i> Додати до кошика
                        </button>
                    </div>
                </template>
                <template v-else-if="type === 'basketList'">
                    <div class="counter-input">
                        <button @click="$emit('decrease')">-</button>
                        <input type="number" :value="notebook.number" readonly />
                        <button @click="$emit('increase')">+</button>
                    </div>
                    <p class="notebook-item__price">
                        {{ notebook.price * notebook.number }} ₴
                    </p>
                    <button type="button" class="delete-button" @click="$emit('delete')">
                        Видалити з кошика
                    </button>
                </template>
            </div>
        </div>
    </li>
</template>

<script>
export default {
  name: "NotebookItem",
  props: {
    notebook: {
      type: Object,
      required: true,
    },
    type: {
      type: String,
      required: true,
    },
  },
};
</script>

<style lang="scss" scoped>
.notebook-item.basket-item {
  width: 100%;
}
.notebook-item {
  width: 100%;
  list-style: none;
  border: 1px solid #ddd;
  border-radius: 8px;
  padding: 24px;
  margin: 10px 0;
  background: #fff;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);

  &__wrapper {
    display: flex;
    align-items: center;
    gap: 16px;
    justify-content: space-between;
  }

  &__image {
    width: 150px;
    height: auto;
    border-radius: 4px;
  }

  &__info {
    flex: 1;
  }

  &__title {
    font-size: 18px;
    font-weight: bold;
    margin-bottom: 8px;
  }

  &__discount {
    color: #d9534f;
    font-weight: bold;

    .discount-price {
      font-size: 16px;
    }
  }

  &__oldPrice {
    text-decoration: line-through;
    color: #999;
  }

  &__price {
    font-size: 20px;
    font-weight: bold;
    color: #5cb85c;
  }

  .button-wrapper {
    display: flex;
    gap: 10px;
    margin-top: 10px;
  }

  .buy-button,
  .cart-button,
  .delete-button {
    padding: 8px 16px;
    border: none;
    cursor: pointer;
    border-radius: 4px;
    transition: 0.3s;
  }

  .buy-button {
    background-color: #28a745;
    color: white;
    text-decoration: none;

    &:hover {
      background-color: #218838;
    }
  }

  .cart-button {
    background-color: #f0ad4e;
    color: white;
    display: flex;
    align-items: center;
    gap: 5px;

    &:hover {
      background-color: #ec971f;
    }
  }

  .delete-button {
    background-color: #d9534f;
    color: white;

    &:hover {
      background-color: #c9302c;
    }
  }

  .counter-input {
    display: flex;
    align-items: center;
    gap: 5px;
    margin: 10px 0;

    button {
      width: 30px;
      height: 30px;
      background-color: #f0f0f0;
      border: none;
      font-size: 18px;
      cursor: pointer;
      border-radius: 4px;
    }

    input {
      width: 40px;
      text-align: center;
      border: 1px solid #ccc;
      border-radius: 4px;
    }
  }
}
</style>
