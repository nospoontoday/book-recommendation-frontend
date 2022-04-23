<template>
  <div class="home">
    <ApolloQuery :query ="require('@/graphql/queries/Categories.gql')">
      <template slot-scope="{ result: { data, loading } }">
        <div v-if="loading">Loading . . .</div>
        <div v-else>
          <a href="#" v-for="category of data.categories" :key="category.id" class="link-margin" @click="selectCategory(category.id)">
            {{ category.id }}. {{ category.name }}
          </a>
        </div>
      </template>
    </ApolloQuery>

    <!-- <ApolloQuery :query ="require('@/graphql/queries/Books.gql')">
      <template slot-scope="{ result: { data, loading } }">
        <div v-if="loading">Loading . . .</div>
        <div v-else>
          <div v-for="book of data.books" :key="book.id">
            {{ book.id }}. {{ book.title }}
          </div>
        </div>
      </template>
    </ApolloQuery> -->

    <ApolloQuery :query ="require('@/graphql/queries/Category.gql')" :variables="{ id: selectedCategory }">
      <template slot-scope="{ result: { data, loading } }">
        <div v-if="loading">Loading . . .</div>
        <div v-else>
          <div v-for="book of data.category.books" :key="book.id">
            {{ book.id }}. {{ book.title }}
          </div>
        </div>
      </template>
    </ApolloQuery>
  </div>
</template>

<script>
// @ is an alias to /src
import gql from 'graphql-tag'

export default {
  name: 'HomeView',
  components: {

  },
  data() {
    return {
      selectedCategory: 1,
      categories: []
    }
  },
  apollo: {
    categories: gql`{
      categories {
        id
        name
      }
    }`,
  },

  methods: {
    selectCategory(category) {
      this.selectedCategory = category
    }
  }
}
</script>

<style scoped>
  .link-margin{
    margin-right: 24px;
  }
</style>