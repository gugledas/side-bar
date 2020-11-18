<template>
  <div class="d-flex menu-vuejs" :class="sideStatus">
    <transition name="slide-fade">
      <div class="sidebar shadow-sm ">
        <div class="sidebar__header">
          <img :src="urlImageUser" alt="" class="rounded-circle" />
          <div class="header">
            <a href="#">
              <h4>{{ nameUser_display }}</h4>
            </a>
            <p><i class="fa fa-circle text-success mr-1"></i>connecté</p>
          </div>
        </div>
        <div class="sidebar__nav">
          <div
            class="sidebar__item "
            :class="[nav.active ? 'sidebar__item--active' : '']"
            v-for="(nav, i) in check"
            :key="i"
            @click="activeNav(i)"
          >
            <div class="sidebar__link d-flex justify-content-between">
              <a :href="nav.relative" class="titre-link">
                <span v-if="nav.options.fa_icon || nav.options.wbu_fa_icon">
                  <i
                    class="icone"
                    :class="[
                      nav.options.fa_icon_prefix,
                      nav.options.fa_icon,
                      nav.options.wbu_fa_icon
                    ]"
                  ></i>
                </span>
                {{ nav.title }}
              </a>
              <span @click="dropdownActive(i)">
                <i class="fa fa-angle-down drop" v-if="dropdownlength(i)"></i>
              </span>
            </div>
            <transition name="slide-fade">
              <div class="sidebar__dropdown" v-if="nav.droped">
                <ul>
                  <li
                    class="drop-item"
                    :class="drop.active ? 'drop-item--active' : ''"
                    v-for="(drop, index) in nav.below"
                    :key="index"
                  >
                    <i
                      v-if="drop.options.fa_icon || drop.options.wbu_fa_icon"
                      class="icone"
                      :class="[
                        drop.options.fa_icon_prefix,
                        drop.options.fa_icon,
                        drop.options.wbu_fa_icon
                      ]"
                    ></i>
                    <a
                      class="ml-2"
                      :href="drop.relative"
                      @click="linkActive(i, index)"
                      >{{ drop.title }}</a
                    >
                  </li>
                </ul>
              </div>
            </transition>
          </div>
        </div>
      </div>
    </transition>
    <b-navbar-brand
      href="#"
      class=" sidebar-hamburger"
      @click="changeStatutSideBar"
      ><span style="font-size:1.5em; color:#343a40;cursor: pointer">
        <i class="fa fa-bars"></i></span
    ></b-navbar-brand>
  </div>
</template>

<script>
var axios;
if (window.axios) {
  axios = window.axios;
}
const userActionMenu = "actionMenu";

