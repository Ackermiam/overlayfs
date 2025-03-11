<template>
  <div>
    <div class="dark-souls-container">
      <h1>Bosses</h1>
      <div class="boss-list" ref="bossList">
        <div
          v-for="(boss, index) in bosses"
          :key="index"
          class="boss"
          :class="{ validated: index <= validatedIndex }"
          style="margin-right: 20px"
        >
          {{ boss }}
          <div v-if="index < bosses.length - 1" class="separator"></div>
        </div>
      </div>
    </div>
    <img src="/chevalier.png" class="chevalier" />
    <div class="epeeContainer">
      <img src="/epees.png" class="epee epeeUn" />
      <img src="/epees.png" class="epee epeeDeux" />
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted, nextTick } from "vue";

const bosses = ref([
  "Iudex Gundyr",
  "Vordt",
  "Abys watcher",
  "Wolnir",
  "Crystal sage",
  "Diacres",
  "Pontiff sulhyvan",
  "Aldrich",
  "Danseuse boréale",
  "Dragon slayer armor",
  "Lotric princes",
  "Chevalier des cendres",
]);

const validatedIndex = ref(-1);
const bossList = ref(null);

const channel = new BroadcastChannel("boss_channel");

const validateBoss = () => {
  if (validatedIndex.value < bosses.value.length - 1) {
    validatedIndex.value++;
    channel.postMessage(validatedIndex.value);

    nextTick(() => {
      if (bossList.value) {
        bossList.value.scrollTo({
          top: bossList.value.scrollHeight,
          behavior: "smooth",
        });
      }
    });
  }
};

const invalidateLastBoss = (event) => {
  event.preventDefault();
  if (validatedIndex.value >= 0) {
    validatedIndex.value--;
    channel.postMessage(validatedIndex.value);
  }
};

channel.onmessage = (event) => {
  validatedIndex.value = event.data;
};

onMounted(() => {
  window.addEventListener("click", validateBoss);
  window.addEventListener("contextmenu", invalidateLastBoss);
});

onUnmounted(() => {
  window.removeEventListener("click", validateBoss);
  window.removeEventListener("contextmenu", invalidateLastBoss);
});
</script>

<style scoped>
.dark-souls-container {
  position: fixed;
  top: 200px;
  right: 20px;
  padding: 10px 30px 70px 30px;
  background-color: #3333337a;
  color: white;
  border: 2px solid #572e0c;
  border-radius: 8px;
}

h1 {
  font-family: "shadow";
  font-size: 2.5em;
  color: #ffd5a2;
  text-align: center;
}

.boss-list {
  max-height: 250px;
  overflow-y: auto;
}

.boss-list::-webkit-scrollbar {
  width: 6px;
}

.boss-list::-webkit-scrollbar-thumb {
  background-color: #815d31;
  border-radius: 4px;
}

.boss-list::-webkit-scrollbar-track {
  background: transparent;
}

.boss {
  margin-bottom: 10px;
}

.validated {
  text-decoration: line-through;
  text-decoration-color: rgb(0, 255, 0);
  text-decoration-thickness: 4px;
  opacity: 0.5;
}

.separator {
  height: 1px;
  width: 100%;
  background-color: rgba(104, 104, 104, 0.596);
  margin-top: 10px;
}

.chevalier {
  width: 150px;
  height: auto;
  position: fixed;
  top: 90px;
  right: 55px;
}

.epeeContainer {
  position: fixed;
  top: 550px;
  right: 25px;
}

.epee {
  width: 250px;
  height: auto;
  position: fixed;
  top: 610px;
  right: 10px;
}

.epeeUn {
  transform: rotateZ(-20deg);
  animation: rotateFirstSword 5s infinite;
}

.epeeDeux {
  transform: rotateZ(200deg);
  animation: rotateSecondSword 5s infinite;
}

@keyframes rotateFirstSword {
  0% {
    transform: rotateZ(-20deg);
  }
  10% {
    transform: rotateZ(-15deg);
  }
  60% {
    transform: rotateZ(-30deg);
  }
  100% {
    transform: rotateZ(-20deg);
  }
}
@keyframes rotateSecondSword {
  0% {
    transform: rotateZ(200deg);
  }
  10% {
    transform: rotateZ(195deg);
  }
  60% {
    transform: rotateZ(215deg);
  }
  100% {
    transform: rotateZ(200deg);
  }
}
</style>
