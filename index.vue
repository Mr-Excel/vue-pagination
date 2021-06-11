<template>
  <div class="min-w-max">
    <section
      style="height:32px"
      class="flex justify-between bg-white  px-10 py-6 text-gray-700 font-montserrat"
    >
      <div v-if="showFilter" class="flex items-center">
        <div class="pr-2 text-gray-400 font-medium">
          <span id="text-before-input">
            {{ textBeforeInput }}
          </span>
        </div>
        <div class="w-14 rounded-md border border-indigo-400 px-1 py-1">
          <input
            v-model.number="input"
            class="w-12 focus:outline-none px-2"
            type="text"
          />
        </div>
        <div
          @click.prevent="changePage(input)"
          class="flex items-center pl-4 font-medium cursor-pointer"
        >
          <span id="text-after-input">
            {{ textAfterInput }}
          </span>
          <svg
            class="h-4 w-4"
            xmlns="http://www.w3.org/2000/svg"
            fill="none"
            viewBox="0 0 24 24"
            stroke="currentColor"
          >
            <path
              stroke-linecap="round"
              stroke-linejoin="round"
              stroke-width="2"
              d="M9 5l7 7-7 7"
            />
          </svg>
        </div>
      </div>
      <ul class="flex items-center __pagination_li">
        <li class="pr-6">
          <a href="#" @click.prevent="changePage(1)">
            <div
              class="flex items-center justify-center hover:bg-gray-200 rounded-full transform rotate-45 h-6 w-6"
            >
              <div class="transform -rotate-45">
                First
              </div>
            </div>
          </a>
        </li>
        <li class="pr-6" v-if="hasPrev()">
          <a href="#" @click.prevent="changePage(prevPage)">
            <div
              class="flex items-center justify-center hover:bg-gray-200 rounded-full transform rotate-45 h-6 w-6"
            >
              <div class="transform -rotate-45">
                <svg
                  class="h-4 w-4"
                  xmlns="http://www.w3.org/2000/svg"
                  fill="none"
                  viewBox="0 0 24 24"
                  stroke="currentColor"
                >
                  <path
                    stroke-linecap="round"
                    stroke-linejoin="round"
                    stroke-width="2"
                    d="M15 19l-7-7 7-7"
                  />
                </svg>
              </div>
            </div>
          </a>
        </li>
        <li class="pr-6" v-if="hasFirst()">
          <a href="#" @click.prevent="changePage(1)">
            <div
              class="flex hover:bg-gray-200 rounded-full transform rotate-45 h-6 w-6 items-center justify-center"
            >
              <span class="transform -rotate-45">
                1
              </span>
            </div>
          </a>
        </li>
        <li class="pr-6" v-if="hasFirst()">...</li>
        <li class="pr-6" v-for="page in pages" :key="page">
          <a href="#" @click.prevent="changePage(page)">
            <div
              :class="{
                'bg-gradient-to-r from-blue-400 to-indigo-400': current == page,
              }"
              class="flex hover:bg-gray-200 rounded-full transform rotate-45 h-6 w-6 items-center justify-center"
            >
              <span class="transform -rotate-45">{{ page }}</span>
            </div>
          </a>
        </li>
        <li class="pr-6" v-if="hasLast()">...</li>
        <li class="pr-6" v-if="hasLast()">
          <a href="#" @click.prevent="changePage(totalPages)">
            <div
              class="flex hover:bg-gray-200 rounded-full transform rotate-45 h-6 w-6 items-center justify-center"
            >
              <span class="transform -rotate-45">
                {{ totalPages }}
              </span>
            </div>
          </a>
        </li>
        <li class="pr-6" v-if="hasNext()">
          <a href="#" @click.prevent="changePage(nextPage)">
            <div
              class="flex items-center justify-center hover:bg-gray-200 rounded-full transform rotate-45 h-6 w-6"
            >
              <div class="transform -rotate-45">
                <svg
                  class="h-4 w-4"
                  xmlns="http://www.w3.org/2000/svg"
                  fill="none"
                  viewBox="0 0 24 24"
                  stroke="currentColor"
                >
                  <path
                    stroke-linecap="round"
                    stroke-linejoin="round"
                    stroke-width="2"
                    d="M9 5l7 7-7 7"
                  />
                </svg>
              </div>
            </div>
          </a>
        </li>
        <li class="pr-6">
          <a href="#" @click.prevent="changePage(lastPage)">
            <div
              class="flex items-center justify-center hover:bg-gray-200 rounded-full transform rotate-45 h-6 w-6"
            >
              <div class="transform -rotate-45">
                Last
              </div>
            </div>
          </a>
        </li>
      </ul>
    </section>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
