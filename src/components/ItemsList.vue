<template>
  <div class="items-list">
    <h3 class="items-list__title">{{ title }}</h3>
    
    <div class="items-list__grid">
      <Item
        v-for="item in items"
        :key="item.id"
        :item="item"
        :is-selected="isSelected(item)"
        class="items-list__item"
        @click="$emit('item-selected', item)"
        @drag-start="$emit('drag-start', item, type)"
      />
    </div>
  </div>
</template>

<script setup>
import Item from './Item.vue';

const props = defineProps({
  items: {
    type: Array,
    required: true
  },
  title: {
    type: String,
    default: 'Список элементов'
  },
  type: {
    type: String,
    default: ''
  },
  selectedItems: {
    type: Array,
    default: () => []
  }
});

const isSelected = (item) => {
  return props.selectedItems.some(selected => selected.id === item.id);
};
</script>

<style lang="scss">
.items-list {
  &__title {
    margin: 0 0 16px;
    font-size: 16px;
    font-weight: 600;
    color: #333;
  }

  &__grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(150px, 1fr));
    gap: 12px;
  }

  &__item {
    transition: transform 0.2s ease;
    
    &:hover {
      transform: translateY(-2px);
    }
  }
}
</style>