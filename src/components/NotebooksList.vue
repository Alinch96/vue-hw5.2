<template>
  <ul class="notebooks-list">
    <notebook-item
      v-for="notebook in notebooks"
      :key="notebook.id"
      :notebook="notebook"
      type="ordinaryList"
      @add="$emit('add', notebook)"
    />
  </ul>
</template>

<script>
import NotebookItem from "./NotebookItem.vue";
export default {
  name: "NotebooksList",
  components: {
    NotebookItem,
  },
  props: {
    notebooks: {
      type: Array,
      default: () => [],
    },
  },
};
</script>

<style lang="scss" scoped>
.notebooks-list {
  width: 70%;
  display: flex;
  flex-direction: column;
  gap: 20px;
  list-style: none;
}
</style>