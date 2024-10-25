<template>
  <div>
    <section class="py-40 mt-20">
      <div
        class="wrapper flex flex-col lg:flex-row items-center justify-center gap-8"
      >
        <div class="pet-title text-center lg:text-left">
          <h1 class="text-8xl font-bold leading-none">Bom<br />Garoto</h1>
          <h3 class="text-2xl my-4">Automação para o seu pet</h3>
          <a
            class="bg-purple-500 text-fontcolor py-2 px-6 rounded hover:bg-white hover:text-bgcolor font-bold"
            href="#about"
          >
            SAIBA MAIS
          </a>
        </div>
        <div class="pet-img">
          <img
            src="../assets/images/dog-black-white-background.webp"
            class="rounded-full"
          />
        </div>
      </div>
    </section>
    <section class="bg-gray-100 py-40 -mt-20" id="about">
      <div
        class="wrapper flex flex-col lg:flex-row items-center justify-center gap-8"
      >
        <div class="about-img w-1/3">
          <img
            src="../assets/images/drdog.webp"
            alt="Imagem do cachorro"
            class="rounded-full"
          />
        </div>
        <div class="about-title text-center lg:text-left space-y-4">
          <h1 class="text-4xl font-bold">Sobre Bom Garoto</h1>
          <p>
            O Bom Garoto é um sistema inovador de dispenser automático de petz
            que pode ser controlado remotamente.
          </p>

          <RouterLink
            class="bg-purple-500 text-fontcolor py-2 px-6 rounded hover:bg-white hover:text-bgcolor font-bold"
            to="/about"
            >Leia Mais</RouterLink
          >
        </div>
      </div>
    </section>

    <section id="services">
      <div class="wrapper text-center">
        <h1 class="text-4xl font-bold mb-8">Serviços</h1>
        <div class="pet-cards flex flex-wrap justify-center gap-8">
          <div class="card text-center bg-gray-200 p-6 rounded shadow-md w-64">
            <img
              src="../assets/images/service1.webp"
              alt=""
              class="card-img rounded-full mb-4"
            />
            <h2 class="card-head text-xl font-semibold">Automação</h2>
            <p class="card-pera">Automação alimentícia para o pet.</p>
          </div>
          <div class="card text-center bg-gray-200 p-6 rounded shadow-md w-64">
            <img
              src="../assets/images/service2.webp"
              alt=""
              class="card-img rounded-full mb-4"
            />
            <h2 class="card-head text-xl font-semibold">Automação</h2>
            <p class="card-pera">Automação alimentícia para o pet.</p>
          </div>
        </div>
      </div>
    </section>

    <section class="bg-gray-100">
      <div class="wrapper text-center">
        <h1 class="text-4xl font-bold mb-8">Amor pelo Cliente</h1>
        <div
          class="grallery grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-4"
        >
          <div class="grallery-img">
            <img
              src="../assets/images/dog.jpg"
              class="w-full h-40 object-cover rounded-lg"
            />
          </div>
          <div class="grallery-img">
            <img
              src="../assets/images/cat2.jpg"
              class="w-full h-40 object-cover rounded-lg"
            />
          </div>
          <div class="grallery-img">
            <img
              src="../assets/images/dog3.jpg"
              class="w-full h-40 object-cover rounded-lg"
            />
          </div>
        </div>
      </div>
    </section>

    <section>
      <div class="wrapper text-center">
        <h1 class="text-4xl font-bold mb-8">O que os donos dizem</h1>
        <div class="love-card grid grid-cols-1 md:grid-cols-3 gap-8">
          <div class="l-card bg-gray-200 p-6 rounded shadow-md">
            <h2 class="text-xl font-semibold">"Amor ao Trabalho"</h2>
            <p class="text-sm mt-4">
              Com o dispenser automático, minha rotina com meu pet ficou muito
              mais tranquila.
            </p>
            <h4 class="mt-8 font-bold">— Amanda Lopes</h4>
          </div>
        </div>
      </div>
    </section>

    <section class="bg-gray-100" id="contact">
      <div class="wrapper text-center space-y-8">
        <h1 class="text-4xl font-bold">Contato</h1>
        <div
          class="detail-box grid grid-cols-1 sm:grid-cols-2 md:grid-cols-4 gap-4 text-sm"
        >
          <div class="detail">
            <h2>Telefone</h2>
            <p>123-456-7890</p>
          </div>
          <div class="detail">
            <h2>Email</h2>
            <p>victor.ross098@gmail.com</p>
          </div>
        </div>
        <form id="contactForm" class="input-box space-y-4">
          <input
            type="text"
            name="name"
            placeholder="Nome"
            class="w-full p-2 border-b-2 border-gray-400 focus:outline-none"
          />
          <textarea
            name="message"
            placeholder="Como podemos ajudar?"
            class="w-full p-2 border-b-2 border-gray-400 resize-none"
          ></textarea>
          <button
            class="submit-btn w-full bg-button-bg text-white p-2 rounded hover:bg-button-hover-bg"
          >
            Enviar
          </button>
        </form>
      </div>
    </section>
  </div>
</template>

<style lang="postcss">
section {
  @apply px-10 py-40 -mt-20;
}
</style>
