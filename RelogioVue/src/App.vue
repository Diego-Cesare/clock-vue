<template>
<div class="w-full h-screen flex items-center justify-center">
  <Relogio/>
</div>
</template>

<script setup>
import {ref} from 'vue'
import Relogio from './components/Relogio.vue'
const novoNome = ref('')

function emitirEvento(n){
  novoNome.value = n
}

</script>
