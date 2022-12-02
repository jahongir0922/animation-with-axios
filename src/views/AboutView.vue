<template>
  <div id="about">
    <Transition mode="out-in" name="toldiring">
      <h3 id="toldiring" v-if="toldiring">Maydonni to'ldiring</h3>
    </Transition>
    <input @keyup.enter="addDos" v-model="addDo" type="text" />
    <Transition mode="out-in" name="bajarish">
      <h1 v-if="bajarish" id="bajarish">Bajarish uchun hech vaqo qolmadi</h1>
    </Transition>
    <TransitionGroup name="do" tag="ul" mode="out-in">
      <li
        id="do"
        @click="delMalumot(item.id)"
        v-for="(item, index) in toDos"
        :key="index"
      >
        {{ item.do }}
      </li>
    </TransitionGroup>
  </div>
</template>
<script setup>
import { computed, ref } from "vue";
import axios from "axios";
const toDos = ref([]);
var addDo = ref();
var xatolik = ref("");
var toldiring = ref(false);
// var bajarish = ref(false);

var bajarish = computed(() => {
  if (!toDos.value.length) {
    return true;
  } else {
    return false;
  }
});

function addDos() {
  if (addDo.value) {
    axios
      .post("http://localhost:3000/toDos", {
        do: addDo.value,
      })
      .then(() => {
        addDo.value = "";
        getMalumot();
      })
      .catch((err) => {
        console.log(err.message);
      });
  } else {
    toldiring.value = true;
    setTimeout(() => {
      toldiring.value = false;
    }, 2000);
  }
}
function delMalumot(iddagisini) {
  axios
    .delete("http://localhost:3000/toDos/" + iddagisini)
    .then(() => {
      getMalumot();
    })
    .catch((err) => {
      xatolik.value = err.message;
      // console.log(err.message);
    });
}
function getMalumot() {
  axios
    .get("http://localhost:3000/toDos")
    .then((res) => {
      toDos.value = [...res.data];
    })
    .catch((err) => {
      xatolik.value = err.message;
      // console.log(err.message);
    });
}
getMalumot();
</script>
<style scoped lang="scss">
#about {
  display: flex;
  padding: 50px;
  align-items: center;
  flex-direction: column;
  gap: 10px;
  #toldiring {
    position: absolute;
    top: -20px;
    left: 50%;
    transform: translateX(-50%);
    background-color: red;
    padding: 15px;
    border-radius: 5px;
    color: white;
    animation-name: toldiring;
    animation-duration: 1s;
  }
  input {
    padding: 15px;
    border-radius: 5px;
    width: 300px;
  }
  ul {
    display: flex;
    flex-direction: column;
    gap: 40px;
    #do {
      padding: 15px;
      border-radius: 5px;
      width: 300px;
      text-align: center;
      background-color: rgb(222, 206, 206);
    }
  }
}
// ------------------transitions
// ----1)

@keyframes toldiring {
  0% {
    left: 50%;
    // transform: translateX(-45%);
    transform: translateX(calc(-50% - 15px));
  }
  10% {
    left: 50%;
    // transform: translateX(55%);
    transform: translateX(calc(-50% + 15px));
  }
  20% {
    left: 50%;
    // transform: translateX(-40%);
    transform: translateX(calc(-50% - 20px));
  }
  30% {
    left: 50%;
    // transform: translateX(60%);
    transform: translateX(calc(-50% + 20px));
  }
  40% {
    left: 50%;
    // transform: translateX(-35%);
    transform: translateX(calc(-50% - 25px));
  }
  50% {
    left: 50%;
    // transform: translateX(65%);
    transform: translateX(calc(-50% + 25px));
  }
  60% {
    left: 50%;
    // transform: translateX(-40%);
    transform: translateX(calc(-50% - 20px));
  }
  70% {
    left: 50%;
    // transform: translateX(60%);
    transform: translateX(calc(-50% + 20px));
  }
  80% {
    left: 50%;
    // transform: translateX(-45%);
    transform: translateX(calc(-50% - 15px));
  }
  90% {
    left: 50%;
    // transform: translateX(55%);
    transform: translateX(calc(-50% + 15px));
  }
  100% {
    left: 50%;
    // transform: translateX(50%);
    transform: translateX(-50%);
  }
}
.toldiring-enter-from {
  opacity: 0;
}
.toldiring-enter-to {
  opacity: 1;
}
.toldiring-enter-active {
  transition: all 0.5s ease;
}
.toldiring-leave-from {
  opacity: 1;
}
.toldiring-leave-to {
  opacity: 0;
}
.toldiring-leave-active {
  transition: all 0.5s ease;
}
// ----2)
.do-enter-active,
.do-leave-active {
  transition: all 0.5s ease;
}
.do-enter-from,
.do-leave-to {
  opacity: 0;
  transform: translateX(100px);
}
// ----3)
.bajarish-enter-active,
.bajarish-leave-active {
  transition: all 0.5s ease;
}
.bajarish-enter-from,
.bajarish-leave-to {
  opacity: 0;
  transform: translateX(100px);
}
</style>
