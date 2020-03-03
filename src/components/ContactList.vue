<template>
  <div>
    <ContactForm
      @formSubmitted="handleSubmit"
      @formEdited="handleUpdateContact"
      :contactToEdit="contactToEdit"
    />
    <h1>Contact List:</h1>
    <div class="l-contact-grid">
      <div
        class="c-contact-card"
        v-for="contact in contactList"
        :key="contact.id"
      >
        <h2>{{ contact.firstName }}</h2>
        <p>{{ contact.lastName }}</p>
        <p>{{ contact.email }}</p>
        <div class="c-contact-card__actions">
          <button
            @click="handleRemoveContact(contact.id)"
            class="c-button-remove"
          >
            Remove contact
          </button>
          <button @click="handleEditContact(contact.id)" class="c-button-edit">
            Edit contact
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import contacts from '../constants/contacts';
import ContactForm from './ContactForm';

export default {
  name: 'ContactList',
  components: {
    ContactForm
  },
  data() {
    return {
      contactList: contacts,
      contactToEdit: {}
    };
  },
  methods: {
    handleSubmit(value) {
      if (!value.firstName) {
        value.firstName = 'Default First Name';
      }

      if (!value.lastName) {
        value.lastName = 'Default Last Name';
      }
      this.contactList.push(value);
    },
    handleRemoveContact(id) {
      this.contactList = this.contactList.filter(contact => contact.id !== id);
    },
    handleEditContact(id) {
      this.contactToEdit = this.contactList.find(contact => contact.id == id);
    },
    handleUpdateContact(updatedContact) {
      this.contactList.forEach(contact => {
        if (!contact.id == updatedContact.id) {
          return false;
        } else {
          contact.name = updatedContact.firstName;
          contact.lastName = updatedContact.lastName;
          contact.email = updatedContact.email;
        }
      });
    }
  }
};
</script>
<style scoped>
h3 {
  margin: 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}

.c-contact-card {
  display: flex;
  flex-direction: column;
  width: 30rem;
  margin: 0 auto;
  text-align: left;
  border-bottom: 1px solid #123;
  padding: 10px;
}

.c-contact-card__actions {
  display: flex;
  justify-content: flex-end;
  margin-right: -10px;
}

.c-button-remove {
  background-color: rgb(233, 136, 45);
  margin-left: 0;
}

.c-button-edit {
  background-color: rgb(182, 214, 130);
  margin-left: 0;
}
</style>
