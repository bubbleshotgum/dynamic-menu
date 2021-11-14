<template>
  <smart-menu 
    :lang="lang"
    @change-lang="tryChangeLanguage"
    @rotate-elements="rotate"/>
  <fixed-content />
</template>

<script>
import SmartMenu from "@/components/SmartMenu"
import FixedContent from "@/components/FixedContent"
export default {
  name: 'App',
  components: {SmartMenu, FixedContent},
  data: () => ({
    lang: 'en'
  }),
  methods: {
    tryChangeLanguage(lang)
    {
      if(this.lang !== lang)
        this.lang = lang
    }

  }
}
</script>

<style>
* {
  padding: 0;
  margin: 0;
}
#app {
  height: 200vh;
}
</style>
