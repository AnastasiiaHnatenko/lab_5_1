<template>
  <v-app id="inspire">
    <!-- Навігаційний Drawer -->
    <v-navigation-drawer v-model="drawer" app :right="right">
      <v-list>
        <v-list-item>
          <v-list-item-content>
            <v-list-item-title>Shopping List</v-list-item-title>
          </v-list-item-content>
        </v-list-item>

        <v-divider></v-divider>

        <!-- Список покупок -->
        <v-list>
          <div v-for="item in shoppingList" :key="item.id">
            <v-list-item @click="doneBought(item.id)" :class="{ 'blue lighten-5': item.bought }">
              <v-list-item-action>
                <v-checkbox :input-value="item.bought" :label="item.title" />
              </v-list-item-action>

              <v-list-item-content>
                <v-list-item-title :class="{ 'text-decoration-line-through': item.bought }">
                  {{ item.title }}
                </v-list-item-title>
              </v-list-item-content>

              <!-- Кнопка видалення справа -->
              <v-list-item-action>
                <v-btn
                  icon
                  variant="text"
                  @click.stop="deleteItem(item.id)"
                >
                  <v-icon>mdi-delete</v-icon>
                </v-btn>
              </v-list-item-action>
            </v-list-item>

            <v-divider />
          </div>
        </v-list>
      </v-list>
    </v-navigation-drawer>

    <!-- Верхній бар -->
    <v-app-bar app fixed :elevation="2">
      <template v-slot:prepend>
        <v-app-bar-nav-icon @click="drawer = !drawer" />
      </template>
      <v-app-bar-title>Shopping List App</v-app-bar-title>
    </v-app-bar>

    <v-main>
      <!-- Поле для введення нового елемента -->
      <v-text-field
        v-model="newItemTitle"
        label="New item"
        append-icon="mdi-plus"
        clearable
        @click:append="addNewItem"
        @keyup.enter="addNewItem"
      />

      <!-- Список покупок -->
      <v-list>
        <div v-for="item in shoppingList" :key="item.id">
          <v-list-item @click="doneBought(item.id)" :class="{ 'blue lighten-5': item.bought }">
            <v-list-item-action>
              <v-checkbox :input-value="item.bought" :label="item.title" />
            </v-list-item-action>

            <v-list-item-content>
              <v-list-item-title :class="{ 'text-decoration-line-through': item.bought }">
                {{ item.title }}
              </v-list-item-title>
            </v-list-item-content>

            <!-- Кнопка видалення справа -->
            <v-list-item-action>
              <v-btn
                icon
                variant="text"
                @click.stop="deleteItem(item.id)"
              >
                <v-icon>mdi-delete</v-icon>
              </v-btn>
            </v-list-item-action>
          </v-list-item>

          <v-divider />
        </div>
      </v-list>
    </v-main>
  </v-app>
</template>

<script setup>
import { ref } from 'vue'

const drawer = ref(false)
const right = ref(false)

// Масив покупок
const shoppingList = ref([
  { id: 1, title: 'Apples', bought: false },
  { id: 2, title: 'Bananas', bought: false },
  { id: 3, title: 'Carrots', bought: false },
])

// Змінна для введення нового елемента
const newItemTitle = ref('')

// Метод для зміни статусу покупки
function doneBought(id) {
  const item = shoppingList.value.find(i => i.id === id)
  if (item) {
    item.bought = !item.bought
  }
}

// Метод для додавання нового елемента
function addNewItem() {
  const title = newItemTitle.value.trim()
  if (title !== '') {
    const newId = shoppingList.value.length > 0
      ? Math.max(...shoppingList.value.map(i => i.id)) + 1
      : 1

    shoppingList.value.push({
      id: newId,
      title: title,
      bought: false,
    })

    newItemTitle.value = '' // Очищаємо поле після додавання
  }
}

// Метод для видалення елемента
function deleteItem(id) {
  shoppingList.value = shoppingList.value.filter(item => item.id !== id)
}
</script>
