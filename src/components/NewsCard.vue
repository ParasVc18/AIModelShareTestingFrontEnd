<template>
  <section class="p-4 rounded-lg shadow-lg bg-gray-50 w-80" @click="myFunction" :id="'parentDiv'+index">
    <div class="h-96" style="height:auto">
      <a
        class="text-xl font-bold text-center text-blue-800 hover:text-blue-600 hover:underline"
        :href="post.url"
        target="_blank"
        rel="noreferrer"
      >
        {{ post.title }}
      </a>
      <img
        class="w-full mt-2 rounded"
        :src="post.thumbnail"
        :alt="post.caption"
        height="140"
        width="210"
      />
    </div>
    <div :id="'myDIV'+index" style="display:none">
      <p class="mt-2 text-justify text-gray-700 line-clamp-4">
        {{ post.abstract }}
      </p>
    </div>
    <div>
      <p class="mt-4 font-bold text-gray-600">{{ post.byline }}</p>
      <p class="font-light text-gray-600">
        {{ formatDate(post.published_date) }}
      </p>
    </div>
  </section>
</template>

<script>
import { format } from "date-fns"

export default {
  props: {
    post: {
      type: Object,
      required: true,
    },
      index: {
        type: Number,
        required: true,
      }
  },
  methods: {
    formatDate(strDate) {
      return format(new Date(strDate), "MMMM do, yyyy")
    },
    myFunction(event) {
      var x = document.getElementById("myDIV1");
      for (let pathVar in event.path){
        if (event.path[pathVar].id){
          if(event.path[pathVar].id.includes("parentDiv")){
            let substringIndex = event.path[pathVar].id.indexOf("parentDiv")
            let elementIndex = event.path[pathVar].id.substring(9);
            x = document.getElementById("myDIV"+elementIndex);
          }
        }
      }
      if (x.style.display === "none") {
        x.style.display = "block";
      } else {
        x.style.display = "none";
      }
    },
  },
}
</script>
