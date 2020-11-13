# pthread

A naive bindings for pthread in [Carp](https://github.com/carp-lang/Carp)

## Example

```clojure
(load "git@github.com:TimDeve/pthread@v0.1.1")

(use-all Thread Result Array)

(defn main []
  (let-do [mutex &(Mutex.new [])
           fun-access &(fn [arr]
                           (push-back! arr (Int.random-between 0 10000)))
           fun-thread &(fn [] (Mutex.access mutex fun-access))]
    (Random.seed)
    (ignore (join-all (repeat 1000 &(fn [] (unsafe-from-success (create fun-thread))))))
    (Mutex.access mutex &(fn [arr] (println* (str arr))))))
```

