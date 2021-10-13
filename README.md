# StepZen+Nuxt

This sample repo uses the [Nuxt Mountains API](https://api.nuxtjs.dev/mountains) and StepZen's [`@rest` directive](https://stepzen.com/docs/connecting-backends/how-to-connect-a-rest-service) to fetch a list of mountains and expose a GraphQL API to our Nuxt frontend.

## Outline

* [Project Setup](#1-project-setup)
* [Deploy API](#2-deploy-api)
  * [GraphQL API](#21-graphql-api)
* [Mountains Component - `NuxtMountains.vue`](#3-mountains-component---nuxtmountainsvue)
* [Nuxt App - `index.vue`](#4-nuxt-app---indexvue)
* [Configuration - `nuxt.config.js`](#5-configuration---nuxtconfigjs)

## 1. Project Setup

Before you get started, you'll need to get yourself a [StepZen account](https://stepzen.com/request-invite) and [install the StepZen CLI](https://stepzen.com/docs/quick-start).

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

## 2. Deploy API

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

This also deployed our API to `https://${username}.stepzen.net/stepzen-nuxt-tutorial/users/__graphql` where `${username}` is your [StepZen account name](https://stepzen.com/account).

**Note:** Make sure to run `cp .env.example .env` and fill in the environment details using the output from the "Deploy API" step (`stepzen start`) and your [StepZen account page](https://stepzen.com/account) as a reference.

Your `.env` file should look similar to:

```sh
REACT_APP_STEPZEN_API_KEY=foousername+9001::1e9e17d05711e0f09271db4888b66e364df638fe0c77ea33984599c5b87f9427
REACT_APP_STEPZEN_ENDPOINT=https://foousername.stepzen.net/stepzen-nuxt-tutorial/users/__graphql
```

### 2.1 GraphQL API

For more information around the GraphQL API, please look at the [`mountains.graphql`](stepzen/schema/mountains.graphql) file in our `schema` directory for our `Mountain` interface and `Query` type.


## 3. Mountains Component - `NuxtMountains.vue`

We have a directory for `components` with a [`NuxtMountains.vue`](./components/NuxtMountains.vue) file. This contains our query to fetch the mountain data from the [NuxtJS Mountain API](https://api.nuxtjs.dev/mountains).

The endpoint used in `fetch()` is the API call that needs to be made [in a serverless function](https://github.com/stepzen-samples/stepzen-nuxt/issues/2) to protect your StepZen keys.

## 4. Nuxt App - `index.vue`

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

## 5. Configuration - `nuxt.config.js`

Adding `true` for `components` lets you use any components inside the `components` directory without needing to import the components.

```javascript
export default {
  components: true
}
```
