<template>
  <div class="home">
    <ApolloQuery :query ="categoriesQuery">
      <template slot-scope="{ result: { data } }">
        <div v-if="!data">Loading . . .</div>
        <div v-else>
          <a href="" class="link-margin" @click.prevent="selectCategory('all')">All</a>
          <a href="" class="link-margin" @click.prevent="selectCategory('featured')">Featured</a>
          <a href="#" v-for="category of data.categories" :key="category.id" class="link-margin" @click.prevent="selectCategory(category.id)">
            {{ category.id }}. {{ category.name }}
          </a>
        </div>
      </template>
    </ApolloQuery>

    <div v-if="selectedCategory == 'all'">
      <ApolloQuery :query ="query">
        <template slot-scope="{ result: { data } }">
          <div v-if="!data">Loading . . .</div>
          <div v-else>
            <div v-for="book of data.books" :key="book.id">
              {{ book.id }}. {{ book.title }}
            </div>
          </div>
        </template>
      </ApolloQuery>
    </div>

    <div v-else-if="selectedCategory == 'featured'">
      <ApolloQuery :query ="query" :variables="{ featured: true }">
        <template slot-scope="{ result: { data } }">
          <div v-if="!data">Loading . . .</div>
          <div v-else>
            <div v-for="book of data.featuredBooks" :key="book.id">
              {{ book.id }}. {{ book.title }}
            </div>
          </div>
        </template>
      </ApolloQuery>      
    </div>

    <div v-else>
      <ApolloQuery :query ="categoryQuery" :variables="{ id: selectedCategory }">
        <template slot-scope="{ result: { data } }">
          <div v-if="!data">Loading . . .</div>
          <div v-else>
            <div v-for="book of data.category.books" :key="book.id">
              {{ book.id }}. {{ book.title }}
            </div>
          </div>
        </template>
      </ApolloQuery>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import categoryQuery from '@/graphql/queries/Category.gql'
import categoriesQuery from '@/graphql/queries/Categories.gql'
import booksQuery from '@/graphql/queries/Books.gql'
import booksFeaturedQuery from '@/graphql/queries/BooksFeatured.gql'


export default {
  name: 'HomeView',
  components: {

  },
  data() {
    return {
      categoryQuery: categoryQuery,
      categoriesQuery: categoriesQuery,
      booksQuery: booksQuery,
      booksFeaturedQuery: booksFeaturedQuery,
      selectedCategory: 'all',
      query: booksQuery,
      categories: []
    }
  },
  methods: {
    selectCategory(category) {
      if (category === 'all') {
        this.query = booksQuery
      } else if (category === 'featured') {
        this.query = booksFeaturedQuery
      } else {
        this.query = categoryQuery
      }

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