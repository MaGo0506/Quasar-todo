<template>
  <q-page class="bg-grey-3 column">
    <div class="row q-pa-sm bg-primary">
      <q-input
        v-model="newTask"
        @keyup.enter="addTask"
        class="col q-pb-0"
        filled
        square
        bg-color="white"
        placeholder="Add Task"
        dense
      >
        <template v-slot:append>
          <q-btn
            @click="addTask"
            round dense
            flat
            icon="add"
          />
        </template>
      </q-input>
    </div>
    <q-list
      class="bg-white"
      separator
      bordered
    >
      <q-item
        :class="{'done bg-blue-1' : task.done}"
        v-for="(task, i) in tasks"
        :key="i"
        @click="task.done = !task.done"
        clickable
        v-ripple
      >
        <q-item-section avatar>
          <q-checkbox
            v-model="task.done"
            class="no-pointer-events"
            color="primary"
          />
        </q-item-section>
        <q-item-section>
          <q-item-label v-text="task.title"/>
        </q-item-section>
        <q-item-section
          v-if="task.done"
          side
        >
          <q-btn
            @click.stop="deleteTask(i)"
            flat
            round
            color="primary"
            dense
            icon="delete"
          />
        </q-item-section>
      </q-item>
    </q-list>
    <div
      class="no-tasks absolute-center"
      v-if="!tasks.length"
    >
      <q-icon
        name="check"
        size="100px"
        color="primary"
      />
      <div class="text-h5 text-primary text-center" v-text="`No Tasks`"/>
    </div>
  </q-page>
</template>

<script>
import {defineComponent, reactive, ref} from 'vue'
import {useQuasar} from "quasar";

export default defineComponent({
  setup() {
    const $q = useQuasar(),
      newTask = ref(''),
      tasks = reactive([]);

    const deleteTask = (index) => {
        $q.dialog({
          title: 'Confirm',
          message: 'Really delete?',
          cancel: true,
          persistent: true
        }).onOk(() => {
          tasks.splice(index, 1);
          $q.notify('Task deleted');
        })
      },

      addTask = () => {
        tasks.push({
          title: newTask.value,
          done: false
        });
        newTask.value = '';
      }

    return {
      tasks,
      deleteTask,
      newTask,
      addTask
    }
  }
})
</script>

<style lang="scss">
.done {
  .q-item__label {
    text-decoration: line-through;
    color: #bbb;
  }
}

.no-tasks {
  opacity: 0.5;
}

</style>
