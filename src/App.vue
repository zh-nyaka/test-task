<template>
  <div class="items-selector">
    <!-- Блок ошибок -->
    <div v-if="error" class="items-selector__error">
      {{ error }}
      <button 
        class="items-selector__error-close"
        @click="error = ''"
      >×</button>
    </div>

    <!-- Верхние блоки -->
    <div class="items-selector__top-section">
      <SelectedItems
        class="items-selector__block items-selector__block--left"
        title="Выбранные вещи"
        subtitle="Максимум 6 вещей"
        :items="selectedUserItems"
        :max-items="6"
        @item-removed="removeUserItem"
        @items-reordered="reorderUserItems"
        @error="handleError"
      />
      
      <SelectedItems
        class="items-selector__block items-selector__block--right"
        title="Выбранная вещь"
        subtitle="Максимум 1 вещь"
        :items="selectedAvailableItem ? [selectedAvailableItem] : []"
        :max-items="1"
        placeholder="Ничего не выбрано"
        @item-removed="removeAvailableItem"
        @error="handleError"
      />
    </div>

    <!-- Нижние блоки -->
    <div class="items-selector__bottom-section">
      <ItemsList
        class="items-selector__block items-selector__block--left"
        title="Вещи пользователя"
        :items="userItems"
        type="user"
        :selected-items="selectedUserItems"
        @item-selected="selectUserItem"
        @drag-start="handleDragStart"
      />
      
      <ItemsList
        class="items-selector__block items-selector__block--right"
        title="Вещи на выбор"
        :items="availableItems"
        type="available"
        :selected-items="selectedAvailableItem ? [selectedAvailableItem] : []"
        @item-selected="selectAvailableItem"
        @drag-start="handleDragStart"
      />
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue';
import ItemsList from './components/ItemsList.vue';
import SelectedItems from './components/SelectedItems.vue';

const userItems = ref([
  { id: 1, name: "Shoes 1" },
  { id: 2, name: "Shoes 2" },
  { id: 3, name: "Shoes 3" },
  { id: 4, name: "Shoes 4" },
  { id: 5, name: "T-shirt 1" },
  { id: 6, name: "T-shirt 2" },
  { id: 7, name: "T-shirt 3" },
  { id: 8, name: "T-shirt 4" },
]);

const availableItems = ref([
  { id: 11, name: "Jacket 1" },
  { id: 12, name: "Jacket 2" },
  { id: 13, name: "Jacket 3" },
  { id: 14, name: "Jacket 4" },
  { id: 15, name: "Hoodie 1" },
  { id: 16, name: "Hoodie 2" },
  { id: 17, name: "Hoodie 3" },
  { id: 18, name: "Hoodie 4" },
]);

const selectedUserItems = ref([]);
const selectedAvailableItem = ref(null);

const selectUserItem = (item) => {
  if (selectedUserItems.value.length >= 6) return;
  if (!selectedUserItems.value.some((i) => i.id === item.id)) {
    selectedUserItems.value.push(item);
  }
};

const removeUserItem = (index) => {
  selectedUserItems.value.splice(index, 1);
};

const reorderUserItems = (fromIndex, toIndex) => {
  const items = [...selectedUserItems.value];
  const [movedItem] = items.splice(fromIndex, 1);
  items.splice(toIndex, 0, movedItem);
  selectedUserItems.value = items;
};


const selectAvailableItem = (item) => {
  selectedAvailableItem.value = item;
};

const removeAvailableItem = () => {
  selectedAvailableItem.value = null;
};

const handleDragStart = (item, type) => {
  if (type === "user") {
    selectUserItem(item);
  } else if (type === "available") {
    selectAvailableItem(item);
  }
};
</script>

<style lang="scss">
.items-selector {
  &__container {
    display: flex;
    flex-direction: column;
    gap: 20px;
    padding: 20px;
    font-family: Arial, sans-serif;
  }
  &__top-section {
    margin-bottom: 40px;
  }
  &__top-section,
  &__bottom-section {
    display: grid;
    grid-template-columns: repeat(2, 1fr);
    width: 100%;
    gap: 20px;
  }

  &__block {
    flex: 1;
    border: 1px solid #ccc;
    padding: 15px;
    border-radius: 5px;
  }

  &__error {
    position: fixed;
    top: 20px;
    left: 50%;
    transform: translateX(-50%);
    background-color: #ff4444;
    color: white;
    padding: 10px 20px;
    border-radius: 5px;
    display: flex;
    align-items: center;
    gap: 10px;
    z-index: 1000;
    box-shadow: 0 2px 10px rgba(0, 0, 0, 0.2);

    &-close {
      background: none;
      border: none;
      color: white;
      cursor: pointer;
      font-size: 16px;
    }
  }
}
</style>