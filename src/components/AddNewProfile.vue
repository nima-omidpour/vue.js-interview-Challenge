<template>
  <div class="section">
    <p class="header">Add new profile:</p>
    <div class="flex-row" :class="{ 'input-error': errorMessages.name }">
      <label for="newProfileName" class="label">Name:</label>
      <input id="newProfileName" v-model="newProfile.name" class="input" />
    </div>
    <p class="error-message" v-show="errorMessages.name">
      {{ errorMessages.name }}
    </p>
    <div class="flex-row" :class="{ 'input-error': errorMessages.email }">
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
</template>

<script>
import Multiselect from "./Multiselect.vue";

export default {
  components: { Multiselect },
  data() {
    return {
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
    addProfile() {
      if (this.checkAllFields()) {
        console.log(this.newProfile);
        this.$emit("addProfile", this.newProfile);
      }
    },

    updateSelectedSpecializations(value) {
      this.newProfile.description = [...value];
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
</style>
