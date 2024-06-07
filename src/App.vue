<script setup>
import contactsInfo from "./contacts.json";
import { ref, computed } from "vue";

const contacts = ref(contactsInfo.slice(5, 10));

const addRandomContact = () => {
  const remainingContacts = contactsInfo.filter(
    (contact) =>
      !contacts.value.some((shownContact) => shownContact.id === contact.id)
  );
  const randomIndex = Math.floor(Math.random() * remainingContacts.length);
  contacts.value.push(remainingContacts[randomIndex]);
};

const sortedContacts = computed(() => {
  return [...contacts.value].sort((a, b) => a.name.localeCompare(b.name));
});

const sortByName = () => {
  contacts.value = [...contacts.value].sort((a, b) =>
    a.name.localeCompare(b.name)
  );
};

const sortByPopularity = () => {
  contacts.value = [...contacts.value].sort(
    (a, b) => b.popularity - a.popularity
  );
};

const deleteContact = (id) => {
  const index = contacts.value.findIndex((contact) => contact.id === id);
  if (index !== -1) {
    contacts.value.splice(index, 1);
  }
};
</script>

<template>
  <div class="container">
    <h1>Iron Contacts</h1>
    <div class="buttons">
      <button class="button" @click="addRandomContact">ADD RANDOM CONTACT</button>
      <button class="button" @click="sortByName">SORT BY POPULARITY</button>
      <button class="button" @click="sortByPopularity">SORT BY NAME</button>
    </div>
    <table>
      <thead>
        <tr>
          <th>Picture</th>
          <th>Name</th>
          <th>Popularity</th>
          <th>Won an Oscar</th>
          <th>Won an Emmy</th>
          <th>Action</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="contact in contacts" :key="contact.id">
          <td>
            <img class="image"
              :src="contact.pictureUrl"
              alt="Foto de {{ contact.name }}"
              style="width: 100px; height: 100px"
            />
          </td>
          <td>{{ contact.name }}</td>
          <td>{{ contact.popularity }}</td>
          <td>
            <span v-if="contact.wonOscar">🏆</span>
            <span v-else></span>
          </td>
          <td>
            <span v-if="contact.wonEmmy">🏆</span>
            <span v-else></span>
          </td>
          <td><button class="delete-button" @click="deleteContact(contact.id)">Delete</button></td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<style>
@import url("https://fonts.googleapis.com/css2?family=Roboto:ital,wght@0,100;0,300;0,400;0,500;0,700;0,900;1,100;1,300;1,400;1,500;1,700;1,900&display=swap");

table {
  margin: 0 auto; 
  border-collapse: collapse;
  width: 60%; 
  background-color: #414143;
}

th, td {
  border: 1px solid #111111; 
  padding: 8px;
  color: white;
}

th {
  background-color: #45188d; 
}

.container {
  font-family: "Roboto", sans-serif;
  text-align: center;
}

.buttons {
  display: flex;
  flex-direction: row;
  justify-content: center;
  gap: 80px;
  padding: 20px;
}

.button {
  font-size: 16px;
  background-color: #45188d;
  color: white;
  padding: 5px;
  border-radius: 20px;
  width: 200px;
}

.delete-button{
  font-size: 18px;
  background-color: #45188d;
  color: white;
  padding: 10px;
  border-radius: 20px;
  width: 100px;
  border-color: #5f5fe8;
}

.image {
  border-radius: 20px;
}


</style>