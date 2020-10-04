# sentry-clj

[![Clojars Project](https://img.shields.io/clojars/v/io.sentry/sentry-clj.svg)](https://clojars.org/io.sentry/sentry-clj)

A very thin wrapper around the [official Java library for Sentry](https://docs.sentry.io/clients/java/).

This project follows the version scheme MAJOR.MINOR.COMMITS where MAJOR and
MINOR provide some relative indication of the size of the change, but do not
follow semantic versioning. In general, all changes endeavour to be
non-breaking (by moving to new names rather than by breaking existing names).
COMMITS is an ever-increasing counter of commits since the beginning of this
repository.

## Usage

```clojure
(require '[sentry-clj.core :as sentry])

; you can initialize Sentry manually by providing a DSN in the code (as shown below),
; or use one of the other configuration options (such as environment variables) described
; in the Java SDK documentation (which will automatically initialize Sentry the first
; time you attempt to send an event): https://docs.sentry.io/clients/java/config/

(sentry/init! "https://public:private@sentry.io/1")

(try
  (do-something-risky)
  (catch Exception e
    (sentry/send-event {:throwable e})))
```

## Supported event keys

- `:breadcrumbs` - a collection of `Breadcrumb` maps. See below.
- `:dist` - a `String` which identifies the distribution.
- `:environment` - a `String` which identifies the environment.
- `:event-id` - a `String` id to use for the event. If not provided, one will be automatically generated.
- `:extra` - a map with `Keyword` or `String` keys (or anything for which `clojure.core/name` can be invoked) and values which can be JSON-ified. If `:throwable` is given, this will automatically include its `ex-data`.
- `:fingerprint` - a sequence of `String`s that Sentry should use as a [fingerprint](https://docs.sentry.io/learn/rollups/#customize-grouping-with-fingerprints).
- `:level` - a `Keyword`. One of `:debug`, `:info`, `:warning`, `:error`, `:fatal`. Probably most useful in conjunction with `:message` if you need to report an exceptional condition that's not an exception.
- `:logger` - a `String` which identifies the logger.
- `:message` - a `String`. This is a different message than that of a Throwable.
- `:platform` - a `String` which identifies the platform.
- `:release` - a `String` which identifies the release.
- `:request` - a map containing Request information. See below.
- `:server-name` - a `String` which identifies the server name.
- `:tags` - a map with `Keyword` or `String` keys (or anything for which `clojure.core/name` can be invoked) and values which can be coerced to `Strings` with `clojure.core/str`.
- `:throwable` - a `Throwable` object. Sentry's bread and butter.
- `:transaction` - a `String` which identifies the transaction.
- `:user` - a map containing User information. See below.

### Breadcrumbs

Read more about breadcrumbs at [docs.sentry.io/learn/breadcrumbs/](https://docs.sentry.io/learn/breadcrumbs/).

When an event has a `:breadcrumbs` key, each element of the value collection should be a map.
Supported keys of the map are:

- `:type` - A `String`
- `:level` - a `String`
- `:message` - a `String`
- `:category` - a `String`
- `:data` - a map with `String` keys and `String` values

### User

When an event has a `:user` key, the data should be contained within a map, thus:

- `:email` - A `String`
- `:id` - A `String`
- `:username` - A `String`
- `:ip-address` - A `String`
- `:other` - A map containing key/value pairs of `Strings`, i.e., `{"a" "b" "c" "d"}`

### Request

When an event has a `:request` key, the data should be contained within a map, thus:

- `:url` - A `String`
- `:method` - A `String`
- `:query-string` - A `String`
- `:data` - An arbitrary value (e.g., a number, a string, a blob...)
- `:cookies` - A `String`
- `:headers` - A map containing key/value pairs of `Strings`, i.e., `{"a" "b" "c" "d"}`
- `:env` - A map containing key/value pairs of `Strings`, i.e., `{"a" "b" "c" "d"}`
- `:other` - A map containing key/value pairs of `Strings`, i.e., `{"a" "b" "c" "d"}`

## License

Copyright © 2020 Coda Hale, Sentry

Distributed under the Eclipse Public License either version 1.0 or (at
your option) any later version.
