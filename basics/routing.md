# Routing

A central task solved by Pagekit is *Routing*. When the browser hits a URL,
the framework will determine which controller action will be called.

If a route is not unique across your application, the one that has been added
first is the one being used. As this is framework internal behavior that might
change, you should not rely on this but rather make sure your routes are unique. If no route exists for a requested URL, a 404 error will be shown.

Routes can either be defined inside a [module definition]() or typically within a [controller]()