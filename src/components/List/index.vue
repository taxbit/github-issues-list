<template>
  <div class="list">

      <div v-if="isLoading" class="spinner"></div>

      <template v-for="issue in issues">
        <ListItem
          v-if="!issue.pull_request && issue.state=='open' && !isLoading"
          :key="issue.id" 
          :issue="issue" />
      </template>

      <div class="list__pager">
        <button 
          v-for="link in links" 
          :key="link.page + link.rel"
          class="list__pager-button" 
          @click="goPage(link.url)">
          {{ link.rel }}
        </button>
      </div>

  </div>
</template>

<script>
import Axios from 'axios'
import ListItem from './ListItem'
import parseLink from 'parse-link-header'

  export default {
    components: {
      ListItem,
    },
    data() {
      return {
        issues: null,
        links: {},
        isLoading: false,
      }
    },
    mounted () {
      this.getIssues('https://api.github.com/repos/vuejs/vue/issues')
    },
    methods: {
      getIssues(link) {
        this.isLoading = true
        Axios.get(link)
          .then( (response) => {
            this.links = parseLink(response.headers.link)
            this.issues = response.data
            this.isLoading = false
          })
          .catch((error) => {
            console.log('Что-то пошло не так:',error)
          })
      },
      goPage(link) {
        this.getIssues(link)
      }
    },
  }
</script>

<style lang="scss">
  .list {
    width: 80%;
    margin: 0 auto;

    &__pager {
      display: flex;
      height: 50px;
      justify-content: center;
      align-items: start;

      &-button {
        border: 1px solid black;
        border-radius: 4px;
        cursor: pointer;
        outline: none;
        &:not(:last-of-type) {
          margin-right: 5px;
        }
      }
    }
  }

  .spinner {
    display: inline-block;
    width: 50px;
    height: 50px;
    border: 3px solid rgba(255,255,255,.3);
    border-radius: 50%;
    border-top-color: rgb(78, 78, 78);
    animation: spin 1s ease-in-out infinite;
    -webkit-animation: spin 1s ease-in-out infinite;
    margin-top: 10vh;
    margin-bottom: 10vh;
  }

  @keyframes spin {
    to { -webkit-transform: rotate(360deg); }
  }
  @-webkit-keyframes spin {
    to { -webkit-transform: rotate(360deg); }
  }
</style>
