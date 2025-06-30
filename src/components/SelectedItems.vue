<template>
  <div class="selected-items">
    <div class="selected-items__header">
      <h3 class="selected-items__title">{{ title }}</h3>
      <span class="selected-items__counter">{{ items.length }}/{{ maxItems }}</span>
    </div>
    
    <p class="selected-items__subtitle">{{ subtitle }}</p>
    
    <div 
      class="selected-items__list"
      :class="{ 'selected-items__list--empty': !items.length }"
    >
      <div 
        v-if="!items.length" 
        class="selected-items__empty"
      >
        {{ placeholder }}
      </div>
      
      <Item
        v-for="(item, index) in items"
        :key="item.id"
        :item="item"
        :show-remove="true"
        class="selected-items__item"
        @remove="$emit('item-removed', index)"
        @drag-start="handleDragStart(index, $event)"
        @dragover.prevent
        @drop="handleDrop(index, $event)"
      />
    </div>
  </div>
</template>

<script setup>
import Item from './Item.vue';

const props = defineProps({
  items: {
    type: Array,
    default: () => []
  },
  title: {
    type: String,
    default: 'Выбранные элементы'
  },
  subtitle: {
    type: String,
    default: ''
  },
  placeholder: {
    type: String,
    default: 'Ничего не выбрано'
  },
  maxItems: {
    type: Number,
    default: 6
  }
});

const emit = defineEmits(['item-removed', 'items-reordered']);

const handleDragStart = (index, event) => {
  event.dataTransfer.setData('text/plain', index);
};

const handleDrop = (dropIndex, event) => {
  const dragIndex = event.dataTransfer.getData('text/plain');
  if (dragIndex !== '') {
    emit('items-reordered', parseInt(dragIndex), dropIndex);
  }
};
</script>

<style lang="scss">
.selected-items {
  &__header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 8px;
  }

  &__title {
    margin: 0;
    font-size: 16px;
    font-weight: 600;
    color: #333;
  }

  &__counter {
    font-size: 14px;
    color: #666;
    background: #f0f0f0;
    padding: 2px 8px;
    border-radius: 12px;
  }

  &__subtitle {
    margin: 0 0 12px;
    font-size: 13px;
    color: #666;
  }

  &__list {
    min-height: 120px;
    padding: 8px;
    display: flex;
    align-items: baseline;
    flex-wrap: wrap;
    gap: 8px;
    border-radius: 8px;
    background: #fafafa;
    border: 1px dashed #e0e0e0;

    &--empty {
      display: flex;
      align-items: center;
      justify-content: center;
    }
  }

  &__empty {
    color: #999;
    font-size: 14px;
    text-align: center;
    padding: 16px;
  }

  &__item {
    margin-bottom: 8px;
    
    &:last-child {
      margin-bottom: 0;
    }
  }
}
</style>