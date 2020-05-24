<template>
  <div class="container">
    <div class="row justify-content-center">
      <p class="alert alert-primary" role="alert">
        <a
          href="https://en.wikipedia.org/wiki/Special:Random"
          target="_blank"
        >Click Here For Random Article</a>
      </p>
    </div>
    <div class="row justify-content-center">
      <div class="form-group">
        <input class="form-control" placeholder="Search" v-on:keyup.enter="wikiSearch">
        <small class="form-text text-muted">Press enter to get search results</small>
      </div>
    </div>
    <div class="row justify-content-center">
      <div
        class="card"
        style="width: 25rem;"
        v-for="result in searchResult"
        v-bind:key="result.title"
      >
        <div class="card-body">
          <h5 class="card-title">{{result.title}}</h5>
          <p>{{result.extract}}</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  data: function() {
    return {
      searchResult: {}
    };
  },
  methods: {
    wikiSearch(event) {
      // Fetch data from wikipedia
      fetch(
        `https://en.wikipedia.org/w/api.php?action=query&format=json&prop=extracts&generator=search&exsentences=1&exlimit=max&exintro=1&explaintext=1&gsrsearch=${
          event.target.value
        }&gsrlimit=5&origin=*`
      )
        // stream to json
        .then(stream => stream.json())
        // select wikipedia pages
        .then(result => result.query.pages)
        // create list of pages
        .then(pages => Object.keys(pages).map(key => pages[key]))
        // add pages to searchResult
        .then(list => (this.searchResult = list))
        // if error, log error
        .catch(error => console.error(error));
    }
  }
};
</script>

<style>
body {
  background-color: lightgrey;
}
</style>
