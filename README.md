## 1. Project Setup

Clone repo and install dependencies.

```bash
git clone https://github.com/stepzen-samples/stepzen-nuxt.git
cd stepzen-nuxt
npm i
```

Start the development server on `localhost:3000`.

```bash
npm run dev
```

You will initially see an error, `Cannot read property 'mountains' of undefined`, because we haven't included our StepZen endpoint yet. See the next section for instructions to include your endpoint.

## 2. GraphQL API

`mountains.graphql` file in our `schema` directory for our `Mountain` interface and `Query` type

```graphql
# stepzen/schema/mountains.graphql

type Mountain {
  title: String!
}

type Query {
  mountains: [Mountain]
    @rest(
      endpoint:"https://api.nuxtjs.dev/mountains"
    )
}
```

`index.graphql` file for our `schema`

```graphql
# stepzen/index.graphql

schema
  @sdl(
    files: [
      "schema/mountains.graphql"
    ]
  ) {
  query: Query
}
```

### Deploy API

Deploy your API with `stepzen start`.

```bash
stepzen start
```

Send the following test query to verify your endpoint is working.

```graphql
query getMountains {
  mountains {
    title
  }
}
```

This also deployed our API to `https://username.stepzen.net/stepzen-nuxt-tutorial/users/__graphql`.

## 3. `NuxtMountains` component

We have a directory for `components` with a `NuxtMountains.vue` file. This contains our query to fetch the mountain data from the [NuxtJS Mountain API](https://api.nuxtjs.dev/mountains).

```vue
// components/NuxtMountains.vue

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
        {{ mountain.title }}
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
        '',
        {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json',
            'Authorization': 'apikey '
          },
          body: JSON.stringify(
            { query: '{ mountains { title } }' }
          )
        }
      )
      .then(res => res.json())
    }
  }
</script>
```

Fill in your username for `https://username.stepzen.net/stepzen-nuxt-tutorial/users/__graphql` and set the URL in `fetch()`. This is the API call that needs to be made [in a serverless function](https://github.com/stepzen-samples/stepzen-nuxt/issues/2) to protect your StepZen keys.

## 4. Nuxt App

Our Nuxt frontend contains an `index.vue` file for our home page inside a directory for `pages`. Our root component renders a title to the home page.

```vue
// pages/index.vue

<template>
  <div>
    <h1>StepZen Nuxt Tutorial</h1>
    <NuxtMountains />
  </div>
</template>
```

![Alt Text](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/lglwi3luxk2xgn9e4527.png)

## 5. `nuxt.config.js`

Adding `true` for `components` lets you use any components inside the `components` directory without needing to import the components.

```javascript
export default {
  components: true
}
```
