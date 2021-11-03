<template>
  <q-page class="flex flex-center row">
    <div class="col-4">
      <!-- Hello {{ user }} -->
      <div class="q-py-md">
        <q-input
          outlined
          bottom-slots
          v-model="todoinput"
          ref="input"
          label="Enter Todo name"
          counter
          v-on:keyup.enter="submittodo"
        >
          <template v-slot:append>
            <q-icon name="send" class="cursor-pointer" @click="submittodo" />
          </template>
        </q-input>
      </div>

      <q-scroll-area
        :thumb-style="thumbStyle"
        visible
        :bar-style="barStyle"
        class="full-width q-pr-xs"
        style="height: 300px; width: 100%"
      >
        <ul class="todo-list">
          <li
            v-for="(list, index) in todolists"
            :key="index"
            :class="{ completed: list.compelete }"
          >
            <q-checkbox v-model="list.compelete" @click="sessionupdate()" />
            <strong>{{ list.name }}</strong>
            <span class="d-block block compelete" v-if="list.compelete"
              >completed</span
            >
            <button class="">
              <q-icon
                name="close"
                class="cursor-pointer"
                @click="removetodo(index)"
              />
            </button>
          </li>
          <li v-if="!todolists.length" class="notodo">
            You don't have todo item
          </li>
        </ul>
      </q-scroll-area>
    </div>
  </q-page>
</template>

<script>
import { defineComponent } from "vue";
import { mapGetters } from "vuex";
export default defineComponent({
  name: "PageIndex",
  data() {
    return {
      id: 0,
      todolists: JSON.parse(sessionStorage.getItem("todolist")) || [],
      todoinput: "",
      thumbStyle: {
        right: "4px",
        borderRadius: "5px",
        backgroundColor: "#027be3",
        width: "5px",
        opacity: 0.75,
      },

      barStyle: {
        right: "2px",
        borderRadius: "9px",
        backgroundColor: "#027be3",
        width: "9px",
        opacity: 0.2,
      },
    };
  },
  computed: {
    ...mapGetters("moduleExample", ["user"]),
  },
  methods: {
    submittodo() {
      if (this.todoinput != "") {
        this.todolists.push({
          id: this.id++,
          name: this.todoinput,
          compelete: false,
        });
        this.sessionupdate();
        this.todoinput = "";
      }
    },
    removetodo(index) {
      this.todolists.splice(index, 1);
      this.sessionupdate();
    },

    sessionupdate() {
      sessionStorage.setItem("todolist", JSON.stringify(this.todolists));
    },
  },
});
</script>
<style lang="scss" scoped>
.todo-list {
  list-style-type: none;
  padding: 0;
  margin: 0;
  width: 100%;
  overflow: auto;
  li {
    position: relative;
    // min-height: 28px;
    padding: 10px 75px 10px 10px;
    border: 1px solid #eee;
    box-shadow: 0 0 9px #e1e1e1;

    &.notodo {
      line-height: 40px;
      min-height: 40px;
    }
    & + li {
      // border-top: 1px solid #eee;
      margin-top: 10px;
    }
    button {
      position: absolute;
      right: 5px;
      top: 10px;
      padding: 0;
      border: none;
      i {
        padding: 10px;
        border: 1px solid #ddd;
      }
    }
    .completed-todo {
      right: 35px;
    }
    &.completed {
      background: #dbffdb;

      > strong {
        opacity: 0.3;
      }
      > .q-checkbox__inner {
        color: green;
      }
      span {
        font-size: 10px;
        color: green;
        font-weight: bold;
      }
    }
  }
}
</style>
