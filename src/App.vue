<template>
  <div id="app" v-cloak>
    <headerc  v-if="!$route.meta.hidenav"/>
    <router-view/>
    <footerc v-if="!$route.meta.hidenav" />
  </div>
</template>

<script>
import headerc from "./components/common/headerc";
import footerc from "./components/common/footerc";

export default {
  name: 'App',
  components: {
      headerc,
      footerc
  }
}
</script>

<style>
[v-cloak] {
    display: none;
}

</style>
