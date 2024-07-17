<template>
  <div
    class="fixed inset-0 bg-black/80 z-50 flex items-center justify-center p-4 sm:p-5"
    @click.self="$emit('close')"
  >
    <div
      class="story-content w-full relative select-none max-w-[488px] h-[80dvh] aspect-[9/16] rounded-lg bg-black"
    >
      <!-- close button -->
      <button
        type="button"
        class="absolute w-8 h-8 bg-transparent focus:outline-none right-0 z-[10] text-white -top-8"
        @click.prevent="$emit('close')"
      >
        <svg
          xmlns="http://www.w3.org/2000/svg"
          fill="none"
          viewBox="0 0 24 24"
          stroke-width="1.5"
          stroke="currentColor"
          class="size-7"
        >
          <path
            stroke-linecap="round"
            stroke-linejoin="round"
            d="M6 18 18 6M6 6l12 12"
          />
        </svg>
      </button>
      <!-- navigation -->
      <nav
        class="navigation-story absolute z-[2] inset-x-4 top-4 h-1 md:h-1.5 flex gap-1 items-center"
      >
        <!-- loader -->
        <div
          v-for="(loader, index) in stories"
          :key="index"
          class="flex flex-auto h-full bg-black/20 overflow-hidden"
        >
          <div class="bg-white/80 h-full"></div>
        </div>
      </nav>
      <!-- items -->
      <div v-for="(story, index) in stories" :key="index" class="w-full h-full">
        <div
          v-if="current === index"
          class="absolute inset-0 overflow-hidden rounded-lg flex items-center justify-center"
        >
          <!-- video -->
          <video
            v-if="story.type === 'video'"
            :ref="`story-video-${index}`"
            class="w-full relative z-[1] pointer-events-none h-full aspect-[9/16] object-cover object-center"
            :src="story.src"
            autoplay
            playsinline
            webkit-playsinline
            :muted="muted"
            controls
          ></video>
          <!-- image -->
          <template v-else>
            <span
              class="absolute pointer-events-none overflow-hidden inset-0 flex items-center justify-center z-[0]"
            >
              <img
                :src="story.src"
                class="scale-150 blur-xl w-full h-full object-center object-cover"
                alt=""
              />
            </span>
            <img
              :src="story.src"
              alt=""
              decoding="async"
              class="max-w-full relative z-[1] max-h-full object-contain object-center"
              @load="handleImageLoad"
            />
          </template>
          <!-- presentation -->
          <div class="absolute grid grid-cols-[0.4fr,_1fr] inset-0 z-[3]">
            <div class="presentation-prev"></div>
            <div class="presentation-next"></div>
          </div>
          <!-- see more -->
          <div
            class="see-more absolute z-[4] pointer-events-none bottom-6 p-4 inset-x-0 flex items-center justify-center"
          >
            <a
              v-if="story.hasOwnProperty('seeMore') && !loading"
              :href="story.seeMore.link"
              :target="story.seeMore.target"
              class="inline-flex pointer-events-auto items-center gap-2 whitespace-nowrap rounded-lg border-0 bg-blue-500 px-6 py-3 text-sm md:text-base font-normal leading-6 text-white focus:outline-none"
            >
              {{ story.seeMore.title }}
            </a>
          </div>
        </div>
      </div>
      <!-- controls -->
      <div
        class="flex z-[3] items-center pointer-events-none justify-between absolute transform top-1/2 -translate-y-1/2 -inset-x-10"
      >
        <button
          type="button"
          class="w-6 h-6 inline-flex p-0 items-center justify-center bg-transparent pointer-events-auto text-white focus:outline-none border-none opacity-50 hover:opacity-100 disabled:opacity-20 disabled:hover:opacity-20"
          :disabled="current === 0"
          @click.prevent="onPrev()"
        >
          <svg
            xmlns="http://www.w3.org/2000/svg"
            viewBox="0 0 24 24"
            fill="currentColor"
            class="size-7 shrink-0"
          >
            <path
              fill-rule="evenodd"
              d="M12 2.25c-5.385 0-9.75 4.365-9.75 9.75s4.365 9.75 9.75 9.75 9.75-4.365 9.75-9.75S17.385 2.25 12 2.25Zm-4.28 9.22a.75.75 0 0 0 0 1.06l3 3a.75.75 0 1 0 1.06-1.06l-1.72-1.72h5.69a.75.75 0 0 0 0-1.5h-5.69l1.72-1.72a.75.75 0 0 0-1.06-1.06l-3 3Z"
              clip-rule="evenodd"
            />
          </svg>
        </button>
        <button
          type="button"
          class="w-6 h-6 inline-flex p-0 items-center justify-center bg-transparent pointer-events-auto text-white focus:outline-none border-none opacity-50 hover:opacity-100 disabled:opacity-20 disabled:hover:opacity-20"
          :disabled="current === stories.length - 1"
          @click.prevent="onNext()"
        >
          <svg
            xmlns="http://www.w3.org/2000/svg"
            viewBox="0 0 24 24"
            fill="currentColor"
            class="size-7 shrink-0"
          >
            <path
              fill-rule="evenodd"
              d="M12 2.25c-5.385 0-9.75 4.365-9.75 9.75s4.365 9.75 9.75 9.75 9.75-4.365 9.75-9.75S17.385 2.25 12 2.25Zm4.28 10.28a.75.75 0 0 0 0-1.06l-3-3a.75.75 0 1 0-1.06 1.06l1.72 1.72H8.25a.75.75 0 0 0 0 1.5h5.69l-1.72 1.72a.75.75 0 1 0 1.06 1.06l3-3Z"
              clip-rule="evenodd"
            />
          </svg>
        </button>
      </div>
      <div
        v-if="loading"
        class="absolute inset-0 flex items-center justify-center z-10 bg-black/20"
      >
        <span
          class="border-[6px] rounded-full border-white border-l-white/40 w-8 h-8 animate-spin"
        ></span>
      </div>
    </div>
  </div>
