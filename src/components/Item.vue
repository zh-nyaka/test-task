<template>
  <div 
    class="item"
    :class="{
      'item--selected': isSelected,
      'item--draggable': draggable
    }"
    draggable="true"
    @dragstart="$emit('drag-start', $event)"
    @click="$emit('click')"
  >
    <span class="item__name">{{ item.name }}</span>
    
    <button 
      v-if="showRemove"
      class="item__remove"
      @click.stop="$emit('remove')"
      aria-label="Удалить"
    >
      <svg class="item__remove-icon" viewBox="0 0 24 24">
        <path d="M19 6.41L17.59 5 12 10.59 6.41 5 5 6.41 10.59 12 5 17.59 6.41 19 12 13.41 17.59 19 19 17.59 13.41 12z"/>
      </svg>
    </button>
  </div>
</template>

<script setup>
defineProps({
  item: {
    type: Object,
    required: true
  },
  showRemove: {
    type: Boolean,
    default: false
  },
  isSelected: {
    type: Boolean,
    default: false
  },
  draggable: {
    type: Boolean,
    default: true
  }
});

defineEmits(['click', 'remove', 'drag-start']);
</script>

<style lang="scss">
.item {
  position: relative;
  padding: 12px 40px 12px 16px;
  background: #f8f8f8;
  border-radius: 8px;
  cursor: pointer;
  transition: all 0.25s ease;
  display: flex;
  align-items: center;
  border: 1px solid #e0e0e0;
  margin: 4px 0;

  &--selected {
    opacity: 0.6;
    pointer-events: none;
  }

  &--draggable {
    cursor: grab;
  }

  &:hover {
    background: #f0f0f0;
    border-color: #ccc;
  }

  &__name {
    flex-grow: 1;
    font-size: 14px;
    color: #333;
  }

  &__remove {
    position: absolute;
    right: 8px;
    top: 50%;
    transform: translateY(-50%);
    width: 24px;
    height: 24px;
    display: flex;
    align-items: center;
    justify-content: center;
    background: none;
    border: none;
    border-radius: 50%;
    cursor: pointer;
    padding: 0;
    transition: all 0.2s ease;
    opacity: 0.5;

    &:hover {
      opacity: 1;
      background: #ffebee;
    }
  }

  &__remove-icon {
    width: 18px;
    height: 18px;
    fill: #f44336;
  }
}
</style>