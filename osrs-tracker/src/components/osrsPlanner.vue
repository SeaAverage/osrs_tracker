<script setup>
  import { ref } from 'vue';

  let user = ref('');
  let playerData = ref(null);
  let loading = ref(false);
  let errorMessage = ref('');


  function getPlayerData(user) {
    loading.value = true;
    const apiUrl = "https://secure.runescape.com/m=hiscore_oldschool/index_lite.json?player=" + user;
    fetch(apiUrl)
      .then(response => {
        if (!response.ok) {
          if (response.status === 404) {
            throw new Error('Player not found');
          }
          throw new Error('An unknown error occurred');
        }
        return response.json();
      })
      .then(data => {
        playerData.value = data;
        console.log(data);
      })
      .catch(error => {
        console.error('Error:', error);
        errorMessage.value = error.message;
      })
      .finally(() => {
        loading.value = false;
      });
  }
</script>

<template>
  <div class="greetings">
    <h1>Please enter your username!</h1>
    <input v-model="user" placeholder="Enter your username" />
    <h3>
      Click the button below to (hopefully) retrieve some of your user data
      <button @click="getPlayerData(user)">Hit the highscores</button>
    </h3>
  </div>
  <div v-if="loading">
    <p>Loading...</p>
  </div>
  <div v-if="errorMessage">
    <p style="color: red;">{{ errorMessage }}</p>
  </div>
  <div v-if="playerData && !loading">
    <h2>Player Data</h2>
    <table>
      <thead>
        <tr>
          <th>Skill</th>
          <th>Level</th>
          <th>Experience</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(value, index) in playerData.skills" :key="index">
          <td>{{ value.name }}</td>
          <td>{{ value.level }}</td>
          <td>{{ value.xp }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>