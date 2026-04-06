<script setup>
import { ref } from 'vue';

const props = defineProps({
  assunto: Object,
  listaAssuntos: Array,
  niveis: Array,
  nivelAtivo: String,
  indiceAtivo: Number
});

const emit = defineEmits(['mudar-nivel', 'mudar-assunto']);
const mostrarIndice = ref(true);
</script>

<template>
  <section class="layout">
    <main class="content-area">
      <header class="page-header">
        <h1>Caderno de Estudos</h1>
      </header>

      <transition name="fade" mode="out-in">
        <article v-if="assunto" :key="assunto.titulo" class="content-card">
          <h2 class="topic-title">{{ assunto.titulo }}</h2>
          
          <div class="topic-section">
            <span class="badge">CONCEITO</span>
            <p>{{ assunto.conceito }}</p>
          </div>

          <div class="topic-section">
            <span class="badge">ESTRUTURA</span>
            <p class="pre-line">{{ assunto.estrutura }}</p>
          </div>

          <div class="topic-section">
            <span class="badge">NA PRÁTICA</span>
            <p class="practice-box">{{ assunto.exemplos }}</p>
          </div>

          <div class="topic-section" v-if="assunto.exercicios">
            <span class="badge">EXERCÍCIOS</span>
            <ul>
              <li v-for="(ex, i) in assunto.exercicios" :key="i">{{ ex }}</li>
            </ul>
          </div>
        </article>
        
        <div v-else class="empty-state">
          Nenhum conteúdo disponível para este nível.
        </div>
      </transition>
    </main>

    <aside :class="['sidebar', { 'is-collapsed': !mostrarIndice }]">
      <button class="toggle-sidebar" @click="mostrarIndice = !mostrarIndice">
        {{ mostrarIndice ? '→' : '←' }}
      </button>

      <div class="sidebar-wrapper" v-show="mostrarIndice">
        <nav class="levels-nav">
          <button 
            v-for="n in niveis" 
            :key="n"
            :class="['btn-level', { active: n === nivelAtivo }]"
            @click="emit('mudar-nivel', n)"
          >
            {{ n }}
          </button>
        </nav>

        <nav class="topics-nav">
          <p class="nav-label">Assuntos</p>
          <button 
            v-for="(item, index) in listaAssuntos" 
            :key="index"
            :class="['btn-topic', { active: index === indiceAtivo }]"
            @click="emit('mudar-assunto', index)"
          >
            {{ item.titulo }}
          </button>
        </nav>
      </div>
    </aside>
  </section>
</template>

<style scoped>
.layout { display: flex; min-height: 100vh; overflow-x: hidden; }

/* Área Principal */
.content-area { flex: 1; padding: 40px; display: flex; flex-direction: column; align-items: center; }
.page-header { margin-bottom: 30px; text-align: center; }
.content-card { 
  background: #fff; width: 100%; max-width: 800px; padding: 40px; 
  border-radius: 16px; box-shadow: 0 10px 30px rgba(0,0,0,0.08); 
}

/* Tipografia e Badges */
.topic-title { font-size: 2rem; color: #1a1a1a; margin-bottom: 30px; border-bottom: 2px solid #eee; padding-bottom: 10px; }
.topic-section { margin-bottom: 25px; }
.badge { 
  display: inline-block; background: #e8f5e9; color: #2e7d32; 
  padding: 4px 12px; border-radius: 4px; font-size: 11px; font-weight: bold; margin-bottom: 8px;
}
.pre-line { white-space: pre-line; line-height: 1.6; }
.practice-box { background: #f8f9fa; padding: 15px; border-left: 4px solid #4CAF50; font-style: italic; }

/* Sidebar */
.sidebar { 
  width: 320px; background: #fff; border-left: 1px solid #e0e0e0; 
  padding: 30px 20px; position: sticky; top: 0; height: 100vh; transition: all 0.3s ease; 
}
.sidebar.is-collapsed { width: 60px; padding: 30px 10px; }
.toggle-sidebar { 
  position: absolute; left: -15px; top: 50px; width: 30px; height: 30px; 
  border-radius: 50%; border: 1px solid #ddd; background: #fff; cursor: pointer;
}

/* Botões de Navegação */
.levels-nav { display: grid; grid-template-columns: repeat(3, 1fr); gap: 10px; margin-bottom: 30px; }
.btn-level { 
  padding: 10px; border: 1px solid #eee; background: none; 
  border-radius: 8px; cursor: pointer; transition: 0.2s; font-weight: bold;
}
.btn-level.active { background: #4CAF50; color: white; border-color: #4CAF50; }

.topics-nav { display: flex; flex-direction: column; gap: 8px; }
.btn-topic { 
  text-align: left; padding: 15px; border: none; background: #f6f5fa; 
  border-radius: 8px; cursor: pointer; font-size: 13px; transition: 0.2s;
}
.btn-topic.active { background: #fff; border: 1px solid #4CAF50; color: #4CAF50; box-shadow: 0 4px 12px rgba(0,0,0,0.05); }

/* Animações */
.fade-enter-active, .fade-leave-active { transition: opacity 0.2s, transform 0.2s; }
.fade-enter-from, .fade-leave-to { opacity: 0; transform: translateY(10px); }
</style>