</template>

<script>
import anime from "animejs/lib/anime.es.js";
import Hammer from "hammerjs";
const TIMELINE_DURATION = 10000;
export default {
  name: "Stories",
  emits: ["close"],
  props: {
    stories: {
      type: Array,
      required: true,
    },
  },
  data() {
    return {
      current: 0,
      muted: true,
      loading: true,
      timeline: null,
      hammertime: null,
    };
  },
  watch: {
    currentImage(newImage) {
      this.checkIfImageCached(newImage);
    },
  },
  computed: {
    stream() {
      return this.$refs[`story-video-${this.current}`];
    },
    currentImage() {
      return this.stories[this.current].src;
    },
  },
  methods: {
    handleImageLoad() {
      this.loading = false;
    },
    checkIfImageCached(imageURL) {
      const image = new Image();
      image.src = imageURL;
      if (image.complete) {
        this.loading = false;
      } else {
        this.loading = true;
      }
    },
    onPrev() {
      if (this.current > 0) {
        this.current -= 1;
        this.timeline.pause();
        this.timeline.seek(this.current * TIMELINE_DURATION);
        this.timeline.play();
      }
    },
    onNext() {
      if (this.current <= this.stories.length - 1) {
        this.current += 1;
        this.timeline.pause();
        this.timeline.seek(this.current * TIMELINE_DURATION);
        this.timeline.play();
      }
      if (this.current === this.stories.length) {
        this.$emit("close");
      }
    },
  },
  created() {
    this.checkIfImageCached(this.currentImage);
  },
  mounted() {
    // remove scroll
    document.body.style.overflow = "hidden";

    const container = document.querySelector(".story-content");

    this.timeline = anime.timeline({
      autoplay: true,
      duration: TIMELINE_DURATION,
      easing: "linear",
      loop: true,
    });

    this.stories.forEach((story, index) => {
      this.timeline.add({
        targets: document.querySelectorAll(".navigation-story > div")[index]
          .children[0],
        width: "100%",
        changeBegin: () => {
          this.current = index;
          if (story.type === "video") {
            this.loading = false;
          }
        },
        complete: () => {
          if (index === this.stories.length - 1) {
            this.$emit("close");
          }
        },
      });
    });

    let hammertime = new Hammer(container);

    // detenemos la historia
    hammertime.on("press", () => {
      this.timeline.pause();
    });
    // continuamos con la historia
    hammertime.on("pressup", () => {
      this.timeline.play();
    });

    console.log(document.querySelectorAll("see-more"));

    hammertime.on("tap", (e) => {
      console.log(e.target);
      if (e.target.classList.contains("presentation-next")) {
        this.onNext();
      }
      if (e.target.classList.contains("presentation-prev")) {
        this.onPrev();
      }
    });
  },
  beforeDestroy() {
    // add scroll
    document.body.style.overflow = "auto";
  },
};
</script>
