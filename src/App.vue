<template>
  <div id="app">
    <p>Profiles List</p>
    <div class="section">
      <div class="flex-row input-wrapper">
        <label class="label" for="filter">Find profile:</label>
        <input id="filter" v-model="filter" class="input" />
      </div>
      <div class="options">
        <div class="buttons">
          <button @click="sortAsc">▲</button>
          <button @click="sortDesc">▼</button>
        </div>

        <button @click="toggleNewProfile" class="buttons add-profile-btn">
          add new profile
        </button>
      </div>

      <TransitionGroup name="list" tag="div">
        <ProfileCard
          v-for="profile in filteredProfiles"
          :key="profile.id"
          :profile="profile"
          @toggle-like="toggleLike"
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

    <AddNewProfile
      :profiles="profiles"
      ref="addNewProfile"
      @addProfile="addProfile"
    />
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
      errorMessages: [],
      validations: { emailIsValid: false, nameIsValid: false },
      profiles: [
        {
          id: 1,
          name: "Wojciech",
          email: "wojciech@poz.pl",
          description: "Anaesthesiologist",
          likes: 34,
          isLiked: false,
        },
        {
          id: 2,
          name: "Maria",
          email: "maria@poz.pl",
          description: "Radiologist",
          likes: 28,
          isLiked: false,
        },
        {
          id: 3,
          name: "Anna",
          email: "anna@poz.pl",
          description: "Surgeon",
          likes: 53,
          isLiked: false,
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
    toggleLike(userProfile) {
      this.profiles.map((profile) => {
        if (profile.id === userProfile.id) {
          if (!profile.isLiked) {
            profile.isLiked = true;
            profile.likes++;
          } else if (profile.isLiked && profile.likes > 1) {
            profile.isLiked = false;
            profile.likes = profile.likes - 2;
          } else {
            profile.isLiked = false;
            profile.likes = 0;
          }
        }
      });
    },
    toggleNewProfile() {
      this.$refs.addNewProfile.toggleModal();
    },
    addProfile(newProfile) {
      this.profiles.push({
        id: this.profiles.length + 1,
        ...newProfile,
        likes: 0,
      });
      this.$refs.addNewProfile.toggleModal();
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
.input-wrapper {
  overflow: hidden;
  border-radius: 4px;
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
  padding-left: 0;
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
.options {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 34px;
}
button {
  transition: all 0.2s;
}
button:hover {
  box-shadow: none;
  transform: scale(0.98);
}
.add-profile-btn {
  text-align: center;
  justify-content: center;
}
</style>
