<script setup>
  import { ref } from 'vue';
  import { nextTick } from 'vue';

  const renderComponent = ref(true);
  let user = ref('');
  let playerData = ref(null);

  const forceRerender = async () => {
    renderComponent.value = false; M
    await nextTick();
    renderComponent.value = true;
  };

  function getPlayerData(user) {
    const apiUrl = "https://secure.runescape.com/m=hiscore_oldschool/index_lite.json?player=" + user;
    fetch(apiUrl)
      .then(response => {
        if (!response.ok) {
          throw new Error('Network response was not ok');
        }
        return response.json();
      })
      .then(data => {
        playerData.value = data;
        console.log(data);
      })
      .catch(error => {
        console.error('Error:', error);
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
  <div v-if="playerData">
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