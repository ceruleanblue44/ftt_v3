<template>
      <div class="pagination-section">
      <div class="prev" @click="prevPage()"> Previous </div>
      <div class="number" v-for="(num, index) in numPages()"
                          :key="index"
                          @click="gotoPage(num)"
                          :class="[num == currentPage ? 'active' : '' ]"
                          > {{ num }}

      </div>
      <div class="next" @click="nextPage()"> Next </div>
    </div>
</template>

<script>
export default {
  props: {
    currentPage: Number,
    numPages: Function,   
    gotoPage: Function,
    prevPage: Function,
    nextPage: Function,
    styleObj: Object,
  }
}
</script>
