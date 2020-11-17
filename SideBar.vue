<template>
  <div class="d-flex menu-vuejs" :class="sideStatus">
    <transition name="slide-fade">
      <div class="sidebar shadow-sm ">
        <div class="sidebar__header">
          <img :src="urlImageUser" alt="" class="rounded-circle" />
          <div class="header">
            <a href="#">
              <h4>{{ nameUser }}</h4>
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
              <a :href="nav.relative">
                <div class="titre-link">
                  <span v-if="nav.options">
                    <i
                      class="text-success"
                      :class="[nav.options.fa_icon_prefix, nav.options.fa_icon]"
                    ></i>
                  </span>
                  {{ nav.title }}
                </div>
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
                      v-if="drop.options.fa_icon"
                      class="text-success"
                      :class="[
                        drop.options.fa_icon_prefix,
                        drop.options.fa_icon
                      ]"
                    ></i>
                    <a
                      class="ml-2"
                      :href="drop.relative"
                      @click="linkActive(i, index)"
                      :style="drop.active ? 'color: rgb(97, 220, 97);' : ''"
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
    json: [
      {
        key: "5b296ded-b082-4ab9-b831-d427b2a5b1f9",
        title: "Listes des emplois",
        description: null,
        uri: "recherche-emploi",
        alias: "recherche-emploi",
        external: false,
        absolute: "http://airportlome.kksa/recherche-emploi",
        relative: "/recherche-emploi",
        existing: true,
        weight: "-49",
        expanded: false,
        enabled: true,
        uuid: null,
        options: {
          query: []
        }
      },
      {
        key: "1262ed52-fd99-4855-bf27-6c5051977b67",
        title: "Publier un emploi",
        description: null,
        uri: "multistepvalidation/CheckRoute/emploi",
        alias: "multistepvalidation/CheckRoute/emploi",
        external: false,
        absolute:
          "http://airportlome.kksa/multistepvalidation/CheckRoute/emploi",
        relative: "/multistepvalidation/CheckRoute/emploi",
        existing: true,
        weight: "-47",
        expanded: false,
        enabled: true,
        uuid: null,
        options: {
          query: {
            go: "/node/add/jobs"
          }
        }
      },
      {
        key: "bf68be73-1b26-437f-b895-9de0f4ac4d9b",
        title: "Espace Employeurs",
        description: null,
        uri: "employeur",
        alias: "employeur",
        external: false,
        absolute: "http://airportlome.kksa/employeur",
        relative: "/employeur",
        existing: true,
        weight: "-46",
        expanded: false,
        enabled: true,
        uuid: null,
        options: {
          query: []
        }
      },
      {
        key: "bfa5735f-e8ed-44c5-9efe-9adca7ee260e",
        title: "Espace Candidat",
        description: null,
        uri: "candidats",
        alias: "candidats",
        external: false,
        absolute: "http://airportlome.kksa/candidats",
        relative: "/candidats",
        existing: true,
        weight: "-45",
        expanded: false,
        enabled: true,
        uuid: null,
        options: {
          query: []
        }
      },
      {
        key: "30848330-516e-4fa5-a5b8-1afa98965cd7",
        title: "Publier un CV",
        description: null,
        uri: "multistepvalidation/CheckRoute/identification_du_candidat",
        alias: "multistepvalidation/CheckRoute/identification_du_candidat",
        external: false,
        absolute:
          "http://airportlome.kksa/multistepvalidation/CheckRoute/identification_du_candidat",
        relative: "/multistepvalidation/CheckRoute/identification_du_candidat",
        existing: true,
        weight: "-44",
        expanded: false,
        enabled: true,
        uuid: null,
        options: {
          query: {
            go: "identification_du_candidat"
          }
        }
      },
      {
        key: "8f40c479-9d44-4ae1-9b91-cbc2dd8de1e0",
        title: "Liste des cv",
        description: null,
        uri: "",
        alias: "",
        external: false,
        absolute: "http://airportlome.kksa/",
        relative: "/",
        existing: true,
        weight: "-43",
        expanded: false,
        enabled: true,
        uuid: null,
        options: {
          fa_icon: "",
          fa_icon_prefix: "fa",
          fa_icon_tag: "i",
          fa_icon_appearance: "before"
        }
      },
      {
        key: "aaf04980-61b3-4cbe-b49c-74539cb253ee",
        title: "Actualités",
        description: null,
        uri: "",
        alias: "",
        external: false,
        absolute: "http://airportlome.kksa/",
        relative: "/",
        existing: true,
        weight: "-42",
        expanded: false,
        enabled: true,
        uuid: null,
        options: []
      },
      {
        key: "6b19adea-0092-46e2-9890-b7ae4cb411f6",
        title: "Formations",
        description: null,
        uri: "",
        alias: "",
        external: false,
        absolute: "http://airportlome.kksa/",
        relative: "/",
        existing: true,
        weight: "-41",
        expanded: false,
        enabled: true,
        uuid: null,
        options: {
          fa_icon: "",
          fa_icon_prefix: "fa",
          fa_icon_tag: "i",
          fa_icon_appearance: "before"
        },
        below: [
          {
            key: "33d1913c-f2fe-479e-b420-74cba7f86e1d",
            title: "Liste des formations",
            description: null,
            uri: "liste-des-formations",
            alias: "liste-des-formations",
            external: false,
            absolute: "http://airportlome.kksa/liste-des-formations",
            relative: "/liste-des-formations",
            existing: true,
            weight: "-50",
            expanded: false,
            enabled: true,
            uuid: null,
            options: {
              fa_icon: "",
              fa_icon_prefix: "fa",
              fa_icon_tag: "i",
              fa_icon_appearance: "before",
              query: []
            }
          }
        ]
      },
      {
        key: "b68348ef-862c-4c37-a36a-cb235d13dc3c",
        title: "Liste des entreprises",
        description: null,
        uri: "liste-des-entreprises",
        alias: "liste-des-entreprises",
        external: false,
        absolute: "http://airportlome.kksa/liste-des-entreprises",
        relative: "/liste-des-entreprises",
        existing: true,
        weight: "-39",
        expanded: false,
        enabled: true,
        uuid: null,
        options: {
          fa_icon: "",
          fa_icon_prefix: "fa",
          fa_icon_tag: "i",
          fa_icon_appearance: "before",
          query: []
        },
        below: [
          {
            key: "922a156a-181c-4cc3-a301-9e5e8e35f97d",
            title: "Ajouter son entreprise",
            description: null,
            uri: "node/add/prestataires",
            alias: "node/add/prestataires",
            external: false,
            absolute: "http://airportlome.kksa/node/add/prestataires",
            relative: "/node/add/prestataires",
            existing: true,
            weight: "-50",
            expanded: false,
            enabled: true,
            uuid: "18d668ff-5bae-4b3e-ad0a-94ff73012d7d",
            options: {
              fa_icon: "",
              fa_icon_prefix: "fa",
              fa_icon_tag: "i",
              fa_icon_appearance: "before",
              query: []
            }
          }
        ]
      },
      {
        key: "9838573b-3a46-412d-bf76-88a961666641",
        title: "Appels d'offre",
        description: null,
        uri: "",
        alias: "",
        external: false,
        absolute: "http://airportlome.kksa/",
        relative: "/",
        existing: true,
        weight: "-37",
        expanded: false,
        enabled: true,
        uuid: null,
        options: {
          fa_icon: "",
          fa_icon_prefix: "fa",
          fa_icon_tag: "i",
          fa_icon_appearance: "before"
        },
        below: [
          {
            key: "fd88d949-0078-4541-aebf-bf5e64ec4e6a",
            title: "liste des appels d offre",
            description: null,
            uri: "liste-des-appels-d-offres",
            alias: "liste-des-appels-d-offres",
            external: false,
            absolute: "http://airportlome.kksa/liste-des-appels-d-offres",
            relative: "/liste-des-appels-d-offres",
            existing: true,
            weight: "-50",
            expanded: false,
            enabled: true,
            uuid: null,
            options: {
              fa_icon: "",
              fa_icon_prefix: "fa",
              fa_icon_tag: "i",
              fa_icon_appearance: "before",
              query: []
            }
          },
          {
            key: "d28409fc-ecf9-4058-a94f-59ea36b1b222",
            title: "Ajouter un appel d'offre",
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
              query: []
            }
          }
        ]
      },
      {
        key: "dfce21e8-f002-4cdc-8a50-9af2095e6b75",
        title: "Vendre son entreprise",
        description: null,
        uri: "",
        alias: "",
        external: false,
        absolute: "http://airportlome.kksa/",
        relative: "/",
        existing: true,
        weight: "-36",
        expanded: false,
        enabled: true,
        uuid: null,
        options: [],
        below: [
          {
            key: "de62c7fa-5163-4473-93a3-69ecb016dffc",
            title: "Liste des entreprises à vendre",
            description: null,
            uri: "liste-des-entreprises",
            alias: "liste-des-entreprises",
            external: false,
            absolute: "http://airportlome.kksa/liste-des-entreprises",
            relative: "/liste-des-entreprises",
            existing: true,
            weight: "-50",
            expanded: false,
            enabled: true,
            uuid: null,
            options: {
              query: []
            }
          },
          {
            key: "4061eae5-aa7f-453f-8174-14bf59bf38aa",
            title: "Ajouter une entreprise à vendre",
            description: null,
            uri: "multistepvalidation/CheckRoute/emploi",
            alias: "multistepvalidation/CheckRoute/emploi",
            external: false,
            absolute:
              "http://airportlome.kksa/multistepvalidation/CheckRoute/emploi",
            relative: "/multistepvalidation/CheckRoute/emploi",
            existing: true,
            weight: "-49",
            expanded: false,
            enabled: true,
            uuid: null,
            options: {
              query: {
                go: "/node/add/vendre_son_entreprise"
              }
            }
          }
        ]
      },
      {
        key: "b1bd6da5-eaaf-4cf4-a002-f4e08c16563a",
        title: "Hotels",
        description: null,
        uri: "liste-des-hotels",
        alias: "liste-des-hotels",
        external: false,
        absolute: "http://airportlome.kksa/liste-des-hotels",
        relative: "/liste-des-hotels",
        existing: true,
        weight: "-35",
        expanded: false,
        enabled: true,
        uuid: null,
        options: {
          query: []
        }
      },
      {
        key: "45f7eaf4-c2f3-4b8c-9bfe-a9bc810f2e13",
        title: "Publicité",
        description: null,
        uri: "liste-des-publicite",
        alias: "liste-des-publicite",
        external: false,
        absolute: "http://airportlome.kksa/liste-des-publicite",
        relative: "/liste-des-publicite",
        existing: true,
        weight: "-34",
        expanded: false,
        enabled: true,
        uuid: null,
        options: {
          query: []
        }
      },
      {
        key: "4ebb167c-5973-458b-a937-6ee6ddeb58d8",
        title: "Prix",
        description: null,
        uri: "node/507",
        alias: "page-de-base/prix",
        external: false,
        absolute: "http://airportlome.kksa/page-de-base/prix",
        relative: "/page-de-base/prix",
        existing: true,
        weight: "-33",
        expanded: false,
        enabled: true,
        uuid: "b26ffefb-2b98-49c6-88de-e8f3f6fddbee",
        options: []
      }
    ],

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
          }
          if (el.below !== undefined) {
            el.below = check(el.below);
            for (let act of el.below) {
              if (act.relative == currentUrl) {
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

    &__dropdown {
      background-color: transparent;
      margin-top: 10px;
      color: black;
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
        margin-left: -20px;
        margin-bottom: 0;
      }
      .drop-item {
        color: black !important;
        padding: 5px 5px 7px 10px;
        display: flex;
        &--active {
          color: rgb(97, 220, 97);
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
      &:hover {
        color: white;
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
      span {
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
