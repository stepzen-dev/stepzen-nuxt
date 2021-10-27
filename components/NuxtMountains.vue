<template>
  <p v-if="$fetchState.pending">
    Fetching...
  </p>

  <div v-else>
    <h2>Mountains</h2>
    <ul>
      <li
        v-for="mountain of mountains.data.mountains"
        v-bind:key="mountain.items"
      >
        <nuxt-link :to="{ name: 'mountains-id', params: { id: mountain.title } }">{{ mountain.title }}</nuxt-link>
      </li>
    </ul>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        mountains: []
      }
    },
    async fetch() {
      this.mountains = await fetch(
        `${process.env.STEPZEN_ENDPOINT}`,
        {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json',
            'Authorization': `apikey ${process.env.STEPZEN_API_KEY}`
          },
          body: JSON.stringify(
            { query: '{ mountains { title } }' }
          )
        }
      )
      .then(res => res.json())
      
      console.log(this.mountains)
      console.log(this.mountains.data.mountains)
    }
  }
</script>