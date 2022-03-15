<template>
  <nav
    class="navbar"
    :class="darkMode ? 'dark' : ''"
    role="navigation"
    aria-label="main navigation"
  >
    <div
      class="
        navbar-brand
        is-vcentered is-align-items-center is-justify-content-center
      "
    >
      <router-link
        class="navbar-item"
        to="/"
        :class="darkMode ? 'navitem' : ''"
      >
        <img :src="logo" width="140" class="" />
      </router-link>

      <a
        role="button"
        class="navbar-burger"
        aria-label="menu"
        aria-expanded="false"
        data-target="navbarBasicExample"
      >
        <span aria-hidden="true" :class="darkMode ? 'darkText' : ''"></span>
        <span aria-hidden="true" :class="darkMode ? 'darkText' : ''"></span>
        <span aria-hidden="true" :class="darkMode ? 'darkText' : ''"></span>
      </a>
    </div>

    <div
      id="navbarBasicExample"
      class="navbar-menu"
      :class="darkMode ? 'dark' : ''"
    >
      <div class="navbar-end">
        <router-link
          to="/explore"
          class="navbar-item nav-list-menu"
          :class="darkMode ? 'darkText' : ''"
        >
          Explore
        </router-link>

        <router-link
          to="/creators"
          class="navbar-item nav-list-menu"
          :class="darkMode ? 'darkText' : ''"
        >
          Our Creators
        </router-link>

        <router-link
          to="/gallery"
          class="navbar-item nav-list-menu"
          :class="darkMode ? 'darkText' : ''"
        >
          Gallery
        </router-link>

        <router-link
          to="/how-it-works"
          class="navbar-item nav-list-menu"
          :class="darkMode ? 'darkText' : ''"
        >
          How It Works
        </router-link>
        <div class="navbar-item">
          <div class="nav-button buttons">
            <a
              class="button btn-connect"
              v-if="!wallet.connected"
              @click="connectWallet()"
            >
              <strong>Connect</strong>
            </a>
            <a
              class="button btn-connect"
              v-if="wallet.connected"
              @click="disconnectWallet()"
            >
              <strong>Disconnect</strong>
            </a>

            <a @click="toggleDarkMode()">
              <img
                :src="darkMode ? sunImg : moonImg"
                width="25"
                height="25"
                alt=""
              />
            </a>
          </div>
        </div>
      </div>
    </div>
  </nav>
</template>
<script>
import sunImg from "../assets/Sun.png";
import moonImg from "../assets/Moon.png";
import logo from "../assets/SuperFansOnly_Logo-Black-Background.svg";
import { useStore } from "vuex";
import { computed } from "@vue/reactivity";
export default {
  name: "Navbar",

  setup() {
    var img = darkMode ? sunImg : moonImg;
    const store = useStore();
    const darkMode = computed(() => store.state.darkMode);
    const wallet = computed(() => store.state.wallet);
    const toggleDarkMode = () => {
      store.dispatch("trackingEvents", {
        event: "CHANGE THEME",
        data: darkMode.value,
      });
      store.dispatch("toggleDarkMode");
      img = moonImg;
      let navbarBurger = document.getElementsByClassName("navbar-burger");
      if (navbarBurger.length > 0) {
        if (navbarBurger[0].classList.contains("is-active"))
          navbarBurger[0].classList.remove("is-active");
      }
    };
    const disconnectWallet = () => {
      store.dispatch("disconnectWallet");
    };
    const connectWallet = () => {
      store.dispatch("connectWallet");
    };

    // console.log(wallet);
    return {
      img,
      darkMode,
      toggleDarkMode,
      wallet,
      disconnectWallet,
      connectWallet,
    };
  },

  mounted() {
    // console.log("from the navbar", this.connected);

    document.addEventListener("DOMContentLoaded", () => {
      // Get all "navbar-burger" elements
      const $navbarBurgers = Array.prototype.slice.call(
        document.querySelectorAll(".navbar-burger"),
        0
      );
      // Check if there are any navbar burgers
      if ($navbarBurgers.length > 0) {
        // Add a click event on each of them
        $navbarBurgers.forEach((el) => {
          el.addEventListener("click", () => {
            // Get the target from the "data-target" attribute
            const target = el.dataset.target;
            const $target = document.getElementById(target);
            // Toggle the "is-active" class on both the "navbar-burger" and the "navbar-menu"
            // console.log("el.classList");
            // console.log(el.classList);
            el.classList.toggle("is-active");
            $target.classList.toggle("is-active");
          });
        });
      }
    });
  },
  data: () => {
    return {
      sunImg,
      moonImg,
      logo,
    };
  },
};
</script>

<style scoped>
.navbar-menu.is-active .navbar-end {
  width: 200px;
  margin: 0px auto;
  text-align: justify;
}
.navbar-item img {
  max-height: 125px;
  max-width: 125px;
  margin-right: 1rem;
  margin-left: 1rem;
}
@media screen and (max-width: 768px) {
  .navbar-item img {
    max-height: 80px;
    max-width: 80px;
  }

  .nav-button {
    justify-content: center;
  }

  .nav-list-menu {
    padding-left: 3rem;
  }
}
.navbar-burger {
  position: absolute;
  right: 5%;
}
</style>
