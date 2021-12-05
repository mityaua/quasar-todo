<template>
  <!-- Задаем фон для страницы -->
  <q-page class="bg-grey-3 column">
    <div class="row q-pa-sm bg-primary">
      <q-input
        v-model.trim="inputText"
        @keyup.enter="addTask"
        class="col"
        bg-color="white"
        placeholder="Add task"
        maxlength="120"
        counter
        square
        filled
        dense
      >
        <template v-slot:hint> Field length </template>

        <!-- Кнопка на инпуте -->
        <template v-slot:append>
          <q-btn round dense flat icon="add" @click="addTask" />
        </template>
      </q-input>
    </div>

    <!-- Фон для списка + разделители + границы -->
    <q-list class="bg-white" separator bordered>
      <!-- Иттерация по списку тасок -->
      <q-item
        v-ripple
        v-for="(task, index) in tasks"
        :key="task.title"
        :class="{ 'done bg-green-1': task.done }"
        @click="toggleTask(task)"
        clickable
      >
        <!-- Чекбокс -->
        <q-item-section avatar>
          <q-checkbox
            v-model="task.done"
            color="primary"
            class="no-pointer-events"
          />
        </q-item-section>

        <!-- Тайтл таски -->
        <q-item-section>
          <q-item-label>{{ task.title }}</q-item-label>
        </q-item-section>

        <!-- Кнопка удаления таски -->
        <q-item-section v-if="task.done" side>
          <q-btn
            flat
            round
            dense
            color="negative"
            icon="delete"
            @click.stop="deleteTask(index)"
        /></q-item-section>
      </q-item>
    </q-list>

    <div v-if="!tasks.length" class="no-tasks absolute-center">
      <q-icon name="check" size="100px" color="primary" />

      <div class="text-h4 text-primary">No tasks</div>
    </div>
  </q-page>
</template>

<script>
import { useQuasar } from "quasar";
import { defineComponent, ref } from "vue";

export default defineComponent({
  name: "TodoPage",
  setup() {
    const $q = useQuasar();

    // Переменная для инпута
    const inputText = ref("");

    // Массив тасок
    const tasks = ref([]);

    // Тоглит таски
    const toggleTask = (task) => (task.done = !task.done);

    // Добавляет новую таску
    const addTask = () => {
      if (!inputText.value) {
        $q.notify({
          message: "Please fill out this field",
          color: "warning",
          textColor: "dark",
          icon: "warning",
        });

        return;
      }

      tasks.value.unshift({
        title: inputText.value,
        done: false,
      });

      $q.notify({
        message: "Successfully added",
        color: "primary",
        icon: "done",
      });

      inputText.value = "";
    };

    // Удаляет таску
    const deleteTask = (index) => {
      $q.dialog({
        title: "Confirm",
        message: "Really delete?",
        cancel: true,
        persistent: true,
      }).onOk(() => {
        tasks.value.splice(index, 1);
        $q.notify({
          message: "Task deleted",
          color: "positive",
          icon: "delete_sweep",
        });
      });
    };

    return { inputText, tasks, addTask, toggleTask, deleteTask };
  },
});
</script>

<style lang="scss" scoped>
.done {
  .q-item__label {
    text-decoration: line-through;
    color: #bbb;
  }
}

.no-tasks {
  opacity: 0.5;
  text-align: center;
}
</style>
