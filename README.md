# Vue3 Multifunctional List

## About

Multifunctional list built with Vue3 Composition API.

### Search Bar

- Non-empty value can be cleared through the clear button, or by pressing Escape
  key.
- If the search bar is empty, the clear and add buttons becomes hidden.
- If searched string (case insensitive) is already present in the list, the add
  button becomes disabled and appropriate item markes as "Exact match".
- If no exact match is found, the add button becomes enabled and click on it (or
  by pressing Enter key) adds the string value to the list.

### List

- List data stores in
  [localStorage](https://developer.mozilla.org/en-US/docs/Web/API/Window/localStorage).
- Each item in list keeps track of when that item was added to the list.
- Each item tracks in what order it was added in the following way:
  - Starting from #1
  - New item # is equal to maximum number in list + 1
  - Deleting an item from list doesn't recalculate # of the remaining items

### Sorting

- List is sortable by either string value (from a to z) or by added date.

## Project setup

```bash

# install

npm ci

# serve with hot-reload for development

npm run serve

```
