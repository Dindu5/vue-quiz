<template>
  <div class="tabs is-toggle is-fullwidth">
    <ul>
      <li class="is-active">
        <a>
          <span>Quiz App : 20</span>
        </a>
      </li>
      <li class="has-background-success">
        <a>
          <span class="icon is-small"
            ><i class="fas fa-check" aria-hidden="true"></i
          ></span>
          <span>{{ correctAnswers }}</span>
        </a>
      </li>
      <li>
        <a>
          <span class="icon is-small"
            ><i class="far fa-file-alt" aria-hidden="true"></i
          ></span>
          <span>{{ submitted }}</span>
        </a>
      </li>
    </ul>
  </div>
  <hr />
</template>
<script>
export default {
  name: "Header",
  props: ["submitted", "correctAnswers"],
};
</script>
<style lang="css">
</style>