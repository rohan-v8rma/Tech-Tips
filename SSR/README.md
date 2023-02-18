# Server Side Rendering 

Server-side rendering (SSR) is the process of rendering a React application on the server and sending the fully-rendered HTML to the client. This can provide a number of benefits, such as:

## 1. Improved SEO

Search engines can easily crawl and index the fully-rendered HTML, making it easier for users to find your website.

## 2. Faster initial load time

With SSR, the initial HTML is already loaded on the page, so the browser can start rendering the page immediately, which can make the initial load time faster.

## 3. Better user experience for slow internet connection

By sending a fully-rendered page, the user can start interacting with the page even before the JavaScript has loaded, improving the user experience for users on slow internet connections.

---

## Setting up SSR

There are a few libraries that can help you to set up SSR for your React application. 

One of the most popular libraries is `Next.js`. 

`Next.js` is a framework that allows you to easily set up SSR for your React application. It provides a simple way to organize your code and handle routing, while also providing a development server that automatically handles SSR for you. 

You can also use other popular libraries like `Razzle` or `after.js`.

Setting up SSR can be a bit more complex than setting up a traditional client-side React application, as you'll need to handle rendering on the server as well as on the client. 

However, with the help of these libraries, it can be much easier to set up SSR for your application.

When it comes to implementation, the first thing to do is to run the React application on the server and render the initial HTML on the server-side. This initial HTML can be returned to the browser and rendered. After the page has been loaded, React takes over and renders the page client-side. 

This process of rendering on the server and client side is called ***"isomorphic rendering"*** or ***"universal rendering"***.

It's worth noting that, if your application does not need the benefits mentioned above, you may not need SSR, then you can just render your application on client side.

---

## Downsides of SSR

Every request leads to a new page being re-rendered from the server to the browser. 

This means all the scripts, styles, and templates will be reloaded on the browser each time a request is sent to the server, resulting in a poor user experience.

## Overcoming the downsides of SSR

There are a few different ways to prevent every request from re-downloading resources such as stylesheets and templates of an SSR application:

### 1. Use caching

One of the simplest ways to prevent resources from being re-downloaded is to use caching. By caching resources such as stylesheets and templates, the server can return the cached version of the resource instead of re-downloading it on every request. This can be done by adding appropriate headers to the response, such as Cache-Control or Expires headers. Some server-side frameworks or http servers have built-in caching features.

### 2. Use a Content Delivery Network (CDN)

Another way to prevent resources from being re-downloaded is to use a Content Delivery Network (CDN). A CDN is a network of servers that are distributed around the world, and it's used to deliver content to users based on their geographic location. By using a CDN, resources such as stylesheets and templates can be cached at the edge of the network, so they are delivered to users faster and with less load on the origin server.

### 3. Use long term caching

A modern way to prevent resources from being re-downloaded is to use long-term caching. This is a technique in which assets are given a unique name or hash based on its contents, so when the contents of the file are updated, the name or the hash would change. This way, the browser would detect that the file is different and download it again, but if the file stays the same, the browser would use the cached version and prevent the request.

### 4. Use the browser storage

Another way to prevent resources from being redownloaded on each request is to use the browser storage, such as `localStorage` or `indexedDB` storage. 

By storing the resources in the browser storage after the first request, it prevents the resources from being re-downloaded on each request.

---

### Some caveats with these optimizations

It's worth noting that some of the aforementioned methods may not work with all types of resources, and different methods may work better depending on the specific use case. 

It's also important to consider:
- How caching and long-term caching will interact with deployment of new versions of the app and; 
- To have a way to expire the caches on a controlled basis.