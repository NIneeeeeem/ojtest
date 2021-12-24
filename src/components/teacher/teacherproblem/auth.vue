<template>

</template>

<script>
import Cookies from 'js-cookie'
const TokenKey = 'Admin-Token'
export default {
  name: "auth"
}


export function getToken() {
  return Cookies.get(TokenKey)
}
</script>

<style scoped>

</style>
