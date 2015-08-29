---
name: Modifying Values
desc: Modifying existing view attributes during the binding process.
---

A binder can modify an existing value by setting the value to a lambda:

    ruby:
    binding :foo do
     { class: lambda { |klass| klass << 'class_name' } }
    end
