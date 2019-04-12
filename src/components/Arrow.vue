<template>
  <svg xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" viewBox="0 0 24 24">
    <g :fill="color">
         <path id="b" d="M7.83 11l4.882-4.882a.994.994 0 0 0-.002-1.413 1.012 1.012 0 0 0-1.42.005l-6.58 6.58a1.01 1.01 0 0 0 0 1.42l6.58 6.58a.998.998 0 0 0 1.415.005 1 1 0 0 0-.002-1.413L7.83 13h11.174c.55 0 .996-.444.996-1 0-.552-.448-1-.996-1H7.83z"/>
    </g>
  </svg>
</template>

<script>
export default {
  name: 'Arrow',
  props: {
    color: { type: String }
  }
}
</script>
