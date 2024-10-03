<template>
  <div class="card-container mt-2" ref="cardContainer">
    <div class="card" :class="{ flipped: showAdult }" @click="toggleImage">
      <div class="card-face card-front">
        <img
          :src="childImage"
          class="card-img-top"
          :class="borderClass"
          alt="Foto de Criança"
        />
      </div>
      <div class="card-face card-back">
        <img
          :src="adultImage"
          class="card-img-top"
          :class="borderClass"
          alt="Foto de Adulto"
        />
        <h6 class="card-name">{{ formattedName }}</h6>
      </div>
    </div>
  </div>
</template>

<script>
import { ref, computed } from "vue";

export default {
  name: "CardComp",
  props: {
    childImage: String,
    adultImage: String,
    name: String,
    borderClass: String,
  },
  setup(props) {
    const showAdult = ref(false);
    const cardContainer = ref(null);

    const formattedName = computed(() => {
      return formatName(props.name);
    });

    function toggleImage() {
      showAdult.value = !showAdult.value;
      if (showAdult.value) {
        scrollToCard();
      }
    }

    function scrollToCard() {
      if (cardContainer.value) {
        const elementPosition =
          cardContainer.value.getBoundingClientRect().top + window.scrollY + 50;
        const elementHeight =
          cardContainer.value.getBoundingClientRect().height;
        const viewportHeight = window.innerHeight;

        // Verifica se o card está parcialmente fora da tela
        if (elementPosition + elementHeight > window.scrollY + viewportHeight) {
          const offsetPosition =
            elementPosition + elementHeight - viewportHeight;
          window.scrollTo({
            top: offsetPosition,
            behavior: "smooth",
          });
        } else {
          // Se o card estiver completamente visível, rola apenas um pouco para garantir que o nome apareça
          cardContainer.value.scrollIntoView({
            behavior: "smooth",
            block: "nearest",
          });
        }
      }
    }

    function formatName(name) {
      const spacedName = name.replace(/([a-z])([A-Z])/g, "$1 $2");
      return spacedName
        .split(" ")
        .map((word) => word.charAt(0).toUpperCase() + word.slice(1))
        .join(" ");
    }

    return {
      showAdult,
      formattedName,
      toggleImage,
      cardContainer,
    };
  },
};
</script>
