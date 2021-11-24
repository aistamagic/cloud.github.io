
# Dynamic Hyperlambda slots

A dynamic Hyperlambda slot is the equivalent of what you would refer to as a _"function"_ in other programming
languages, since it allows you to declare a dynamic lambda object, you can pass arguments to, return arguments
from, and treat the same way you'd treat a function in a more traditional programming language. Consider the
following code.

```
slots.create:foo.bar
   config.get:"magic:databases:default"
   return:x:-
```

The above create a _"dynamic slot"_ for you that you can invoke using the following.

```
signal:foo.bar
```

Assuming you're using MySQL as your default database provider, the above will return the following.

```
signal:mysql
```

A dynamic slot can contain any amount of Hyperlambda you wish, allowing you to encapsulate your logic
into a single point, from where you can reuse it later as you see fit. Below is an example of a slightly
more useful slot, returning all roles that exists in your particular installation.

```
slots.create:foo.bar.get-roles
   data.connect:[generic|magic]
      data.read
         table:roles
         columns
            name
               as:.
      return:x:@data.read/*/*
```

Invoking the above slot with `signal:foo.bar.get-roles` would result in something
resembling the following.

```
signal
   .:admin
   .:blocked
   .:chat-admin
   .:guest
   .:moderator
   .:reset-password
   .:root
   .:translator
   .:unconfirmed
```

The following slots exists in Hyperlambda allowing you to create, signal/invoke, and administrate your
dynamic slots.

* __[signal]__ - Invokes an already existing dynamic slot slot
* __[slots.create]__ - Creates a new dynamic slot
* __[slots.delete]__ - Deletes an existing dynamic slot
* __[slots.exists]__ - Returns true if the specified dynamic slot already exists
* __[slots.get]__ - Returns the lambda object associated with a slot
* __[slots.vocabulary]__ - Returns all dynamic slots to caller, similar to **[vocabulary]**, except will _only_ return dynamic slots.

## Persisting dynamic slots

When you create a dynamic slot, it only exists in memory. If for some reasons your web server falls down, your
slot will cease to exist. To create a dynamic slot that is always re-created as your web server start, and/or your
module is installed, you'll have to put your __[slots.create]__ invocation into a file inside your
module's _"magic.startup"_ folder. For instance, if your module is called _"foo"_, and your slot is
named __[foo.bar]__, typically the full name of this file would be _"/modules/foo/magic.startup/foo.bar.hl"_.

All Hyperlambda files existing within your module's _"magic.startup"_ folder will be automatically executed
as the system restarts, and/or is installed.

## Namespacing your slots

Notice, when you invoke __[slots.create]__ any existing slots with the same name will simply be overwritten.
This is intentional, since it creates a simple _"polymorphistic"_ behaviour. However, this implies you need
to carefully _namespace_ your slots. Our advice is to use the name of your company, then the name of your
application/module, for then to add something describing what your slot actually does. An example of this can
be found below.

```
slots.create:acme-company.foo-module.meaning-of-life
   return:int:42
```

This prevents one module from interferring with objects in another module, by resulting in a unique namespace,
internally within your server, and/or also globally to some extent, and also makes your code more maintainable
in the long run.

## Wrapping up

In this article we walked you through how to invoke HTTP endpoints using Hyperlambda. We talked about HTTP headers,
automatic conversion back and forth between lambda objects and JSON, in addition to some additional features of
the HTTP slots in Hyperlambda.

* [Documentation](/documentation/)