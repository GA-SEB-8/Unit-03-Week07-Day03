<h1>
  <span class="headline">Fetching Data in React</span>
  <span class="subhead">Concepts</span>
</h1>

**Learning objective:** By the end of this lesson, students will be able to describe what AJAX is and its role in making HTTP requests in single-page applications.

## Retrieving data in React

Data can flow within a React app through `props`, but what happens when we want to retrieve data outside our application? In order to build a full-stack React app, we need a method for communicating with external resources, like a server. This is where [AJAX](https://developer.mozilla.org/en-US/docs/Glossary/AJAX) comes into play.

## What is AJAX?

The term **AJAX (Asynchronous JavaScript and XML)** refers to a collection of techniques for sending and receiving data within client-side web applications. These techniques allow client-side apps to communicate asynchronously with a server and update the DOM accordingly. Ultimately, this produces more interactive web applications.

Despite the inclusion of XML in its name, today, AJAX typically deals with the exchange of JSON (JavaScript Object Notation) data. It's important to note that this exchange of data is asynchronous, meaning data can be fetched without freezing the user interface or requiring a page reload. In modern web apps, this asynchronous exchange of data is often achieved through JavaScript's **Fetch API (Application Programming Interface)** or libraries such as Axios.

> 🧠 AJAX originally relied on the XMLHttpRequest object in JavaScript, but tools like the Fetch API or Axios have largely replaced this approach.

## The role of AJAX in single-page applications

Before single-page applications (SPAs), websites like Amazon used server-side rendered (SSR) applications, where each user interaction or change in data required loading a new page. While effective, this process was time-consuming for loading each product page separately.

SPAs, like Google Maps, revolutionized this by loading only small data pieces for user interactions and dynamically updating the DOM. Unlike its predecessor MapQuest, where moving the map required reloading pages, Google Maps uses JavaScript to handle user interactions such as dragging. It sends AJAX requests to the server, receives only the necessary data, and updates the map without reloading the entire page. This results in a smoother and faster user experience.

## AJAX in React

In this module, we will be using `fetch()` within our React apps to request data from external sources. Oftentimes, these external sources will be **Web APIs**.

In web development, a **Web API** refers to a web service that allows clients to communicate with a server. While traditional websites might respond to requests with HTML, Web APIs often respond to HTTP requests with something like JSON data. This allows the information to be consumed by other applications.

With AJAX techniques like `fetch()`, our React apps can make requests to an API and then update the DOM using the data sent as a response.

> 💡 With a React app, we can fetch the data from anywhere. Unlike server-side rendered applications, there is no inherent connection between the React applications we will write and the data sources we will work with.
