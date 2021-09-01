# Create / update htpassword

Example with add or replace patchesJson6902

```
- op: add
  path: /users/-
  value:
    ops1

- op: add
  path: /users/-
  value:
    ops2
```

or 

```
- op: replace
  path: /users
  value: 
  - dev1
  - dev2
  - dev3

```
