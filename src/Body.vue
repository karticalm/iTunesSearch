<template>
  <div id="body">
    <div class="search-area">
      <input type="text" name="Search" id="search" @change="storeSearchText" />
      <input type="button" value="Search" @click="callApi" />
    </div>
    <CountryShareBar
      :ind="percentages.ind"
      :can="percentages.can"
      :usa="percentages.usa"
    />
    <div id="search-display">
      <ol v-if="searchResults">
        <li v-for="(item, index) in searchResults" :key="index">
          <span style="font-weight: bold">{{ item.trackName }}</span> by
          {{ item.artistName }}
        </li>
      </ol>
    </div>
  </div>
</template>

<script>
import CountryShareBar from "./CountryShareBar";
export default {
  name: "Body",
  components: {
    CountryShareBar,
  },
  data() {
    return {
      searchText: null,
      searchResults: null,
      percentages: {
        ind: 33,
        can: 33,
        usa: 33,
      },
    };
  },

  beforeUpdate() {
    this.calculatePercentages();
  },

  methods: {
    calculatePercentages() {
      /**
       * Calculate total length of response
       * filter out based on country
       * Count and Store percentages
       */
      const result = this.searchResults;
      const total = result.length;
      const indCount = result.filter((item) => {
        return item.country === "IND";
      });
      const canCount = result.filter((item) => {
        return item.country === "CAN";
      });
      const usaCount = result.filter((item) => {
        return item.country === "USA";
      });
      this.percentages.ind = (indCount.length / total) * 100;
      this.percentages.can = (canCount.length / total) * 100;
      this.percentages.usa = (usaCount.length / total) * 100;
    },

    storeSearchText(e) {
      this.searchText = e.target.value;
    },
    async callApi(e) {
      /**
       * Take the search input
       * integrate and call the Api with that search input
       * parse and store the result in searchResults
       */
      const apiData = await fetch(
        `https://itunes.apple.com/search?term=${this.searchText}`
      );
      const jsonApiData = await apiData.json();
      this.searchResults = jsonApiData.results;
    },
  },
};
</script>

<style>
.share-bar {
  display: flex;
  margin: 5px;
}
.country-bar {
  padding: 5px;
}
#search-display {
  padding: 10px;
  margin: 5px;
}
.search-area {
  padding: 5px;
  margin: 5px;
}
</style>