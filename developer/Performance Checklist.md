# Performance Checklist
---

## 1. Use In-Built Performance Quick Wins

Laravel offers a few in-built performance quick wins that can apply to any app.

The most significant performance quick win is route caching. Did you know that every time you boot your Laravel app, your app determines the middleware, resolves aliases, resolves route groups, and identifies the controller action and parameter inputs for every single route entry?

You can bypass the route processing by caching all the required routing information using the route:cache Artisan command:

```php
php artisan route:cache
php artisan config:cache
php artisan event:cache
```
> `Tip:` You should make sure to add the above caching commands to your deployment script so that every time you deploy, your routes, config, views, and events are re-cached. Otherwise, any changes you make to your route or config files will not update in your application.

## 2. Optimize Composer

A common mistake sometimes made by Laravel developers is to install all dependencies in production. Some development packages such as Ignition record your queries, logs, and dumps in memory to give you a friendly error message with context for ease of debugging. While this is useful in development, it can slow down your application in production.

In your deployment script, make sure to use the --no-dev flag while installing packages using Composer:

```bash
composer install --no-interaction --prefer-dist --no-dev -o 
```

Additionally, make sure to use the `-o' flag in production as above. This enables Composer to optimize the autoloader by generating a "classmap".

You may choose to use the --classmap-authoritative flag instead of the `-o' flag for further optimization if your app does not generate classes at runtime. Make sure to check out the Composer documentation on autoloader [optimization strategies](https://getcomposer.org/doc/articles/autoloader-optimization.md).

## 3. Choose the Right Drivers

Choosing the right cache, queue, and session drivers can make quite a difference to application performance.

For caching in production, we recommend the in-memory cache drivers such as Redis, Memcached, or DynamoDB. You may consider local filesystem caching for a single-server setup, although it would be slower than the in-memory options.

For queueing, we recommend the Redis, SQS, or Beanstalkd drivers. The database driver is not suitable for production environments and is known to have deadlock issues.

For sessions, we recommend the Database, Redis, Memcached, or DynamoDB drivers. The cookie driver has the file size and security limitations and is not recommended for production.

## 4. Queue Your Time-Consuming Tasks

## 5. Set Compression Headers On Text Format Files

Compression headers can have a significant impact on application performance. Ensure that you enable compression headers on your web server or CDN for text format files, like CSS, JS, XML, or JSON.

Most image formats are already compressed and are not text format files (with the exception of SVG, which is an XML document). So, image formats do not need to be compressed.

You may set up gzip or brotli (preferably both as brotli may not be supported for older browsers) at your web server or CDN level to achieve a huge performance boost.

Typically, compression would be able to reduce your file size by around 80%!

## 6. Set Cache Headers On Static Assets

Caching can provide a performance boost for your application, especially for static assets such as images, CSS, and JS files. It is recommended to enable cache-control headers at the webserver level or at your CDN level (if applicable). If you wish to set these headers at your Laravel app instead of the webserver, you may use Laravel's cache control middleware.

Cache headers ensure that browsers don't request static assets on subsequent visits to your website. This can enhance your user experience as your website loads faster on subsequent visits.

Make sure you use cache-busting so that when you change your CSS or JS code, browsers avoid relying on stale cached content. Laravel Mix provides cache busting out of the box.

## 7. Consider Using A CDN To Serve Assets

Content Delivery Networks (CDNs) are a geographically distributed group of servers that serve content closer to application visitors by using a nearby server. This enables visitors to experience faster loading times.

Besides faster loading times, CDNs also have other benefits such as decreased web server load, DDoS protection, and analytics on assets served.

Some popular CDNs include Cloudflare, AWS Cloudfront, and Azure CDN. Most CDNs are free for a certain usage threshold. Do consider using CDNs for boosting asset serving performance.

Laravel offers CDN support out of the box for Mix and the asset helper function.

## 8. Minify Your JS and CSS Code

## 9. Use Cache Wisely

Some common use cases of caching could include:

- `Caching static pages:` Caching static pages is a no-brainer. Laravel's website uses page caching for every single documentation page.
- `Fragment or partial caching:` Sometimes, instead of caching full pages, it may be useful to cache page fragments. For instance, you may want to cache the page header that contains the name of the user and a profile pic. Instead of fetching the data from the database every time, you can cache the header fragment in one go.
- `Query caching:` If your application queries the database on a high frequency for items that seldom change, it may be useful to cache the queries. For instance, if you run an e-commerce store, you might want to cache the items displayed on the store homepage rather than fetching them from the database on every store visit.

## 10. Identify Your App's Performance Bottlenecks

If some of your pages have high loading times or high memory usage, it may be essential to identify performance bottlenecks. Many tools exist within the Laravel ecosystem to help you do that, including Laravel Telescope, Laravel Debugbar, and Clockwork.

Some common performance bottlenecks include:

- `N+1 Queries:` If your code executes one query for each record, it will result in more network round trips and a larger number of queries. This can be solved in Laravel using eager loading.
- `Duplicate Queries:` If your code executes the same query more than once for the same request, it may slow down your application. Typically these issues can be solved by extracting data computation or retrieval to a separate class if multiple services or classes need the same set of data.
- `High Memory Usage:` To reduce memory usage in your application, consider using lazy collections and query chunking for reducing model hydrations. For storing files, check out automatic streaming to reduce memory usage.
- `Slow Queries:` If you have queries that are taking too long to execute, you should consider query caching and/or using explain statements to optimize query execution plans.
If you are unable to identify performance bottlenecks in your application using the debugging tools mentioned above, you may consider using profiling tools such as XDebug or Blackfire.

For detailed information
[Performance Checklist](https://laravel-news.com/performance-checklist)
[Laravel Optimization](https://geekflare.com/laravel-optimization/)