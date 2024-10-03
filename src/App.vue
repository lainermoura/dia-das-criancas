<template>
  <div id="app mt-0 pt-0">
    <header class="d-flex h-auto">
      <!-- <img src="./assets/header/logo-header.png" class="logo w-sm-auto" alt="Imagem com Dia das Crianças escrito" /> -->
      <div class="row header ms-1 pb-0">
        <h1 class="mb-4 p-0">Você consegue adivinhar quem são essas crianças?</h1>
        <p class="m-0">Clique nas fotos e descubra!</p>
      </div>
      <!-- <img src="./assets/header/logo-smf-header.png" class="logo-smf" alt=""> -->
    </header>

    <div class="container">
      <div class="row d-flex justify-content-center position-relative">
        <div
          class="col-6 col-md-2 mt-2 d-flex align-items-stretch justify-content-center"
          v-for="(person, index) in visiblePeople"
          :key="index"
        >
          <CardComp v-bind="person" :borderClass="getBorderClass(index)" />
        </div>
      </div>

      <!-- Adicionar um indicador de carregamento (opcional) -->
      <div v-if="isLoading" class="loading-spinner">
        Carregando...
      </div>
    </div>
  </div>
</template>

<script>
import { ref, onMounted, onBeforeUnmount } from "vue";
import CardComp from "./components/CardComp.vue";

export default {
  name: "App",
  components: {
    CardComp,
  },
  setup() {
    const people = ref(loadImages()); // Carrega todas as imagens de uma vez
    const visiblePeople = ref([]); // Inicialmente, não há cards visíveis
    const itemsPerLoad = 18; // Número de cards a carregar por vez
    let currentIndex = 0; // Controla o índice do próximo card a ser carregado
    const isLoading = ref(false); // Estado para mostrar indicador de carregamento
    let lastIndex = null; // Para gerenciar a borda aleatória

    // Função para carregar mais 18 cards
    function loadMoreCards() {
      if (currentIndex >= people.value.length) return; // Não carregar se não houver mais cards

      isLoading.value = true; // Ativa o estado de carregamento
      const nextIndex = currentIndex + itemsPerLoad;
      const newItems = people.value.slice(currentIndex, nextIndex);
      visiblePeople.value = [...visiblePeople.value, ...newItems]; // Adiciona novos cards à lista visível
      currentIndex = nextIndex;
      isLoading.value = false; // Desativa o estado de carregamento
    }

    // Detectar rolagem até o final da página
    function handleScroll() {
      const scrollPosition = window.innerHeight + window.scrollY;
      const bottomPosition = document.documentElement.offsetHeight;

      if (scrollPosition >= bottomPosition - 50) { // Perto do fim da página
        loadMoreCards();
      }
    }

    // Função para carregar as imagens (cards)
    function loadImages() {
      const context = require.context(
        "@/assets/fotos",
        false,
        /\.(png|jpe?g|svg)$/
      );
      const images = context.keys().reduce((acc, key) => {
        const match = key.match(/\/(.+)-(crianca|adulto)\.(png|jpe?g|svg)$/i);
        if (match) {
          const name = match[1];
          const type = match[2];
          if (!acc[name]) acc[name] = { name };
          acc[name][type] = context(key);
        }
        return acc;
      }, {});

      Object.entries(images).forEach(([name, image]) => {
        if (!image.crianca || !image.adulto) {
          console.error(
            `Faltando foto: ${!image.crianca ? "crianca" : "adulto"} para ${name}`
          );
        }
      });

      const completePairs = Object.values(images).filter(
        (image) => image.crianca && image.adulto
      );

      completePairs.sort((a, b) => a.name.localeCompare(b.name, 'pt-BR'));

      return completePairs.map((image) => ({
        childImage: image.crianca,
        adultImage: image.adulto,
        name: image.name,
      }));
    }

    // Função para gerar borda aleatória
    function getBorderClass() {
      let randomIndex;
      do {
        randomIndex = Math.floor(Math.random() * 6) + 1;
      } while (randomIndex === lastIndex);

      lastIndex = randomIndex;
      return `border-color-${randomIndex}`;
    }

    // Carrega os primeiros 18 cards quando o componente é montado
    onMounted(() => {
      loadMoreCards();
      window.addEventListener("scroll", handleScroll); // Adiciona evento de rolagem
    });

    // Remove o evento de rolagem quando o componente é desmontado
    onBeforeUnmount(() => {
      window.removeEventListener("scroll", handleScroll);
    });

    return {
      visiblePeople,
      isLoading,
      getBorderClass,
    };
  },
};
</script>

<style>
@import 'style.css';
</style>
