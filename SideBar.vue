<template>
  <div class="d-flex menu-vuejs" :class="sideStatus">
    <div class="sidebar shadow-sm ">
      <vuescroll :ops="ops">
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
      </vuescroll>
      <a class="log-out" :href="logOutLink"
        ><span><i :class="logOutIcon"></i></span> {{ logOutTitle }}</a
      >
    </div>
    <b-navbar-brand
      href="#"
      class=" sidebar-hamburger"
      @click="changeStatutSideBar"
      v-if="CroixRouge"
      ><span style="font-size:1.5em; color:#343a40;cursor: pointer">
        <i class="fa fa-bars"></i></span
    ></b-navbar-brand>
    <span class="croix-rouge" @click="changeStatutSideBar"
      ><i class="fas fa-times"></i
    ></span>
  </div>
</template>

<script>
import vuescroll from "vuescroll";
var axios;
if (window.axios) {
  axios = window.axios;
}
const userActionMenu = "actionMenu";

export default {
  components: {
    vuescroll
  },
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
    },
    logOutLink: {
      type: String,
      default: "#"
    },
    logOutTitle: {
      type: String,
      default: "Se déconnecter"
    },
    logOutIcon: {
      type: String,
      default: "fas fa-power-off"
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
    json: [
      {
        key: "multistepvalidation.my_account",
        title: "Tableau de bord",
        description: "",
        uri: "user/1",
        alias: "user/1",
        external: false,
        absolute: "http://airportlome.kksa/user/1",
        relative: "/user/1",
        existing: true,
        weight: "-51",
        expanded: false,
        enabled: true,
        uuid: "9e8251c7-1c11-4b08-852e-337dc28bd93c",
        options: {
          wbu_fa_icon: "fas fa-tachometer-alt",
          fa_icon_appearance: "before"
        },
        below: [
          {
            key: "b2211112-60ed-4e28-8f98-38bf03e128dc",
            title: "Proposer  un emploi",
            description: null,
            uri: "node/add/jobs",
            alias: "node/add/jobs",
            external: false,
            absolute: "http://airportlome.kksa/node/add/jobs",
            relative: "/node/add/jobs",
            existing: true,
            weight: "-50",
            expanded: false,
            enabled: true,
            uuid: "b5d88bae-d4c9-4273-8f09-540914769c95",
            options: {
              wbu_fa_icon: "far fa-address-book",
              fa_icon_appearance: "before",
              query: []
            }
          },
          {
            key: "1ad90ed3-b8c3-4c32-961b-d1cafee765fd",
            title: "Proposer un appel d'offre",
            description: null,
            uri: "node/add/appel_d_offres",
            alias: "node/add/appel_d_offres",
            external: false,
            absolute: "http://airportlome.kksa/node/add/appel_d_offres",
            relative: "/node/add/appel_d_offres",
            existing: true,
            weight: "-49",
            expanded: false,
            enabled: true,
            uuid: "a76f3c4e-3985-46b1-a7db-4a5b7ce6dc68",
            options: {
              wbu_fa_icon: "fas fa-network-wired",
              fa_icon_appearance: "before",
              query: []
            }
          },
          {
            key: "b4c736a1-e41d-4df0-ac08-9f8081dbaabb",
            title: "Proposer une formations",
            description: null,
            uri: "node/add/formations",
            alias: "node/add/formations",
            external: false,
            absolute: "http://airportlome.kksa/node/add/formations",
            relative: "/node/add/formations",
            existing: true,
            weight: "-48",
            expanded: false,
            enabled: true,
            uuid: "03c6bd77-f1db-40de-a0fd-649146faf9b6",
            options: {
              wbu_fa_icon: "fas fa-graduation-cap",
              fa_icon_appearance: "before",
              query: []
            }
          },
          {
            key: "762b76fb-fc61-4e75-abb8-8357fc7aeb60",
            title: "Proposer un hôtel",
            description: null,
            uri: "node/add/hotel",
            alias: "node/add/hotel",
            external: false,
            absolute: "http://airportlome.kksa/node/add/hotel",
            relative: "/node/add/hotel",
            existing: true,
            weight: "-46",
            expanded: false,
            enabled: true,
            uuid: "b01e8abc-de12-401f-b7ce-0df6a3df2325",
            options: {
              wbu_fa_icon: "fas fa-school ",
              fa_icon_appearance: "before",
              query: []
            }
          },
          {
            key: "a3fbe589-c041-44d8-bb74-ec9bc57a0822",
            title: "Proposer  une publicité",
            description: null,
            uri: "node/add/publicite",
            alias: "node/add/publicite",
            external: false,
            absolute: "http://airportlome.kksa/node/add/publicite",
            relative: "/node/add/publicite",
            existing: true,
            weight: "-45",
            expanded: false,
            enabled: true,
            uuid: "9ee019fc-8b9d-4608-933a-2da221f6baa6",
            options: {
              wbu_fa_icon: "fas fa-ad",
              fa_icon_appearance: "before",
              query: []
            }
          },
          {
            key: "966cbab7-5937-433b-b0f1-22a518930a0c",
            title: "Mes payements",
            description: null,
            uri: "multistepvalidation/payements",
            alias: "multistepvalidation/payements",
            external: false,
            absolute: "http://airportlome.kksa/multistepvalidation/payements",
            relative: "/multistepvalidation/payements",
            existing: true,
            weight: "-44",
            expanded: false,
            enabled: true,
            uuid: null,
            options: {
              wbu_fa_icon: "fas fa-euro-sign",
              fa_icon_appearance: "before",
              query: []
            }
          },
          {
            key: "multistepvalidation.user_cv",
            title: "Ajouter mon cv",
            description: "",
            uri: "user/1/identification_du_candidat",
            alias: "user/1/identification_du_candidat",
            external: false,
            absolute:
              "http://airportlome.kksa/user/1/identification_du_candidat",
            relative: "/user/1/identification_du_candidat",
            existing: true,
            weight: "-42",
            expanded: false,
            enabled: true,
            uuid: "9e8251c7-1c11-4b08-852e-337dc28bd93c",
            options: {
              wbu_fa_icon: "far fa-address-book",
              fa_icon_appearance: "before"
            }
          },
          {
            key: "1c8008db-2f89-452a-b2bd-e5bd4c4edaf9",
            title: "Postuler a des emplois",
            description: null,
            uri: "recherche-emploi",
            alias: "recherche-emploi",
            external: false,
            absolute: "http://airportlome.kksa/recherche-emploi",
            relative: "/recherche-emploi",
            existing: true,
            weight: "-41",
            expanded: false,
            enabled: true,
            uuid: null,
            options: {
              wbu_fa_icon: "far fa-edit",
              fa_icon_appearance: "before",
              query: []
            }
          },
          {
            key: "9e629704-f775-486c-804c-4db489ed645d",
            title: "Liste des emplois postules",
            description: null,
            uri: "multistepvalidation/listes-emploies-postules",
            alias: "multistepvalidation/listes-emploies-postules",
            external: false,
            absolute:
              "http://airportlome.kksa/multistepvalidation/listes-emploies-postules",
            relative: "/multistepvalidation/listes-emploies-postules",
            existing: true,
            weight: "-40",
            expanded: false,
            enabled: true,
            uuid: null,
            options: {
              wbu_fa_icon: "fas fa-user-edit",
              fa_icon_appearance: "before",
              query: []
            }
          },
          {
            key: "2fc88ddc-cf72-459f-9abe-d6ec475118cd",
            title: "Participer a des formations",
            description: null,
            uri: "liste-des-formations",
            alias: "liste-des-formations",
            external: false,
            absolute: "http://airportlome.kksa/liste-des-formations",
            relative: "/liste-des-formations",
            existing: true,
            weight: "-39",
            expanded: false,
            enabled: true,
            uuid: null,
            options: {
              wbu_fa_icon: "fas fa-layer-group",
              fa_icon_appearance: "before",
              query: []
            }
          },
          {
            key: "657ce10b-766f-44b9-81ef-4ceb4a74c27e",
            title: "Activer des alertes pour recevoir les publications",
            description: null,
            uri: "multistepvalidation/notifications-alerts",
            alias: "multistepvalidation/notifications-alerts",
            external: false,
            absolute:
              "http://airportlome.kksa/multistepvalidation/notifications-alerts",
            relative: "/multistepvalidation/notifications-alerts",
            existing: true,
            weight: "-38",
            expanded: false,
            enabled: true,
            uuid: null,
            options: {
              wbu_fa_icon: "fas fa-user-clock",
              fa_icon_appearance: "before",
              query: []
            }
          },
          {
            key: "e68b5a4f-c06c-4ebc-a716-c1f5ada3904b",
            title: "proposer des services comme prestataire",
            description: null,
            uri: "node/add/prestataires",
            alias: "node/add/prestataires",
            external: false,
            absolute: "http://airportlome.kksa/node/add/prestataires",
            relative: "/node/add/prestataires",
            existing: true,
            weight: "-37",
            expanded: false,
            enabled: true,
            uuid: "18d668ff-5bae-4b3e-ad0a-94ff73012d7d",
            options: {
              wbu_fa_icon: "fas fa-server",
              fa_icon_appearance: "before",
              query: []
            }
          },
          {
            key: "e3b1d40d-eb4c-4862-927f-0925cfba3973",
            title: "Vendre son entreprise",
            description: null,
            uri: "node/add/vendre_son_entreprise",
            alias: "node/add/vendre_son_entreprise",
            external: false,
            absolute: "http://airportlome.kksa/node/add/vendre_son_entreprise",
            relative: "/node/add/vendre_son_entreprise",
            existing: true,
            weight: "-36",
            expanded: false,
            enabled: true,
            uuid: "a1f11dbf-dfb2-4e66-9bd2-73c97ad75ec6",
            options: {
              wbu_fa_icon: "fas fa-store-alt",
              fa_icon_appearance: "before",
              query: []
            }
          },
          {
            key: "73df5571-d1d8-4de9-810d-9c55713084f2",
            title: "Facturation",
            description: null,
            uri: "multistepvalidation/facturation",
            alias: "multistepvalidation/facturation",
            external: false,
            absolute: "http://airportlome.kksa/multistepvalidation/facturation",
            relative: "/multistepvalidation/facturation",
            existing: true,
            weight: "-35",
            expanded: false,
            enabled: true,
            uuid: null,
            options: {
              wbu_fa_icon: "fas fa-id-card",
              fa_icon_appearance: "before",
              query: []
            }
          },
          {
            key: "user.logout",
            title: "Se déconnecter",
            description: "",
            uri: "user/logout",
            alias: "user/logout",
            external: false,
            absolute: "http://airportlome.kksa/user/logout",
            relative: "/user/logout",
            existing: true,
            weight: "-33",
            expanded: false,
            enabled: true,
            uuid: null,
            options: {
              wbu_fa_icon: "fas fa-power-off",
              fa_icon_appearance: "before"
            }
          }
        ]
      },
      {
        key: "f7777d8b-687d-4887-bb84-0aa15f3edd0e",
        title: "Se connecter",
        description: null,
        uri: "user/login",
        alias: "user/login",
        external: false,
        absolute: "http://airportlome.kksa/user/login",
        relative: "/user/login",
        existing: true,
        weight: "0",
        expanded: false,
        enabled: true,
        below: [
          {
            key: "user.logout",
            title: "Se necter",
            description: "",
            uri: "user/logout",
            alias: "user/logout",
            external: false,
            absolute: "http://airportlome.kksa/user/logout",
            relative: "/user/logo",
            existing: true,
            weight: "-33",
            expanded: false,
            enabled: true,
            uuid: null,
            options: {
              wbu_fa_icon: "fas fa-power-off",
              fa_icon_appearance: "before"
            }
          },
          {
            key: "user.logout",
            title: "Se déconnecter",
            description: "",
            uri: "user/logout",
            alias: "user/logout",
            external: false,
            absolute: "http://airportlome.kksa/user/logout",
            relative: "/user/logouto",
            existing: true,
            weight: "-33",
            expanded: false,
            enabled: true,
            uuid: null,
            options: {
              wbu_fa_icon: "fas fa-power-off",
              fa_icon_appearance: "before"
            }
          }
        ],
        uuid: null,
        options: {
          query: []
        }
      }
    ],
    ops: {
      scrollPanel: {
        scrollingX: false,
        speed: 2000,
        easing: "easeInOutCubic",
        verticalNativeBarPos: "right"
      },
      rail: { size: "15px" },
      bar: { background: "#ffffff", opacity: 0.4, minSize: 0.4 },
      scrollButton: {
        enable: false,
        background: "#cecece",
        opacity: 1,
        step: 180,
        mousedownStep: 30
      }
    },
    sideStatus: "side-close"
  }),

  computed: {
    CroixRouge() {
      if (this.sideStatus == "side-close") {
        return true;
      } else {
        return false;
      }
    },
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
  .croix-rouge {
    position: relative;
    font-size: 1.7rem;
    color: crimson;
    top: 8px;
    right: 40px;
  }
  .sidebar {
    min-height: 100vh;
    background-color: black;
    width: 300px;
    padding: 1rem 2px;
    .log-out {
      text-align: left;
      font-size: 0.79rem;
      position: relative;
      width: 280px;
      left: 12px;
      display: block;
      bottom: 23px;
      text-transform: uppercase;
      background-color: black;
      padding-top: 10px;
      font-weight: bold;
      span {
        color: crimson;
        margin-right: 8px;
      }
    }
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
      padding-bottom: 1.5rem;
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
