<template>
  <nav>
    <div class="static-header">
      <a
        class="main-logo"
        href="/"
      >
        <span class="image-container">
          <img
            src="/images/logo.png"
            alt="Front.Social"
          >
        </span>
      </a>
      <Avatar
        class="mobile avatar"
        :user="signedInUser"
      />
      <div class="menu-icon">
        <i
          class="ion-ios-menu"
          @click="toggleNav"
        />
      </div>
    </div>
    <ul
      ref="nav"
      class="nav-items"
    >
      <li
        v-for="(link, index) in navLinks"
        :key="index"
        class="nav-item"
      >
        <router-link
          :to="link.path"
          class="nav-item-link"
        >
          {{ link.text }}
        </router-link>
      </li>
      <li
        :key="'signin'"
        class="nav-item right"
      >
        <div class="nav-item-link">
          <Avatar
            class="desktop"
            :user="signedInUser"
          />
          <div
            v-if="!signedInUser"
            class="nav-item-link mobile"
            @click="redirect('/signin')"
          >
            Sign in
          </div>
        </div>
      </li>
    </ul>
  </nav>
</template>

<script>
import store from "@/vuex/store";
import Avatar from "@/components/Header/Avatar";

export default {
  components: { Avatar },
  props: {
    navLinks: {
      type: Array,
      required: true
    }
  },
  computed: {
    signedInUser() {
      return this.$store.state.signedInUser;
    }
  },
  methods: {
    toggleNav() {
      const nav = this.$refs.nav.classList;
      nav.contains("active") ? nav.remove("active") : nav.add("active");
    },
    signout() {
      userService.signout();
      this.$store.commit("signInUser", null);
      this.redirect("/");
    },
    redirect(path) {
      this.toggleNav();
      this.$router.push(path);
    }
  }
};
</script>

<style scoped lang="scss">
.static-header {
  min-width: 200px;
  display: flex;
  .menu-icon {
    display: none;
  }

  .main-logo {
    cursor: pointer;
    display: flex;
    align-items: center;
    width: 280px;
    img {
      width: 100%;
    }
    .image-container {
      padding: 9px 10px 0 10px;
      background-color: white;
      height: 55px;
    }
  }
}

nav {
  display: flex;
  min-height: 55px;
  width: 100%;
  .nav-items {
    display: flex;
    flex-grow: 0.2;
    align-items: center;
    justify-content: space-between;
    margin: 0;
    width: 100%;
    figure {
      cursor: pointer;
      margin-right: 10px;
    }

    i {
      margin-right: 10px;
      font-size: 22px;
    }
    .nav-item {
      list-style-type: none;
      padding: 0 20px;
      font-size: 24px;
      cursor: pointer;
      .nav-item-link {
        color: #114273 !important;
        display: flex;
        flex-direction: row-reverse;
        align-items: center;
      }
      &:hover {
        font-weight: bold;
      }
    }
  }
}

.mobile {
  display: none !important;
}

@media screen and (max-width: 759px) {
  .desktop {
    display: none !important;
  }

  .mobile {
    display: flex !important;
  }

  .avatar {
    margin-top: 5px;
  }
  .static-header {
    display: flex;
    justify-content: space-between;
    width: 100%;
    background-color: white;
    height: 60px;
    position: fixed;
    top: 0;
    z-index: 20;
    .main-logo {
      display: flex;
      align-items: center;
      width: 245px;
      padding-left: 10px;
    }
    .menu-icon {
      display: flex;
      font-size: 45px;
      padding-right: 30px;
    }
  }
  nav {
    display: flex;
    flex-direction: column;
    .nav-items {
      display: none;
    }
    .nav-items.active {
      margin: 0;
      margin-top: 60px;
      display: flex;
      align-items: center;
      padding: 10px;
      flex-direction: column;
      font-weight: bold;
    }
  }
}
li.nav-item a.nav-item-link.exact-active.active {
  font-weight: bold;
  background-color: #11427334;
  text-decoration: none;
}
a.nav-item-link:hover {
  background-color: #11427334;
  text-decoration: none;
}
a.nav-item-link {
  padding: 10px 10px;
}
</style>
