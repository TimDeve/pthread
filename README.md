# pthread

A naive bindings for pthread in [Carp](https://github.com/carp-lang/Carp)

## Example

```clojure
(load "git@github.com:TimDeve/pthread@v0.1.1")

(use-all Result Array)

(defn push-rand-int [arr]
  (push-back! arr (Int.random-between 0 10000)))

(defn main []
  (let-do [mutex &(Mutex.new [])
           callback &(fn [] (Mutex.access mutex &push-rand-int))]
    (Random.seed)
    (ignore
      (Thread.join-all
        (repeat 1000 &(fn [] (unsafe-from-success (Thread.create callback))))))
    (Mutex.access mutex &(fn [arr] (println* (str arr))))))
```

