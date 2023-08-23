# Questions

## Components

```txt
+--- bareos-fd ---+    +--- bareos-dir ---+    +--- catalog ---+
| name            | -> | name             | -> | postgresql    |
| password        | <- | password         |    +---------------+
+-----------------+    +------------------+
      ^ |
      | v
+--- bareos-sd ---+    +--- media ---+
| name            | -> |             |
|                 |    +-------------+
+-----------------+
```

I don't understand the password that each role/component needs. Are the `fd` and `sd` referring to a password set on `dir`?

## Structure

The current code "does it all".

I see two options:

### Combined

This is the current code.

Drawbacks:

- Mixed variables, can be confusing and lead to more conditionals.
- Testing takes longer. The role needs to be called 3 times, once for each component.

### Split

An option is to split, something like this:

1. bareos_repo
2. bareos_dir (Depends on bareos_repo)
3. bareos_sd (Depends on bareos_repo)
4. bareos_fd (Depends on bareos_repo)

Drawbacks:

- More roles to maintain.
- Sometimes difficult to decide where to put what logic.
