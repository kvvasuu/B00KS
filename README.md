# B00KS

![App demo]([https://raw.githubusercontent.com/kvvasuu/pokemon-search/master/demo.png](https://raw.githubusercontent.com/kvvasuu/B00KS/main/demo.png) "App demo")

## General info

"B00KS" is your window to the world of the latest literary bestsellers, straight from The New York Times. Our app, integrated with the NYT API, updates daily with the top books across various categories. Dive into the world of literature, discover new titles, and stay up-to-date with what everyone is reading.

It provides information about books of The New York Times Best Sellers lists.

In the application, we select the list to display. Later we can sort the results by rank or weeks on list.

Each book is displayed in a component that contains a photo, title, author, description and links to online stores. By clicking on the photo we can enlarge it.

Due to the API request limit of 5 per minute, the application has a cooldown timer 30 seconds that is displayed when sending a request after exceeding the limit.
You can check this, for example, by changing the list several times in a row or refreshing the page several times.

## Technologies:

- Vue
- New York Times API

## Setup

```
# install dependencies
npm install

# serve with hot reload at localhost:8080
npm run dev

# build for production with minification
npm run build

# build for production and view the bundle analyzer report
npm run build --report

# run unit tests
npm run unit

# run all tests
npm test
```
