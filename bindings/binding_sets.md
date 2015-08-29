---
name: Binding Sets
desc: Grouping bindings into named sets.
---

Just like routes, bindings can be grouped into named sets:

    ruby:
    Pakyow::App.bindings :my_set do
      # bindings defined here
    end
