
> [!warning] What if the same id is given to multiple styled elements?
> CSS will style all the elements will the specified id. There are no conflicts as far as CSS is concerned.
> 
> Problems arise in the JavaScript side of things where functions like `getElementById` will return the first element it finds with that `id`. This can lead to unexpected behavior, as it won’t reference the second element with the same `id`.
