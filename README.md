# acknowledge

A basic server toolkit with no in-source centralized handlers. Built on top of `bidi`, so should be compatible with servers other than `http-kit`, but it's the one I use.

## Usage

`[acknowledge "0.2.2-SNAPSHOT"]`

```
(ns my-project.core
  (:require
   [org.httpkit.server :as httpkit]
   [acknowledge.core :as handlers]))

(handlers/intern-static! "/" (handlers/resources "public/"))
(handlers/intern-error-fn!
  :not-found
  (fn [req]
    {:status 404 :headers {"Content-Type" "text/html; charset=utf-8"}
	 :content "<html><body><p>Custom not-found handler :p</p></body></html>"}))

(handlers/intern-handler-fn!
  "/" :render-index (fn [req] {:status 200 :headers {"Content-Type" "text/plain"} :body "This is an example index handler"}))

(defn -main
  ([] (-main "8000"))
  ([port] (httpkit/run-server handlers/routes-handler {:port (Integer. port)})))
```

## License

Copyright © 2017 FIXME

Distributed under the Eclipse Public License either version 1.0 or (at
your option) any later version.
