<!--Didnt work-->
<template>
  <span class="Tims">{{ entry[0] }} Uhr , {{ entry[1].replaceAll("/", ".") }}</span><br>
        <h3 class="Task">{{ entry[2] }}</h3>
        <span class="last">{{ entry[3] }}</span>
</template>

<script>
export default {
  props: ['entry'],
  data() {
  }
}
</script>