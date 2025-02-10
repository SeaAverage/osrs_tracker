<script setup>

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
</template>