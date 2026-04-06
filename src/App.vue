<script setup>
import { ref, computed, watch } from 'vue';
import { baseConteudo, niveisDisponiveis } from './data/conteudo';
import ConteudoPrincipal from './components/ConteudoPrincipal.vue';
import Menu from './components/menu.vue';

// Estado Reativo
const nivelAtivo = ref('A1');
const indiceAtivo = ref(0);

// Lógica de Filtragem
const assuntosFiltrados = computed(() => 
  baseConteudo.filter(item => item.nivel === nivelAtivo.value)
);

const assuntoSelecionado = computed(() => 
  assuntosFiltrados.value[indiceAtivo.value] || null
);

// Resetar o índice ao trocar de nível para evitar erros de array vazio
watch(nivelAtivo, () => {
  indiceAtivo.value = 0;
});

const handleMudarNivel = (novoNivel) => {
  nivelAtivo.value = novoNivel;
};

const handleMudarAssunto = (novoIndice) => {
  indiceAtivo.value = novoIndice;
};
</script>

<template>
  
  <div class="app-container">
    <ConteudoPrincipal 
      :assunto="assuntoSelecionado"
      :lista-assuntos="assuntosFiltrados"
      :niveis="niveisDisponiveis"
      :nivel-ativo="nivelAtivo"
      :indice-ativo="indiceAtivo"
      @mudar-nivel="handleMudarNivel"
      @mudar-assunto="handleMudarAssunto"
    />
  </div>
</template>

<style>
/* Estilos Globais */
* { box-sizing: border-box; margin: 0; padding: 0; }
body { font-family: 'Inter', sans-serif; background-color: #F6F5FA; color: #333; }
</style>