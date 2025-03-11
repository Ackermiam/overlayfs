<template>
  <div>
    <div
      class="dark-souls-container"
      @click="validateBoss($event)"
      @contextmenu.prevent="invalidateLastBoss($event)"
    >
      <h1>Bosses</h1>
      <div class="boss-list">
        <div
          v-for="(boss, index) in bosses"
          :key="index"
          class="boss"
          :class="{ validated: index <= validatedIndex }"
        >
          {{ boss }}
          <div v-if="index < bosses.length - 1" class="separator"></div>
        </div>
      </div>
    </div>
    <img src="/fsoverlay/chevalier.png" class="chevalier"/>
    <div class="epeeContainer">
      <img src="/fsoverlay/epees.png" class="epee epeeUn"/>
      <img src="/fsoverlay/epees.png" class="epee epeeDeux"/>
    </div>
  </div>
</template>

<script setup>
import { ref, onUnmounted, onMounted } from "vue";

const bosses = ref([
  "Abyss Watchers",
  "Pontiff Sulyvahn",
  "Nameless King",
  "Sister Friede",
  "Slave Knight Gael",
]);

let validatedIndex = ref(-1);

const validateBoss = (event) => {
  if (validatedIndex.value < bosses.value.length - 1) {
    validatedIndex.value++;
  }
};

const invalidateLastBoss = (event) => {
  event.preventDefault();
  if (validatedIndex.value >= 0) {
    validatedIndex.value--;
  }
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
  background-color: #333333d7;
  color: white;
  border: 2px solid #572e0c;
  border-radius: 8px;
}

h1 {
  font-family: 'shadow';
  font-size: 2.5em;
}

.boss-list {
  pointer-events: none;
}

.boss {
  margin-bottom: 10px;
}

.validated {
  text-decoration: line-through;
  opacity: 0.5;
}

.separator {
  height: 1px;
  width: 100%;
  background-color: rgba(146, 146, 146, 0.322);
  margin-top: 10px;
}

.chevalier {
  width: 150px;
  height: auto;
  position: fixed;
  top: 90px;
  right: 25px;
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
  top: 560px;
  right: -10px;
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
