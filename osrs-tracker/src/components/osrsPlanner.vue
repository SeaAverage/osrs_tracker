<script setup>
defineProps({
  msg: {
    type: String,
  },
})
  const apiUrl = "https://secure.runescape.com/m=hiscore_oldschool/index_lite.json?player=Wohki"

  function getPlayerData(){
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
    <h1>Your hard coded username in the App.vue file is {{ msg }}</h1>
    <h3>
      Click the button below to (hopefully) retrieve some of your user data
      <button @click="getPlayerData">Hit the highscores</button>
    </h3>
  </div>
</template>