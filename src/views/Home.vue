<template>
  <div class="home fade-in">
    <b-container class="daily-container">
      <b-row class="cards">
        <b-col cols="12" md="6" lg="3" class="card-column">
          <div class="card-container">
            <h2 class="underline-blue">Daily</h2>
            <div class="tasks">
              <p class="task">Life before death</p>
              <p class="task">Strength before weakness</p>
              <p class="task">Journey before destination</p>
            </div>
          </div>
        </b-col>

        <template v-for="(card, i) in cards">
          <b-col cols="12" md="6" lg="3" :key="i" class="card-column">
            <div class="card-container">
              <h2 :class="'underline-' + card.color">{{ card.title }}</h2>
              <div class="tasks">
                <template v-for="(task, j) in card.todo">
                  <label :for="card.title + j" :key="j" class="task">
                    <input
                      type="checkbox"
                      :name="card.title"
                      :id="card.title + j"
                      @change="checked(card.title + j)"
                    />
                    {{ task }}
                  </label>
                </template>
              </div>
            </div>
          </b-col>
        </template>

        <b-col cols="12" class="reset-container">
          <button @click="reset">reset</button>
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
const response = [
  {
    title: "Domingo",
    color: "red",
    todo: ["Inglés", "Programación", "Lectura", "Dibujo"],
  },
  {
    title: "Lunes",
    color: "purple",
    todo: ["Francés", "Programación", "Ejercicio", "Lectura"],
  },
  {
    title: "Martes",
    color: "yellow",
    todo: ["Inglés", "Programación", "Ejercicio", "Dibujo"],
  },
  {
    title: "Miércoles",
    color: "blue",
    todo: ["Francés", "Programación", "Ejercicio", "Lectura"],
  },
  {
    title: "Jueves",
    color: "red",
    todo: ["Inglés", "Programación", "Ejercicio", "Dibujo"],
  },
  {
    title: "Viernes",
    color: "purple",
    todo: ["Francés", "Programación", "Ejercicio", "Lectura"],
  },
  {
    title: "Sábado",
    color: "yellow",
    todo: ["Inglés", "Programación", "Lectura", "Dibujo"],
  },
];

export default {
  name: "Home",
  data() {
    return {
      cards: response,
      inputs: Object,
      activities: [],
    };
  },
  mounted() {
    this.inputs = document.querySelectorAll("input[type='checkbox']");
    this.inputs.forEach((input) => this.activities.push(input.id));
    this.inputs.forEach((input) => this.getFromStorage(input.id));
  },
  methods: {
    getFromStorage(id) {
      if (window.localStorage.getItem(id) !== null) {
        const checkbox = document.getElementById(id);
        checkbox.checked = true;
        checkbox.parentNode.classList.add("checked");
      }
    },
    checked(id) {
      const checkbox = document.getElementById(id);
      if (checkbox.checked) {
        checkbox.parentNode.classList.add("checked");
        window.localStorage.setItem(
          id,
          JSON.stringify({ id: id, checked: checkbox.checked })
        );
      } else {
        checkbox.parentNode.classList.remove("checked");
        window.localStorage.removeItem(id);
      }
    },
    reset() {
      window.localStorage.clear();
      this.inputs.forEach((input) => {
        const checkbox = document.getElementById(input.id);
        checkbox.checked = false;
        checkbox.parentNode.classList.remove("checked");
      });
    },
  },
};
</script>

<style lang="scss" scoped>
@import "@/assets/scss/style.scss";
@import "@/assets/scss/mixins.scss";
@import "@/assets/scss/variables.scss";

.home {
  .daily-container {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    padding-top: 1rem;

    .cards {
      justify-content: center;

      .card-column {
        padding: 0.5rem;
        background-color: $primary;
        color: $softGray;

        .card-container {
          height: 100%;
          border: 1px solid $softGray;
          padding: 0.5rem 1rem;

          h2 {
            color: $softGray;
            font-size: 1.5rem;
            margin-bottom: 1.5rem;
          }

          .tasks {
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: flex-start;

            .task {
              width: 100%;
              color: $softGray;
              margin-bottom: auto;
              position: relative;
              &:after {
                position: absolute;
                background-color: $gray;
                content: "";
                height: 2px;
                width: 0;
                left: 1rem;
                bottom: 0.6rem;
                transition: all 0.35s ease;
              }
              &:hover {
                background-color: rgba(255, 255, 255, 0.1);
              }
            }

            .checked {
              color: $gray;
              &:after {
                width: calc(100% - 1rem);
              }
            }
          }
        }
      }

      .reset-container {
        padding: 1rem;
        display: flex;
        justify-content: center;
        align-items: center;

        button {
          @include button-transparent($purple);
          background-color: $primary;
          color: $softGray;
        }
      }
    }
  }
}
</style>
