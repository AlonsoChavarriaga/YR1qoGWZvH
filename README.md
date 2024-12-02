# Akkio Code Challenge

This project is a solution to the Akkio Color Swatch challenge

## Project Setup

Install dependencies:

```sh
npm install
```

Then run the project locally:

```sh
npm run dev
```

## Summary of Design Decisions

I chose to use Vue because it has been a few years since I last worked with it and wanted to brush up on it. To improve the user experience, I used `debounce` from the `moderndash` library, which is a typescript-first, modern version of lodash. I opted for sliders instead of inputs because on a fixed range from 0-100 it seems more intuitive, which combined with the debounced API call, adds a smooth user experience compared to something like number inputs and a "Submit" button. For caching, I implemented a local object that uses `{saturation}-{lightness}` as a key, and prevent API calls if there is a matching key in the cache object.

For further optimization/performance, I would replace the cache object with localStorage or IndexedDB so the data persists, and if there were many more items to render, I would opt for virtualization using something like `vue-virtual-scroll-grid`.

## Bonus Question

To hypothetically reduce the number of API calls we make, we can use a binary search to do an initial call at the start of a hue, skip ahead (say, 20 values for example), and do another call. If the color name has changed, we cut it in half, do a search again using the middle value, and continue until we find the start and end for each named color.
