<template>
  <q-layout view="lHh Lpr lFf">
    <!-- Хедер приложения -->
    <q-header>
      <q-toolbar>
        <q-btn
          flat
          dense
          round
          icon="menu"
          aria-label="Menu"
          @click="toggleLeftDrawer"
        />
      </q-toolbar>

      <!-- q-px - боковые паддинги, q-pt - верхний паддинг, q-mb - нижний отступ -->
      <div class="q-px-lg q-pt-xl q-pb-md">
        <div class="text-h3">Todo</div>
        <div class="text-subtitle1">{{ todayDate }}</div>
      </div>

      <!-- Фоновое изображения в хедере -->
      <q-img src="../assets/header.jpg" class="header-image absolute-top" />
    </q-header>

    <!-- Боковая панель -->
    <q-drawer
      v-model="leftDrawerOpen"
      show-if-above
      :width="250"
      :breakpoint="600"
    >
      <q-scroll-area
        style="
          height: calc(100% - 192px);
          margin-top: 192px;
          border-right: 1px solid #ddd;
        "
      >
        <q-list padding>
          <q-item to="/" exact clickable v-ripple>
            <q-item-section avatar>
              <q-icon name="list" />
            </q-item-section>

            <q-item-section>Todo</q-item-section>
          </q-item>

          <q-item to="/help" exact clickable v-ripple>
            <q-item-section avatar>
              <q-icon name="help" />
            </q-item-section>

            <q-item-section>Help</q-item-section>
          </q-item>
        </q-list>
      </q-scroll-area>

      <!-- Аватар в боковой панели -->
      <q-img
        class="absolute-top"
        src="../assets/header.jpg"
        style="height: 192px"
      >
        <div class="absolute-bottom bg-transparent">
          <q-avatar
            size="56px"
            class="q-mb-sm"
            color="primary"
            text-color="white"
            >D</q-avatar
          >
          <div class="text-weight-bold">Doctor Who</div>
          <div>@nickname</div>
        </div>
      </q-img>
    </q-drawer>

    <q-page-container>
      <!-- Для сохранения состояния или во избежание перерисовки -- пофиксить! -->
      <router-view v-slot="{ Component }"
        ><keep-alive> <component :is="Component" /> </keep-alive
      ></router-view>
    </q-page-container>
  </q-layout>
</template>

<script>
import { date } from "quasar";
import { defineComponent, ref, computed } from "vue";

export default defineComponent({
  name: "MainLayout",

  setup() {
    const leftDrawerOpen = ref(false);

    // Текущий день недели
    const todayDate = computed(() => {
      const timeStamp = Date.now();
      return date.formatDate(timeStamp, "D MMMM (dddd)");
    });

    return {
      leftDrawerOpen,
      toggleLeftDrawer() {
        leftDrawerOpen.value = !leftDrawerOpen.value;
      },
      todayDate,
    };
  },
});
</script>

<style lang="sass" scoped>
.header-image
  height: 100%
  z-index: -1
  opacity: 0.3
  filter: grayscale(100%)
</style>