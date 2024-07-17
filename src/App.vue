<template>
  <div id="app">
    <main>
      <div class="flex h-dvh w-full items-center justify-center gap-3">
        <div v-for="story in stories" :key="story.id">
          <div class="cursor-pointer" @click="openStory(story.id)">
            <img
              :src="story.thumbs"
              alt=""
              class="w-20 h-20 rounded-full object-cover object-center"
            />
          </div>
          <Stories
            v-if="open === story.id"
            :stories="story.substories"
            @close="open = null"
          />
        </div>
      </div>
    </main>
  </div>
</template>

<script>
import Stories from "./components/Stories.vue";
export default {
  name: "App",
  components: {
    Stories,
  },
  data() {
    return {
      open: null,
      stories: [
        {
          id: 1,
          thumbs:
            "https://plus.unsplash.com/premium_photo-1719596873422-20828ded1656?q=80&w=2646&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D",
          substories: [
            {
              src: "https://images.unsplash.com/photo-1535930891776-0c2dfb7fda1a?q=80&w=2574&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D",
              type: "image",
              seeMore: {
                title: "COMPRAR AHORA",
                link: "https://www.google.com",
                target: "_blank",
              },
            },
            {
              src: "https://images.unsplash.com/photo-1721062754942-8453d9c66f97?q=80&w=2671&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D",
              type: "image",
            },
            {
              src: "https://images.unsplash.com/photo-1560440021-33f9b867899d?q=80&w=2518&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D",
              type: "image",
            },
            {
              src: "https://images.unsplash.com/photo-1484154218962-a197022b5858?q=80&w=2674&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D",
              type: "image",
            },
            {
              src: "https://cdn.pixabay.com/video/2021/04/10/70549-535363124_large.mp4",
              type: "video",
            },
          ],
        },
        {
          id: 2,
          thumbs:
            "https://images.unsplash.com/photo-1502672260266-1c1ef2d93688?q=80&w=2580&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D",
          substories: [
            {
              src: "https://plus.unsplash.com/premium_photo-1674676471104-3c4017645e6f?q=80&w=2540&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D",
              type: "image",
            },
          ],
        },
      ],
    };
  },
  methods: {
    openStory(id) {
      this.open = id;
    },
  },
};
</script>
