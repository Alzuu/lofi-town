# 🎵 lofi-town
![GitHub](https://img.shields.io/github/license/Alzuu/lofi-town?style=for-the-badge)
![Made](https://img.shields.io/badge/this-Made%20with%20%F0%9F%92%99%20by%20Allan%20Li-yellow?style=for-the-badge)

Hello reader! 👋 If you're wondering what this repo is about, it is a personal development project of a lofi player application. The purpose of this repo is to practice development with Vue 3 Composition API, as well as create something that is personally meaningful and fun to me.

## Installation
Make sure you have [Node.js](https://nodejs.org/en/) and installed, any newer version should work fine (I use v18.12.0).

1. Clone this repository:

```bash
git clone https://github.com/Alzuu/lofi-town.git
```

2. Go to repository and install dependencies:
```bash
cd lofi-town
npm install
```

3. Create a dotenv file for GIPHY API key
```bash
touch .env
```

4. Open the dotenv file and add the GIPHY API key as `VITE_GIPHY_KEY={your_key}`
After that you've finished the installation!

## Running the project
1. Compile and hot-reload for development

```bash
npm run dev
```

2. Compile and minify for production

```bash
npm run build
```

3. Run the build
```bash
npm run preview
```

## Shoutout
This app is inspired by the amazing work done at [lofi.cafe](https://lofi.cafe), and also fetches GIFs for backgrounds from the [GIPHY API](https://developers.giphy.com).
