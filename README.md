# `deps.edn` User Config

Very similar to lein's `project.clj`. This can be provided via in any of the
following locations:


- Root - found in the installation of clj (or as a resource in tools.deps)
- User - cross-project configuration (typically tools)
      Locations used in this order:
          If $CLJ_CONFIG is set, then use $CLJ_CONFIG (explicit override)
          If $XDG_CONFIG_HOME is set, then use $XDG_CONFIG_HOME/clojure (Freedesktop conventions)
          Else use $HOME/.clojure (most common)
- Project - the deps.edn in the current directory
- External - a deps.edn map passed on the command line


The maps are merged in the order:
> root/user/project/config
with `merge-with merge` except for `:paths` where the most recent value wins.

[Sean Corfield](https://github.com/seancorfield/dot-clojure) and
[Practicalli](https://github.com/practicalli/clojure-deps-edn) both provide nice,
well documented examples of user `deps.edn` files that you can pull from or use
outright.

I personally mostly use it to add additional dependencies that don't need to be
part of the project and may not be used by other developers on the project. I
nest them under `:aliases` just so I can easily turn them on or off as needed.

# `(tap> x)` and `add-tap`
Acts like a little pub/sub system within Clojure for dispatching values to a set
of functions set up by `add-tap`. Each function will be called with the value you
send via `tap>`. I mostly use it as improved print line debugging, but you can
send any value from anywhere in the system, so I've also built an integration
to send Kaocha test run results and figured out how to hook it into timbre to
view logs when running tests in Bengal.

Prior to using Portal I also used it to store values in a single location in the
repl so I could manipulate them later, but Portal offers the same functionality.

> Show timbre hook up and sending to an atom

You can also use `tap>` in CLJS, Shadow has built in tooling to view and explore
tapped values. It's not quite as nice as Portal, but I've generally found it
sufficient for what I'm doing.

# [Portal](https://github.com/djblue/portal)

Sort of like a terminal window that can receive print line statements, with the
advantage that it receives actual Clojure values rather than simple strings. You
can drill into nested values, manipulate how values are displayed, execute a
subset of Clojure functions built into Portal, or pull a selected value back into
the repl to manipulate via Clojure directly.

> Show Portal, Kaocha connection and `#tap` tagged literal

My hacked together tools:
https://github.com/gregcline-reify/portal-kaocha
https://github.com/gregcline-reify/tap-reader
