<template>
  <div class="profile">
    <div class="card">
      <DoctorIcon class="avatar" />
      <div class="data">
        <div class="profile-content">
          <div>
            <strong>{{ profile.name }}</strong>
            <a :href="profile.email" class="email">{{ profile.email }}</a>
          </div>

          <div v-if="Array.isArray(profile.description)" class="description">
            {{ profile.description.join(", ") }}
          </div>

          <div v-else class="description">
            {{ profile.description }}
          </div>
        </div>
        <div class="likes">
          <div @click="toggleLike(profile)" class="heart-and-animated-heart">
            <div ref="heart" class="heart"></div>
            <div ref="animatedHeart" class="animation-heart"></div>
          </div>

          <span class="likes-value">{{ profile.likes }}</span>
        </div>
      </div>
    </div>
    <div class="comment">
      <input class="comment-input" placeholder="Write your comment..." />
    </div>
  </div>
</template>

<script>
import DoctorIcon from "./DoctorIcon";

export default {
  name: "ProfileCard",

  components: {
    DoctorIcon,
  },

  methods: {
    toggleLike(profile) {
      this.$refs.heart.classList.toggle("fill-color");
      this.$refs.animatedHeart.classList.toggle("animation");
      this.$emit("toggle-like", profile);
    },
  },

  props: {
    profile: {
      type: Object,
      required: true,
    },
  },
};
</script>

<style>
.card {
  display: flex;
  align-items: center;
  border: solid 1px rgb(172, 172, 172);
  border-radius: 3px;
  padding: 10px;
  font-size: 14px;
  color: rgb(82, 82, 82);
  background-color: #fff;
  box-shadow: 0 14px 28px rgba(0, 0, 0, 0.25), 0 10px 10px rgba(0, 0, 0, 0.22);
}

.avatar {
  width: 64px;
  min-width: 64px;
  height: 64px;
  border: 1px solid rgb(172, 172, 172);
  border-radius: 100%;
  box-shadow: 0 10px 10px rgba(0, 0, 0, 0.25), 0 5px 5px rgba(0, 0, 0, 0.22);
}

.data {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
}

.profile-content {
  display: flex;
  flex-direction: column;
  text-align: left;
  margin-left: 15px;
}

.email {
  margin-left: 15px;
}

.description {
  margin-top: 5px;
}

.likes-value {
  margin-left: 5px;
  margin-bottom: 8px;
}

.comment {
  display: flex;
  width: 100%;
}

.comment-input {
  width: 100%;
  padding: 7px;
  outline: none;
  font-size: 12px;
}

.heart.fill-color {
  background-color: #89c329;
}

.likes {
  display: flex;
  align-items: center;
  justify-content: center;
  height: 48px;
  margin-left: 20px;
  gap: 12px;
}

.heart {
  position: relative;
  height: 15px;
  width: 15px;
  background: #c9c9c9;
  display: flex;
  justify-content: center;
  text-align: center;
  border-radius: 0 0 0 10px;
  transform: rotate(-45deg);
  cursor: pointer;
}

.heart::before {
  position: absolute;
  content: "";
  top: -50%;
  left: 0;
  width: 100%;
  height: 100%;
  background: inherit;
  border-radius: 50%;
}
.heart:after {
  position: absolute;
  content: "";
  right: -50%;
  top: 0;
  width: 100%;
  height: 100%;
  background: inherit;
  border-radius: 50%;
}
.heart-and-animated-heart {
  display: flex;
  justify-content: center;
  align-items: center;
}
.animation-heart {
  position: absolute;
  display: flex;
  justify-content: center;
  align-items: center;
  width: 15px;
  height: 15px;
  background: #89c329;
  transform: rotate(-45deg);
  border-radius: 0 0 0 10px;
  cursor: pointer;
  visibility: hidden;
}
.animation-heart::after {
  position: absolute;
  content: "";
  right: 0;
  top: -50%;
  width: 100%;
  height: 100%;
  background: inherit;
  border-radius: 50%;
  cursor: pointer;
}
.animation-heart::before {
  position: absolute;
  content: "";
  width: 100%;
  height: 100%;
  right: -50%;
  top: 0;
  background: inherit;
  border-radius: 50%;
}

.animation-heart.animation {
  animation: popup 0.5s linear;
  visibility: visible;
}

@keyframes popup {
  100% {
    transform: scale(1.5) rotate(-45deg);
    opacity: 0;
  }
}
</style>
