<template>
  <div class="list">
      <template v-for="issue in issues">
        <ListItem :key="issue.id" v-if="!issue.pull_request && issue.state=='open'" :issue="issue" />
      </template>
  </div>
</template>

<script>
import Axios from 'axios'
import ListItem from './ListItem'

  export default {
    components: {
      ListItem,
    },
    data() {
      return {
        issues: null,

      }
    },
    mounted () {
      this.getIssues()
    },
    methods: {
      getIssues() {
        Axios.get('https://api.github.com/repos/vuejs/vue/issues')
          .then( (response) => {
            let result = response.headers.link.match(/[^\<\>\"\;\,\s]+/gi);

            console.log(result);
            this.next = response.headers.link.match(/[^\<\>]+/gi)[0]
            this.last = response.headers.link.match(/[^\<\>]+/gi)[1]

            this.issues = response.data
          });
      }
    },
  }
</script>

<style lang="scss">
  .list {
    width: 80%;
    margin: 0 auto;
  }
</style>
