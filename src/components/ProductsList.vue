<template>
  <div>
    <!-- 
    Filters, search bar
    -->
    <div id="search-container">
      <div id="search-box">
        <input id="search-input" placeholder="Search..." name="title" v-model="stitle" />
        <md-button v-on:click="retrieveProducts" class="md-icon-button" id="search-icon">
          <md-icon>search</md-icon>
        </md-button>
      </div>
    </div>
    <!-- 
        Filters
    -->
    <div class="md-layout md-gutter" id="filters">
      <div class="md-layout-item">
        <md-field id="sem-filter">
          <label for="sem">Semester</label>
          <md-select v-model="ssem" name="sem" id="sem">
            <md-option v-for="semester in semesters" :value="semester" :key="semester">{{semester}}</md-option>
          </md-select>
        </md-field>
      </div>

      <div class="md-layout-item">
        <md-field id="branch-filter">
          <label for="branch">Branch</label>
          <md-select v-model="sbranch" name="branch" id="branch">
            <md-option v-for="branch in branches" :value="branch" :key="branch">{{branch}}</md-option>
          </md-select>
        </md-field>
      </div>

      <div class="md-layout-item">
        <md-field id="donation-filter">
          <label for="donation">Type</label>
          <md-select v-model="sdonation" name="donation" id="donation">
            <md-option value="1">Donation</md-option>
            <md-option value="0">Trade</md-option>
          </md-select>
        </md-field>
      </div>
      <!-- 
          Filter Buttons 
      -->
      <div class="md-layout-item" id="filter-buttons">
        <md-button v-on:click="retrieveProducts" class="md-primary">FILTER</md-button>
        <md-button v-on:click="clearSearch" class="md-accent">CLEAR</md-button>
      </div>
    </div>

    <!-- 
    Card layout
    -->

    <!-- <div class="md-layout md-alignment-bottom-center" id="container"> -->
    <div id="container">
      <div v-for="(product,index) in products" :key="index">
        <!-- 
        Book card layout
        -->
        <transition name="fade">
          <div v-if="product.book">
            <md-card class="rounded-card">
              <md-ripple>
                <md-card-media>
                  <a :href="'/product/' + product.id">
                    <img
                      v-bind:src="'http://localhost:8080/'+product.book.image"
                      alt="product.title"
                    />
                  </a>
                </md-card-media>

                <md-card-header>
                  <div class="md-title">
                    {{product.title}}
                    <md-chip class="donation-chip md-primary" v-if="product.donation">D</md-chip>
                  </div>
                  <div class="md-subhead">by {{product.book.author}}</div>
                </md-card-header>

                <md-card-expand>
                  <md-card-actions md-alignment="space-between">
                    <div>
                      <md-chip>{{product.sem}}</md-chip>
                      <md-chip>{{product.branch}}</md-chip>
                    </div>
                    <md-card-expand-trigger>
                      <md-button class="md-icon-button">
                        <md-icon>keyboard_arrow_down</md-icon>
                      </md-button>
                    </md-card-expand-trigger>
                  </md-card-actions>

                  <md-card-expand-content>
                    <md-card-content>
                      Published by {{product.book.publisher}}
                      <br />
                      Contact: {{product.book.phone}}
                      <br />
                      Posted at: {{product.createdAt}}
                      <br />
                    </md-card-content>
                  </md-card-expand-content>
                </md-card-expand>
              </md-ripple>
            </md-card>
          </div>

          <!-- 
        Drive card layout
          -->
          <div v-if="product.drive">
            <md-card class="rounded-card">
              <md-ripple>
                <md-card-header>
                  <a :href="'/product/' + product.id">
                    <div class="md-title">{{product.title}}</div>
                    <div class="md-subhead">{{product.drive.description}}</div>
                  </a>
                </md-card-header>

                <md-card-expand>
                  <md-card-actions md-alignment="space-between">
                    <div>
                      <md-chip>{{product.sem}}</md-chip>
                      <md-chip>{{product.branch}}</md-chip>
                    </div>
                    <md-card-expand-trigger>
                      <md-button class="md-icon-button">
                        <md-icon>keyboard_arrow_down</md-icon>
                      </md-button>
                    </md-card-expand-trigger>
                  </md-card-actions>

                  <md-card-expand-content>
                    <md-card-content>
                      Links to: {{product.drive.url}}
                      <br />
                      Posted at: {{product.createdAt}}
                      <br />
                    </md-card-content>
                  </md-card-expand-content>
                </md-card-expand>
              </md-ripple>
            </md-card>
          </div>

          <!-- 
        Other card layout
          -->
          <div v-if="product.other">
            <md-card class="rounded-card">
              <md-ripple>
                <md-card-media>
                  <a :href="'/product/' + product.id">
                    <img
                      v-bind:src="'http://localhost:8080/'+product.other.image"
                      alt="product.title"
                    />
                  </a>
                </md-card-media>

                <md-card-header>
                  <div class="md-title">
                    {{product.title}}
                    <md-chip class="donation-chip md-primary" v-if="product.donation">D</md-chip>
                  </div>

                  <div class="md-subhead">{{product.other.description}}</div>
                </md-card-header>

                <md-card-expand>
                  <md-card-actions md-alignment="space-between">
                    <div>
                      <md-chip>{{product.sem}}</md-chip>
                      <md-chip>{{product.branch}}</md-chip>
                    </div>
                    <md-card-expand-trigger>
                      <md-button class="md-icon-button">
                        <md-icon>keyboard_arrow_down</md-icon>
                      </md-button>
                    </md-card-expand-trigger>
                  </md-card-actions>

                  <md-card-expand-content>
                    <md-card-content>
                      Contact: {{product.other.phone}}
                      <br />
                      Posted at: {{product.createdAt}}
                      <br />
                    </md-card-content>
                  </md-card-expand-content>
                </md-card-expand>
              </md-ripple>
            </md-card>
          </div>
        </transition>
      </div>
    </div>

    <!-- End card layouts -->
  </div>
