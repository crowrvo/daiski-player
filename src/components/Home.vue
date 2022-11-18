<template>
  <div class="home">
    <player></player>
  </div>
</template>

<script>
// @ is an alias to /src
import player from "@/components/player.vue";

export default {
  name: "Home",
  components: {
    player
  }
};
</script>