export default defineComponent({
  name: "customPagination",
  props: {
    showFilter: {
      type: Boolean,
      default: false,
    },
    current: {
      type: Number,
      default: 1,
    },
    total: {
      type: Number,
      default: 0,
    },
    perPage: {
      type: Number,
      default: 9,
    },
    pageRange: {
      type: Number,
      default: 2,
    },
    textBeforeInput: {
      type: String,
      default: "Go to page",
    },
    textAfterInput: {
      type: String,
      default: "Go",
    },
  },
  data() {
    return {
      input: "",
    };
  },
  methods: {
    hasFirst: function() {
      return this.rangeStart !== 1;
    },
    hasLast: function() {
      return this.rangeEnd < this.totalPages;
    },
    hasPrev: function() {
      return this.current > 1;
    },
    hasNext: function() {
      return this.current < this.totalPages;
    },
    changePage: function(page) {
      if (page > 0 && page <= this.totalPages) {
        this.$emit("page-changed", page);
      }
    },
  },
  computed: {
    pages: function() {
      var pages = [];
      for (var i = this.rangeStart; i <= this.rangeEnd; i++) {
        pages.push(i);
      }
      return pages;
    },
    rangeStart: function() {
      var start = this.current - this.pageRange;
      return start > 0 ? start : 1;
    },
    rangeEnd: function() {
      var end = this.current + this.pageRange;
      return end < this.totalPages ? end : this.totalPages;
    },
    totalPages: function() {
      return Math.ceil(this.total / this.perPage);
    },
    lastPage: function() {
      console.log(Math.ceil(this.total / this.perPage));
      return Math.ceil(this.total / this.perPage);
    },
    nextPage: function() {
      return this.current + 1;
    },
    prevPage: function() {
      return this.current - 1;
    },
    firstPage: function() {
      return 1;
    },
  },
});
</script>

<style scoped>
* {
  --tw-shadow: 0 0 #0000;
}
* {
  --tw-ring-inset: var(--tw-empty); /*!*/ /*!*/
  --tw-ring-offset-width: 0px;
  --tw-ring-offset-color: #fff;
  --tw-ring-color: rgba(59, 130, 246, 0.5);
  --tw-ring-offset-shadow: 0 0 #0000;
  --tw-ring-shadow: 0 0 #0000;
}
.text-gray-400 {
  --tw-text-opacity: 1;
  color: rgba(156, 163, 175, var(--tw-text-opacity));
}
.min-w-max {
  min-width: -webkit-max-content;
  min-width: -moz-max-content;
  min-width: max-content;
}
.flex {
  display: flex;
}
.bg-white {
  --tw-bg-opacity: 1;
  background-color: rgba(255, 255, 255, var(--tw-bg-opacity));
}