</template>
 
<script>
import http from "../http-common";

export default {
  name: "products-list",
  data() {
    return {
      stitle: "",
      ssem: "",
      sbranch: "",
      sdonation: "",
      products: [],
      semesters: ["1", "2", "3", "4", "5", "6", "7", "8"],
      branches: [
        "CSE",
        "ISE",
        "ECE",
        "MECH",
        "CIV",
        "BIO",
        "MED",
        "ELEC",
        "ARCH"
      ]
    };
  },
  methods: {
    /* eslint-disable no-console */
    retrieveProducts() {
      http
        .get(
          "/things?title=" +
            this.stitle +
            "&sem=" +
            this.ssem +
            "&branch=" +
            this.sbranch +
            "&donation=" +
            this.sdonation
        )
        .then(response => {
          this.products = response.data; // JSON are parsed automatically.
          console.log(response.data);
        })
        .catch(e => {
          console.log(e);
        });
    },
    refreshList() {
      this.retrieveProducts();
    },
    clearSearch() {
      (this.stitle = ""),
        (this.ssem = ""),
        (this.sbranch = ""),
        (this.sdonation = "");
      this.retrieveProducts();
    }
  },
  mounted() {
    this.retrieveProducts();
  }
};
</script>
 
<style scoped>
#container {
  display: flex;
  flex-flow: row wrap;
  justify-content: center;
  align-content: stretch;
}

#search-container {
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: center;
  margin: 40px 20px 20px 20px;
}

#search-box {
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: center;
  border: none;
  border-radius: 5px;
  transition: width 0.4s ease;
  background: white;
  box-shadow: 0 7px 30px -10px rgba(150, 170, 180, 0.6);
  outline: none;
}

#search-input {
  font-family: "archiaregular" !important;
  width: 240px;
  position: relative;
  padding: 15px 40px 15px 20px;
  color: black;
  text-transform: uppercase;
  font-size: 16px;
  font-weight: 100;
  letter-spacing: 2px;
  border: none;
  border-radius: 5px;
  background: none;
  transition: width 0.4s ease;
  outline: none;
}

#search-input:focus {
  width: 320px;
}

#filter-buttons {
  display: flex;
  flex-direction: row;
  align-items: center;
}
</style>