<template>
  <div id="app">
    <p>Profiles List</p>
    <div class="section">
      <div class="flex-row">
        <label class="label" for="filter">Find profile:</label>
        <input id="filter" v-model="filter" class="input" />
      </div>
      <div class="buttons">
        <button @click="sortAsc">▲</button>
        <button @click="sortDesc">▼</button>
      </div>

      <TransitionGroup name="list" tag="div">
        <ProfileCard
          v-for="profile in filteredProfiles"
          :key="profile.id"
          :profile="profile"
          class="profile"
        />
      </TransitionGroup>

      <div class="icons-note">
        Icons made by
        <a href="https://www.flaticon.com/authors/freepik" title="Freepik"
          >Freepik
        </a>
        from
        <a href="https://www.flaticon.com/" title="Flaticon">
          www.flaticon.com
        </a>
      </div>
    </div>

    <AddNewProfile @addProfile="addProfile" />
  </div>
</template>

<script>
import AddNewProfile from "./components/AddNewProfile.vue";
import ProfileCard from "./components/ProfileCard";

export default {
  name: "App",

  components: {
    ProfileCard,
    AddNewProfile,
  },

  data() {
    return {
      filter: "",

      selectedSpecializations: ["Radiologist"],
      errorMessages: [],
      validations: { emailIsValid: false, nameIsValid: false },
      profiles: [
        {
          id: 1,
          name: "Wojciech",
          email: "wojciech@poz.pl",
          description: "Anaesthesiologist",
          likes: 34,
        },
        {
          id: 2,
          name: "Maria",
          email: "maria@poz.pl",
          description: "Radiologist",
          likes: 28,
        },
        {
          id: 3,
          name: "Anna",
          email: "anna@poz.pl",
          description: "Surgeon",
          likes: 53,
        },
      ],
    };
  },

  computed: {
    filteredProfiles() {
      return this.profiles.filter((profile) =>
        profile.name.toLowerCase().includes(this.filter.toLowerCase())
      );
    },
  },

  methods: {
    addProfile(newProfile) {
      this.profiles.push({
        id: this.profiles.length + 1,
        ...newProfile,
        likes: 0,
      });
      console.log(newProfile);
    },
    sortAsc() {
      this.profiles.sort(function (a, b) {
        return a.likes - b.likes;
      });
    },

    sortDesc() {
      this.profiles.sort(function (a, b) {
        return b.likes - a.likes;
      });
    },
  },
};
</script>

<style>
#app {
  font-family: "Roboto", helvetica, arial, sans-serif;
  text-align: center;
  color: #2c3e50;
  padding: 60px;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
  position: relative;

  background: linear-gradient(
    135deg,
    rgba(65, 184, 131, 0.9),
    rgba(52, 73, 94, 0.9)
  );

  font-size: 1.5em;
}

button {
  display: block;
  padding: 1em;
  width: 100%;
  background-color: #41b883;
  border: 1px solid;
  color: #fff;
  cursor: pointer;
  font-size: 0.75em;
  font-weight: 600;
  text-shadow: 0 1px 0 rgba(black, 0.2);
}

.content {
  display: flex;
}

.section {
  width: 100%;
  min-width: 300px;
  padding: 2em;
  margin-top: 30px;
  position: relative;
  background: rgba(0, 0, 0, 0.15);
}

@media screen and (min-width: 600px) {
  .section {
    width: 50vw;
    max-width: 15em;
  }
}

.header {
  color: #fff;
}

.section::before {
  content: "";
  position: absolute;
  top: -2px;
  left: 0;
  height: 2px;
  width: 100%;
  background: #35c3c1;
}

.flex-row {
  display: flex;
  margin-bottom: 1em;
}

.label {
  width: 80px;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 12px;

  background: #f5f6f8;
}

.input {
  flex: 1;
  padding: 1em;
  border: 0;
  color: #8f8f8f;
  font-size: 1rem;
  outline: none;
}

.buttons {
  display: flex;
  box-shadow: 0 10px 10px rgba(0, 0, 0, 0.25), 0 5px 5px rgba(0, 0, 0, 0.22);
  margin-top: 30px;
}

.profile {
  margin-top: 20px;
}

.icons-note {
  margin-top: 30px;
  font-size: 10px;
}

.list-move,
.list-enter-active,
.list-leave-active {
  transition: all 0.5s ease;
}

.list-enter-from,
.list-leave-to {
  opacity: 0;
  transform: translateX(30px);
}

.list-leave-active {
  position: absolute;
}

.error-message {
  text-align: left !important;
  margin: -12px 0 16px 8px;
  font-size: 14px;
  color: #ef4444;
  animation: errorFadeInDown 0.5s cubic-bezier(0.25, 0.8, 0.5, 1);
}

.input-error {
  border: 1px solid #ef4444;
}

@keyframes errorFadeInDown {
  0% {
    -webkit-transform: translate3d(0, -10px, 0);
    transform: translate3d(0, -10px, 0);
  }

  59% {
    opacity: 1;
    transform: skewX(20deg);
  }
  70%,
  90% {
    transform: skewX(-20deg);
  }
  100% {
    -webkit-transform: none;
    transform: none;
  }
}
</style>
