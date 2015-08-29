---
name: View Access
desc: Accessing the underlying view object during the binding process.
---

A binder can also obtain access to the view object that is being bound to.

    ruby:
    binding :foo do
      { 
        view: lambda { |view|
          view.prepend('some html')
        }
      }
    end
