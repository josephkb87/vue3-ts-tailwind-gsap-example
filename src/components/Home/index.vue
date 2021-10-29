<template>
  <div>
    <div class="bg-gray-50">
      <div class="container">
        <div class="md:hidden py-4 text-right">
          <button
            @click="openHamburgerMenu"
            class="toggle bg-gray-600 text-white rounded-lg py-1.5 px-3"
          >
            <span class="transform rotate-90 block font-bold">|||</span>
          </button>
        </div>
        <ul
          ref="navbar"
          class="
            navbar
            md:flex
            md:flex-wrap
            md:justify-center
            md:my-0
            md:bg-transparent
            md:p-0
            p-4
            bg-gray-200
            my-2
          "
          v-show="isHamburgerMenuVisible"
        >
          <li
            class="
              nav-item
              md:p-4 md:bg-transparent md:mb-0 md:rounded-none
              bg-gray-300
              rounded
              mb-2
              p-2
            "
            v-for="(i, index) in 4"
            :key="index"
          >
            <a href="#">Item {{ index }}</a>
          </li>
        </ul>
      </div>
    </div>

    <div class="container">
      <div class="py-3">
        <ul class="cards md:flex md:flex-wrap">
          <li
            class="card lg:w-1/3 md:w-1/2 mb-4 md:px-2"
            v-for="(i, index) in 3"
            :key="index"
          >
            <div class="xl:flex p-2 rounded shadow-lg bg-gray-100">
              <div class="xl:w-1/2">
                <img
                  src="@/assets/logo.png"
                  class="w-full md:h-full h-60 rounded"
                  alt=""
                />
              </div>
              <div class="xl:w-1/2 xl:pl-2">
                <div class="xl:block flex flex-wrap justify-between">
                  <p class="text-sm xl:mb-1 font-bold">Something 1</p>
                  <p class="text-sm xl:mb-1 font-bold">Something 2</p>
                  <p class="text-sm xl:mb-1 font-bold">Something 3</p>
                </div>
                <p class="text-base text-justify text-gray-700">
                  Lorem ipsum dolor sit amet consectetur adipisicing elit.
                  Harum, quod earum! Veritatis velit fugiat...
                </p>
              </div>
            </div>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { computed, defineComponent, onMounted, ref } from "vue";
import { gsap } from "gsap";

export default defineComponent({
  name: "Home",

  setup() {
    // properties
    const isHamburgerMenuVisible = ref<boolean>(false);
    const navbar = ref<any>();

    // computed
    const tl = computed(() => gsap.timeline());

    // methods
    const openHamburgerMenu = () => {
      const navItem: any = document.querySelectorAll(".nav-item");

      if (!isHamburgerMenuVisible.value) {
        tl.value.from(navbar.value, {
          duration: 1,
          x: 700,
          onStart: () => {
            isHamburgerMenuVisible.value = true;
          },
        });
        tl.value.from(navItem, {
          duration: 0.5,
          y: -720,
          opacity: 1,
          stagger: 0.2,
        });
      } else if (isHamburgerMenuVisible.value) {
        tl.value.to(navItem, {
          duration: 0.5,
          x: -720,
          opacity: 0,
          stagger: 0.2,
        });
        tl.value
          .to(navbar.value, {
            duration: 0.5,
            x: -700,
          })
          .to(navItem, {
            x: 0,
            opacity: 1,
            onComplete: () => {
              isHamburgerMenuVisible.value = false;
            },
          })
          .to(navbar.value, {
            x: 0,
            onComplete: () => {
              gsap.set(navbar.value, { clearProps: "transform" });
              gsap.set(navItem, { clearProps: "transform" });
              isHamburgerMenuVisible.value = false;
            },
          });
      }
    };
    // lifecycle hooks
    onMounted(() => {
      const navItem: any = document.querySelectorAll(".nav-item");
      const card: any = document.querySelectorAll(".card");

      let screenSize: number = window.innerWidth;
      if (screenSize >= 768) {
        isHamburgerMenuVisible.value = true;
        tl.value
          .fromTo(
            navItem,
            { x: -200, y: -200 },
            { duration: 1, x: 0, y: 0, stagger: 0.3 }
          )
          .fromTo(
            card[0],
            { x: -720, opacity: 0 },
            { duration: 1, opacity: 1, x: 0 },
            1
          )
          .fromTo(
            card[1],
            { y: 360, opacity: 0 },
            { duration: 1, opacity: 1, y: 0 },
            1
          )
          .fromTo(
            card[2],
            { x: 720, opacity: 0 },
            { duration: 1, opacity: 1, x: 0 },
            1
          );
      } else {
        tl.value.fromTo(
          card,
          { y: -720, opacity: 0 },
          { duration: 1, opacity: 1, y: 0, stagger: 0.3 },
          1
        );
      }
      window.addEventListener("resize", (e: any) => {
        screenSize = e.target.innerWidth;
        if (screenSize >= 768) {
          isHamburgerMenuVisible.value = true;
        } else {
          isHamburgerMenuVisible.value = false;
        }
      });
    });

    return {
      // properties
      isHamburgerMenuVisible,
      navbar,
      // methods
      openHamburgerMenu,
    };
  },
});
</script>