export default {
  props: {
    url: {
      type: String
    },
    classParent: {
      type: String
    },
    urlImageUser: {
      type: String,
      default: "http://via.placeholder.com/160x160"
    },
    nameUser: {
      type: String,
      default: "Monsieur Dupont"
    }
  },
  mounted() {
    this.addGrandBlock();
    if (this.url) {
      console.log("voici l’url" + " " + this.url);
      this.loadMenu();
    }
    var modif = document.getElementsByClassName(this.classParent);
    var tab = modif[0].classList;
    var self = this;
    var loc = localStorage.getItem(userActionMenu);
    if (loc != null) {
      if (loc == "ouvert") {
        let func = function() {
          if (tab.contains("grand-bloc-normal") == true) {
            modif[0].classList.replace(
              "grand-bloc-normal",
              "grand-bloc-decaler"
            );
          } else {
            modif[0].classList.add("grand-bloc-decaler");
          }
          self.sideStatus = "side-open";
        };
        setTimeout(func, 1000);
      } else if (loc == "fermer") {
        if (tab.contains("grand-bloc-decaler") == true) {
          modif[0].classList.replace("grand-bloc-decaler", "grand-bloc-normal");
        } else {
          modif[0].classList.add("grand-bloc-normal");
        }
        this.sideStatus = "side-close";
      }
    } else {
      let func = function() {
        if (tab.contains("grand-bloc-normal") == true) {
          modif[0].classList.replace("grand-bloc-normal", "grand-bloc-decaler");
        } else {
          modif[0].classList.add("grand-bloc-decaler");
        }
        self.sideStatus = "side-open";
      };
      setTimeout(func, 1000);
    }
  },
  data: () => ({
    json: [],
    sideStatus: "side-close"
  }),

  computed: {
    check() {
      var currentUrl = window.location.pathname;
      var newe = this.newMenu;
      var check = function(newe) {
        newe.forEach((el, i) => {
          if (el.relative == currentUrl) {
            newe[i].active = true;
            newe[i].droped = true;
          }
          if (el.below !== undefined) {
            el.below = check(el.below);
            for (let act of el.below) {
              if (act.active == true) {
                newe[i].droped = true;
                console.log("un", currentUrl);
              }
            }
          }
        });
        return newe;
      };
      var nouveauMenu = check(newe);
      return nouveauMenu;
    },
    actif(i) {
      if (this.newMenu[i].actived === true) {
        return true;
      } else {
        return false;
      }
    },
    newMenu() {
      for (let item of this.json) {
        this.$set(item, "active", false);
        this.$set(item, "droped", false);
        if (item.below != undefined) {
          for (let nav of item.below) this.$set(nav, "active", false);
        }
      }
      return this.json;
    },
    nameUser_display: {
      get() {
        var string = this.nameUser.replace(/  +/g, " ");
        if (string.length > 12) {
          console.log("this.nameUser : ", string);
          return string.substr(0, 12) + ".";
        } else if (string.length > 0) {
          return string;
        }
        return "User";
      }
    }
  },
  methods: {
    addGrandBlock() {
      var grandBlock = document.getElementsByClassName(this.classParent);
      grandBlock[0].classList.add("grand-block");
    },
    activeNav(i) {
      this.newMenu.forEach(el => (el.active = false));
      this.newMenu[i].active = true;
    },
    changeStatutSideBar() {
      var modif = document.getElementsByClassName(this.classParent);
      var tab = modif[0].classList;
      if (this.sideStatus == "side-close") {
        if (tab.contains("grand-bloc-normal") == true) {
          modif[0].classList.replace("grand-bloc-normal", "grand-bloc-decaler");
        } else {
          modif[0].classList.add("grand-bloc-decaler");
        }
        console.log("1er", modif[0].classList);
        this.sideStatus = "side-open";
        localStorage.setItem(userActionMenu, "ouvert");
      } else {
        if (tab.contains("grand-bloc-decaler") == true) {
          modif[0].classList.replace("grand-bloc-decaler", "grand-bloc-normal");
        } else {
          modif[0].classList.add("grand-bloc-normal");
        }
        this.sideStatus = "side-close";
        localStorage.setItem(userActionMenu, "fermer");
      }
    },
    loadMenu() {
      var self = this;
      axios.get(this.url).then(function(response) {
        console.log("voici response", response);
        if (response.data) {
          self.json = response.data;
          console.log("self json", self.json);
        }
      });
    },
    dropdownlength(i) {
      var element = this.json[i];

      var newObject = Object.keys(element);
      if (newObject.indexOf("below") === -1) {
        return false;
      } else {
        return true;
      }
    },
    dropdownActive(i) {
      if (this.newMenu[i].droped === false) {
        if (this.newMenu[i].below) {
          this.newMenu.forEach(d => (d.droped = false));
          if (this.newMenu[i].below.length > 0) {
            this.newMenu[i].droped = true;
          }
        }
      } else {
        this.newMenu[i].droped = false;
      }
    },
    linkActive(i, index) {
      this.newMenu[i].below.forEach(el => (el.active = false));
      this.newMenu[i].below[index].active = true;
    }
  }
};
</script>
<style lang="scss">
.menu-vuejs {
  transition: all 0.2s;
  .sidebar {
    background-color: black;
    width: 300px;
    padding: 1rem 2px;
    overflow-y: scroll;
    .icone {
      margin-right: 0.3em;
    }

    &__dropdown {
      background-color: transparent;
      margin-top: 10px;
      display: flex;
      flex-direction: column;
      font-weight: bold;
      text-transform: uppercase;
      font-size: 0.8rem;
      a {
        color: white;
      }
      ul {
        list-style: none;
        margin-left: -14px;
        margin-bottom: 0;
      }
      .drop-item {
        padding: 5px 5px 7px 10px;
        display: flex;
        align-items: center;
        &--active {
          a {
            color: rgb(97, 220, 97);
          }
        }
      }
    }
    a {
      color: white;
      text-decoration: none;
      &:hover {
        color: rgb(97, 220, 97);
      }
    }
    &__header {
      font-size: 0.9rem;
      display: flex;
      justify-content: space-around;
      font-weight: bold;
      padding: 2rem 1rem 1rem;
      border-bottom: 1px solid white;
      color: white;
      i {
        color: rgb(37, 216, 37);
      }
      h4 {
        margin-top: 2px;
        font-size: 1.1rem;
      }

      img {
        width: 100%;
        max-width: 60px;
        max-height: 60px;
        height: auto;
        background: #848484;
        border: 1px solid #000;
      }
      .header {
        width: 130px;
        overflow: hidden;
      }
    }
    &__nav {
      margin-top: 2rem;
    }
    &__item {
      font-weight: bold;
      list-style: none;
      text-align: left;
      padding: 7px 6px;
      font-size: 0.9rem;
      border-radius: 5px;
      border-bottom-right-radius: 0px;
      cursor: pointer;
      margin-bottom: 4px;
      text-transform: uppercase;
      .drop {
        margin-right: 4px;
        margin-top: 2px;
        font-size: 18px;
      }

      &__link {
        color: black;
        &:hover {
          color: rgb(75, 192, 75);
        }
      }
      &--active {
        background-color: transparent;
        .titre-link {
          color: rgb(75, 192, 75);
        }
      }
      .fa-angle-down {
        font-weight: 800;
        margin-right: 0.9rem;
        margin-left: 0.2rem;
        color: rgb(75, 192, 75);
      }
      .slide-fade-enter-active {
        transition: all 0.2s ease;
      }
      .slide-fade-leave-active {
        transition: all 0.2s cubic-bezier(1, 0.5, 0.8, 1);
      }
      .slide-fade-enter, .slide-fade-leave-to
/* .slide-fade-leave-active below version 2.1.8 */ {
        transform: translateX(10px);
        opacity: 0;
      }
    }
  }
  .sidebar-hamburger {
    position: absolute;
    left: 300px;
    height: 64px;
    background: white;
    padding: 10px;
    box-shadow: 1px 1px 5px #ccc;
  }
}
.side-close {
  position: fixed;
  height: 100%;
  top: 0;
  left: -300px;
}
.side-open {
  position: fixed;
  height: 100%;
  top: 0;
  left: 0;
}
.grand-block {
  transition: all 0.2s;
}
.grand-bloc-decaler {
  position: relative;
  margin-left: 300px;
}
.grand-bloc-normal {
  margin-left: 0;
}
</style>