.rounded-lg {
  border-radius: 0.5rem;
}
.border {
  border-width: 1px;
}
.border-gray-200 {
  --tw-border-opacity: 1;
  border-color: rgba(229, 231, 235, var(--tw-border-opacity));
}
.px-10 {
  padding-left: 2.5rem;
  padding-right: 2.5rem;
}
.py-3 {
  padding-top: 0.75rem;
  padding-bottom: 0.75rem;
}
.text-gray-700 {
  --tw-text-opacity: 1;
  color: rgba(55, 65, 81, var(--tw-text-opacity));
}
.font-montserrat {
  font-family: Montserrat;
}
.items-center {
  align-items: center;
}
.pr-6 {
  padding-right: 1.5rem;
}
.justify-center {
  justify-content: center;
}
.w-6 {
  width: 32px;
}
.h-6 {
  height: 32px;
}
.rotate-45 {
  --tw-rotate: 45deg;
}
.-rotate-45 {
  --tw-rotate: -45deg;
}
.transform {
  --tw-translate-x: 0;
  --tw-translate-y: 0;
  --tw-rotate: 0;
  --tw-skew-x: 0;
  --tw-skew-y: 0;
  --tw-scale-x: 1;
  --tw-scale-y: 1;
  transform: translateX(var(--tw-translate-x)) translateY(var(--tw-translate-y))
    rotate(var(--tw-rotate)) skewX(var(--tw-skew-x)) skewY(var(--tw-skew-y))
    scaleX(var(--tw-scale-x)) scaleY(var(--tw-scale-y));
}
.rounded-md {
  border-radius: 0.375rem;
}
.rounded-full {
  border-radius: 50%;
}
.hover\:bg-gray-200:hover {
  width: 32px;
  height: 32px;
  --tw-bg-opacity: 1;
  background-color: rgba(229, 231, 235, var(--tw-bg-opacity));
}
.to-indigo-400 {
  --tw-gradient-to: #e8f7f8;
}
.from-blue-400 {
  width: 32px;
  height: 32px;
  border: 1px solid #1c4b5a;
  --tw-gradient-from: #e8f7f8;
}
.__pagination_li{
list-style:none;
}
.bg-gradient-to-r {
  background-image: linear-gradient(to right, var(--tw-gradient-stops));
}
.font-medium {
  font-weight: 500;
}
.pr-2 {
  padding-right: 0.5rem;
}
.py-1 {
  padding-top: 0.25rem;
  padding-bottom: 0.25rem;
}
.px-1 {
  padding-left: 0.25rem;
  padding-right: 0.25rem;
}
.border-indigo-400 {
  --tw-border-opacity: 1;
  border-color: rgba(129, 140, 248, var(--tw-border-opacity));
}
.w-14 {
  width: 3.5rem;
}
.focus\:outline-none:focus {
  outline: 2px solid transparent;
  outline-offset: 2px;
}
.px-2 {
  padding-left: 0.5rem;
  padding-right: 0.5rem;
}
.cursor-pointer {
  cursor: pointer;
}
.pl-4 {
  padding-left: 1rem;
}

.h-4 {
  height: 1rem;
}
.w-4 {
  width: 1rem;
}

/*dasdsadsa*/
/* 
.bg-blue-50 {
  --tw-bg-opacity: 1;
  background-color: rgba(239, 246, 255, var(--tw-bg-opacity));
}

.table {
  display: table;
}

.justify-between {
  justify-content: space-between;
}

.min-h-screen {
  min-height: 100vh;
}

.p-5 {
  padding: 1.25rem;
}

.w-12 {
  width: 3rem;
} */

@-webkit-keyframes spin {
  to {
    transform: rotate(360deg);
  }
}
@keyframes spin {
  to {
    transform: rotate(360deg);
  }
}
@-webkit-keyframes ping {
  100%,
  75% {
    transform: scale(2);
    opacity: 0;
  }
}
@keyframes ping {
  100%,
  75% {
    transform: scale(2);
    opacity: 0;
  }
}
@-webkit-keyframes pulse {
  50% {
    opacity: 0.5;
  }
}
@keyframes pulse {
  50% {
    opacity: 0.5;
  }
}
@-webkit-keyframes bounce {
  0%,
  100% {
    transform: translateY(-25%);
    -webkit-animation-timing-function: cubic-bezier(0.8, 0, 1, 1);
    animation-timing-function: cubic-bezier(0.8, 0, 1, 1);
  }
  50% {
    transform: none;
    -webkit-animation-timing-function: cubic-bezier(0, 0, 0.2, 1);
    animation-timing-function: cubic-bezier(0, 0, 0.2, 1);
  }
}
@keyframes bounce {
  0%,
  100% {
    transform: translateY(-25%);
    -webkit-animation-timing-function: cubic-bezier(0.8, 0, 1, 1);
    animation-timing-function: cubic-bezier(0.8, 0, 1, 1);
  }
  50% {
    transform: none;
    -webkit-animation-timing-function: cubic-bezier(0, 0, 0.2, 1);
    animation-timing-function: cubic-bezier(0, 0, 0.2, 1);
  }
}
</style>
