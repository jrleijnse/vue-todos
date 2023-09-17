<script></script>
<template>
  <!-- slot makes the content of the button dynamic whenever you render it inside another component or make it have a default value -->
  <button><slot>Create</slot></button>
</template>

<style lang="scss" scoped>
button {
  padding: 8px 16px;
  border: none;
}
</style>
