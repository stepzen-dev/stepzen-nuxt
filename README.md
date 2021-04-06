## 1. Project Setup

Create a project directory and a `package.json`.

```bash
mkdir stepzen-nuxt && cd stepzen-nuxt && npm init -y
```

Add the following scripts to your `package.json`.

```json
"scripts": {
  "dev": "nuxt",
  "build": "nuxt build",
  "generate": "nuxt generate",
  "start": "nuxt start"
},
```

Install the necessary dependencies

```bash
yarn add nuxt
```

## 2. Setup Nuxt App

To setup our Nuxt frontend create the following:

1. Directory for `pages`
2. An `index.vue` file for our home page

Our root component renders a title to the home page.

```html
// pages/index.vue

<template>
  <div>
    <h1>StepZen Nuxt Tutorial</h1>
  </div>
</template>
```

Start the development server on `localhost:3000` with the following command:

```bash
yarn dev
```

## 3. Setup GraphQL API

To setup our StepZen API create the following:

1. A `stepzen` directory containing a `schema` directory
2. A `mountains.graphql` file for our `Mountain` interface and `Query` type
3. An `index.graphql` file for our `schema`

```graphql
# stepzen/schema/mountains.graphql

interface Mountain {
  title: String!
}

type Query {
  mountains: [Mountain]
}

type MountainBackend implements Mountain {}

type Query {
  mountainsBackend: [MountainBackend]
    @supplies(
      query:"mountains"
    )
    @rest(
      endpoint:"https://api.nuxtjs.dev/mountains"
    )
}
```

```graphql
# stepzen/schema/index.graphql

schema
  @sdl(
    files: [
      "mountains.graphql"
    ]
  ) {
  query: Query
}
```

### Deploy API

```bash
stepzen start
```

![Alt Text](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/fq8g467eb8h8xl5kpv4v.png)

This also deployed our API to `https://username.stepzen.net/stepzen-nuxt-tutorial/users/__graphql`.


### Create `NuxtMountains` component

To fetch the mountain data from the [NuxtJS Mountain API](https://api.nuxtjs.dev/mountains) create the following:

1. Directory for `components`
2. A `NuxtMountains.vue` file for our query

Fill in your username and set the URL in `fetch()`

```html
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

Create a file in your root directory called `nuxt.config.js`.

```bash
touch nuxt.config.js
```

```javascript
export default {
    components: true
}
```

Now you can use any components inside the `components` directory without needing to import the components.

```html
<template>
  <div>
    <h1>Hello world!</h1>
    <NuxtMountains />
  </div>
</template>
```

![Alt Text](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/lglwi3luxk2xgn9e4527.png)
