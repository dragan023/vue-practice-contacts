<template>
  <form @submit.prevent="handleSubmit">
    <label for="firstName">First Name</label>
    <input
      v-model="newContact.firstName"
      type="text"
      name="firstName"
      id="firstName"
    />

    <label for="lastName">Last Name</label>
    <input
      v-model="newContact.lastName"
      type="text"
      name="lastName"
      id="lastName"
    />

    <label for="emal">Email</label>
    <input v-model="newContact.email" type="email" name="email" id="email" />
    <span class="c-error-msg" v-if="emailError">Email is required</span>

    <button type="submit">Add</button>
    <button v-if="isEditMode" @click.prevent="handleUpdate">Save</button>
  </form>
</template>

<script>
export default {
  name: 'ContactForm',
  data() {
    return {
      newContact: {},
      emailError: false
    };
  },
  methods: {
    handleSubmit() {
      if (!this.newContact.email) {
        this.emailError = true;
        return false;
      }

      this.$emit('formSubmitted', this.newContact);
    },
    handleUpdate() {
      this.$emit('formEdited', this.newContact);
      this.newContact = {};
      this.editMode = false;
    }
  },
  watch: {
    isEditMode: function() {
      this.newContact = { ...this.contactToEdit };
    }
  },
  computed: {
    isEditMode() {
      return this.isEditing;
    }
  },
  props: {
    contactToEdit: {
      type: Object,
      required: false,
      default: () => {}
    },
    isEditing: {
      type: Boolean,
      required: true
    }
  }
};
</script>

<style>
form {
  width: 30rem;
  margin: 0 auto;
  text-align: left;
  display: flex;
  flex-direction: column;
}
input {
  display: block;
  width: 100%;
  padding: 10px 20px;
}

button {
  width: 10rem;
  padding: 5px 20px;
  background: rgb(49, 144, 240);
  border-radius: 2px;
  margin-left: auto;
  cursor: pointer;
  margin-top: 10px;
}

.c-error-msg {
  color: red;
}

p {
  margin-bottom: 6px;
  margin-top: 0;
}
</style>
