<template>
  <div>
    <transition name="fade" appear>
      <div
        class="modal-overlay"
        v-if="showModal"
        @click="showModal = false"
      ></div>
    </transition>

    <transition name="pop" appear>
      <div class="modal" role="dialog" v-if="showModal">
        <span v-show="showModal" @click="showModal = false" class="close-modal">
          &times;
        </span>
        <div class="section add-new-profile-section">
          <Transition>
            <div
              v-if="formStatus.error"
              class="alert alert-danger"
              role="alert"
            >
              user with this information already exist!
              <span @click="formStatus.error = false" class="closebtn">
                &times;
              </span>
            </div>
          </Transition>
          <p class="header">Add new profile:</p>
          <div
            class="flex-row input-wrapper"
            :class="{ 'input-error': errorMessages.name }"
          >
            <label for="newProfileName" class="label">Name:</label>
            <input
              id="newProfileName"
              v-model="newProfile.name"
              class="input"
            />
          </div>
          <p class="error-message" v-show="errorMessages.name">
            {{ errorMessages.name }}
          </p>
          <div
            class="flex-row input-wrapper"
            :class="{ 'input-error': errorMessages.email }"
          >
            <label class="label" for="newProfileEmail">Email:</label>
            <input
              type="email"
              id="newProfileEmail"
              v-model="newProfile.email"
              class="input"
            />
          </div>

          <p class="error-message" v-show="errorMessages.email">
            {{ errorMessages.email }}
          </p>

          <div class="flex-row">
            <Multiselect
              @input="updateSelectedSpecializations"
              :options="specializations"
              :value="newProfile.description"
            />
          </div>
          <button
            type="submit"
            class="add-btn"
            :class="{
              'disabled-button': !areFieldsValid,
            }"
            @click="addProfile"
          >
            Add
          </button>
        </div>
      </div>
    </transition>
  </div>
</template>

<script>
import Multiselect from "./Multiselect.vue";

export default {
  components: { Multiselect },
  props: {
    profiles: {
      type: Array,
      required: true,
    },
  },
  data() {
    return {
      formStatus: {
        error: false,
      },

      showModal: false,
      areFieldsValid: false,
      newProfile: {
        name: "",
        email: "",
        description: [],
      },
      errorMessages: [],
      validations: {
        isEmailValid: false,
        isNameValid: false,
      },
      specializations: [
        "Surgeon",
        "Radiologist",
        "Cardiologist",
        "Psychiatrist",
        "Dermatologist",
      ],
    };
  },
  watch: {
    "newProfile.email": function (value) {
      this.validateEmail(value);
    },
    "newProfile.name": function (value) {
      this.validateName(value);
    },
    "newProfile.description": function () {
      this.checkAllFields();
    },
  },
  computed: {
    existsValue() {
      return this.newProfile.description.length ? true : false;
    },
  },
  methods: {
    toggleModal() {
      this.showModal = !this.showModal;
    },
    addProfile() {
      const isUserExist = this.checkUserExist();
      const areFieldsValid = this.checkAllFields();
      if (areFieldsValid && !isUserExist) {
        this.formStatus.error = false;
        this.$emit("addProfile", this.newProfile);
      } else if (isUserExist) {
        this.formStatus.error = true;
      }
    },

    updateSelectedSpecializations(value) {
      this.newProfile.description = [...value];
    },

    checkUserExist() {
      let foundedUser = false;
      this.profiles.map((profile) => {
        if (profile.email === this.newProfile.email) {
          foundedUser = true;
        }
      });
      return foundedUser;
    },

    validateEmail(value) {
      // eslint-disable-next-line no-useless-escape
      if (/^\w+([\.-]?\w+)*@\w+([\.-]?\w+)*(\.\w{2,3})+$/.test(value)) {
        this.errorMessages["email"] = "";
        this.validations.isEmailValid = true;
      } else {
        this.errorMessages["email"] = "Invalid Email Address";
        this.validations.isEmailValid = false;
      }
    },
    validateName(value) {
      // eslint-disable-next-line no-useless-escape
      if (/^[a-zA-Z$@$!%*?&#^-_. +]+$/.test(value)) {
        this.errorMessages["name"] = "";
        this.validations.isNameValid = true;
      } else {
        this.errorMessages["name"] = "name must be english character";
        this.validations.isNameValid = false;
      }
    },
    checkAllFields() {
      return this.validations.isEmailValid &&
        this.validations.isNameValid &&
        this.existsValue
        ? (this.areFieldsValid = true)
        : (this.areFieldsValid = false);
    },
  },
};
</script>

<style scoped>
.disabled-button {
  cursor: not-allowed;
  background-color: #92c5ae48;
}
.error-message {
  text-align: left !important;
  margin: -12px 0 16px 8px;
  font-size: 14px;
  color: #fd6262;
  animation: errorFadeInDown 0.5s cubic-bezier(0.25, 0.8, 0.5, 1);
}

.input-error {
  border: 1px solid #fd6262;
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

.modal {
  position: fixed;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  margin: auto;
  text-align: center;
  width: fit-content;
  height: fit-content;
  max-width: 22em;
  padding: 2rem;
  border-radius: 1rem;
  box-shadow: 0 5px 5px rgba(0, 0, 0, 0.2);
  background: #fff;
  z-index: 999;
  transform: none;
  background: linear-gradient(
    135deg,
    rgba(65, 184, 131, 0.9),
    rgba(52, 73, 94, 0.9)
  );
}

.modal-overlay {
  content: "";
  position: fixed;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  z-index: 998;
  background: #111e2b;
  opacity: 0.9;
  cursor: pointer;
}
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.4s linear;
}

.fade-enter,
.fade-leave-to {
  opacity: 0;
}

.pop-enter-active,
.pop-leave-active {
  transition: transform 0.4s cubic-bezier(0.5, 0, 0.5, 1), opacity 0.4s linear;
}

.pop-enter,
.pop-leave-to {
  opacity: 0;
  transform: scale(0.3) translateY(-50%);
}

.alert {
  display: flex;
  justify-content: space-between;
  align-items: center;
  font-size: 14px;
  padding: 15px;
  margin-bottom: 20px;
  border: 1px solid transparent;
  background-color: #f2dede;
  border-color: #ebccd1;
  color: #a94442;
  opacity: 1;
  margin-right: auto !important;
  border-radius: 4px;
}
.alert .closebtn {
  font-size: 20px;
  cursor: pointer;
}
.v-enter-active,
.v-leave-active {
  transition: opacity 0.5s ease;
}

.v-enter-from,
.v-leave-to {
  opacity: 0;
}

@media screen and (max-width: 640px) {
  .add-new-profile-section {
    width: unset;
  }
}
@media screen and (max-width: 480px) {
  .close-modal {
    display: block !important;
  }
  .header {
    font-size: 18px;
  }
  .add-new-profile-section {
    margin: 0 auto;
    margin-top: 89px;
    background-color: unset;
  }
  .modal {
    height: 100%;
    width: 100%;
    padding: 0 0;
    border-radius: 0;
  }
}
.close-modal {
  width: 40px;
  height: 40px;
  border-radius: 100%;
  position: absolute;
  top: 12px;
  right: 50%;
  font-size: 34px;
  z-index: 33434343;
  background: #fefefe;
  transform: translateX(50%);
  transition: all 0.2s;
  display: none;
}
.close-modal:hover {
  background-color: #e05454;
  color: #fff;
}
</style>
