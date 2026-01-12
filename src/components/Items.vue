<template>
  <div class="page">
    <!-- TOP BLOCKS -->
    <div class="top">
      <div class="block">
        <h3>Выбранные вещи пользователя</h3>
        <div class="items">
          <div
              v-for="item in selectedUserItems"
              :key="item.id"
              class="item selected"
              v-text="item.name"
              @click="toggleUserItem(item)"
          />
        </div>
      </div>

      <div class="block">
        <h3>Выбранная вещь</h3>
        <div class="items">
          <div v-if="selectedShopItem"
               class="item selected">
            {{ selectedShopItem.name }}
          </div>
          <div v-else class="empty">Ничего не выбрано</div>
        </div>
      </div>
    </div>

    <!-- BOTTOM BLOCKS -->
    <div class="bottom">
      <div class="block">
        <h3>Вещи пользователя</h3>
        <div class="items">
          <div
              v-for="item in userItems"
              :key="item.id"
              class="item"
              :class="{ selected: isUserItemSelected(item) }"
              @click="toggleUserItem(item)"
          >
            {{ item.name }}
          </div>
        </div>
      </div>

      <div class="block">
        <h3>Вещи на выбор</h3>
        <div class="items">
          <div
              v-for="item in shopItems"
              :key="item.id"
              class="item"
              :class="{ selected: selectedShopItem?.id === item.id }"
              @click="selectShopItem(item)"
          >
            {{ item.name }}
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import {userItems, shopItems} from "../data/data.js";

const selectedUserItems = ref([])
const selectedShopItem = ref(null)

const toggleUserItem = (item) => {
  const index = selectedUserItems.value.findIndex(i => i.id === item.id)

  if (index !== -1) {
    selectedUserItems.value.splice(index, 1)
    return
  }

  if (selectedUserItems.value.length < 6) {
    selectedUserItems.value.push(item)
  }
}

const isUserItemSelected = (item) => {
  return selectedUserItems.value.some(i => i.id === item.id)
}

const selectShopItem = (item) => {
  if (selectedShopItem.value?.id === item.id) {
    selectedShopItem.value = null
  } else {
    selectedShopItem.value = item
  }
}
</script>

<style scoped>
.page {
  padding: 20px;
  font-family: Arial, sans-serif;
}

.top, .bottom {
  display: flex;
  gap: 20px;
  margin-bottom: 30px;
}

.block {
  flex: 1;
  border: 1px solid #ccc;
  padding: 10px;
  border-radius: 10px;
}

.items {
  display: flex;
  flex-wrap: wrap;
  gap: 8px;
}

.item {
  padding: 6px 10px;
  border: 1px solid #999;
  cursor: pointer;
  border-radius: 8px;
  &.selected {
    background-color: #074687;
    border-color: #3399ff;
  }
}


.empty {
  color: #999;
}
</style>
