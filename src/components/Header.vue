<template>
  <div class = "container-fluid">
    <nav class="navbar is-dark is-fullwidth">
      <div class="navbar-brand">
        <h1 class = "is-size-1" @click="topHeadlines">News App</h1>
        <div class="navbar-burger burger" @click="active = !active" v-bind:class="{ 'is-active': active }" >
          <span></span>
          <span></span>
          <span></span>
        </div>
      </div>

      <div class="navbar-menu" v-bind:class="{ 'is-active': active }">
        <div class="navbar-start">

        </div>

        <div class="navbar-end">
          <div class ="navbar-item">
            <button type = "button" class = "button is-info" @click="topHeadlines">View Top Headlines</button>
          </div>
          <div class = "navbar-item">
            <!--Pick category-->
              <div class="field has-addons">
                <div class="control">
                  <div class="select ">
                    <select name="categories" v-for = "category in categories" v-model = "category.selectedOption">
                      <option v-for="option in category.options" :value="option">{{option}}</option>
                    </select>
                  </div>
                </div>
                <div class="control">
                  <button type="button" class="button is-info" @click="chooseCategory">Filter by category</button>
                </div>
              </div>
          </div>
          <div class="navbar-item">
            <!--Search using keyword-->
            <div class="field has-addons">
              <div class="control">
                <input class="input" type="text" placeholder= "Search using a keyword" v-model="keyword">
              </div>
              <div class="control">
                <a class="button is-info" type = "button" @click="submitKeyword">
                  Search
                </a>
              </div>
            </div>
          </div>
        </div>
      </div>
    </nav>
  </div>

</template>

<style>

  .navbar-brand:hover {
    cursor: pointer;
  }
  h1 {
    padding-left: 20px;
  }


  .label {
    color: #fff;
  }
</style>

<script>



  import { EventBus } from './event-bus.js';

  export default {
    data() {
      return {
        active: false,
        categories: [
          {
            options: ['Business','Entertainment','General','Health','Science','Sports','Technology'],
            selectedOption: "Business"
          }
        ],
        categoryChosen: "",
        keyword: ""

      }

    },
    methods: {
      topHeadlines() {
        EventBus.$emit('top-headlines');
      },
      chooseCategory() {
        this.categoryChosen = this.categories[0].selectedOption;
        EventBus.$emit('chose-category', this.categoryChosen );
      },
      submitKeyword() {
        EventBus.$emit('chose-keyword', this.keyword );
      }

    }
  }
</script>
