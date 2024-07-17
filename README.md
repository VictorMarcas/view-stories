# View Stories

Project built to visualize artists' stories. Built in Vue2, TailwindCSS, Hammerjs and AnimateJs.

## Project Setup

```sh
npm install
```

### Compile and Hot-Reload for Development

```sh
npm run dev
```

### Compile and Minify for Production

```sh
npm run build
```

### Lint with [ESLint](https://eslint.org/)

```sh
npm run lint
```

## Configuration

```js
// components/Story.vue
const TIMELINE_DURATION = 10000; // 10s

// Stories options
const stories = [
    {
        ...,
        thumbs: 'URL image', // trigger to show stories
        substories: [
            {
                src: 'URL image' // if video, use 'URL video' (mp4)
                type: 'image or video',
                seeMore: {
                    title: 'Title',
                    description: 'Description',
                    url: 'URL'
                } // optional
            }
        ]
    }
]
```
