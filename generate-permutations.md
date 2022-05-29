+++
title = "Generate Permutations"
author = ["Suraj Yadav"]
date = 2022-05-28
tags = ["solution"]
draft = false
+++

## What is permutation? {#what-is-permutation}

-   What is permutation of `{ }`?
    -   `{ }`

-   What is permutation of `{1}`?
    -   `{1}`

-   What is permutation of `{1, 2}`?
    -   `{1, 2}`
    -   `{2, 1}`

-   What is permutation of `{1, 2, 3}`?
    -   `{1, 2, 3}`
    -   `{1, 3, 2}`
    -   `{2, 1, 3}`
    -   `{2, 3, 1}`
    -   `{3, 1, 2}`
    -   `{3, 2, 1}`

-   Which of them start with `1`?
    -   `{1, 2, 3}`
    -   `{1, 3, 2}`

-   What is the rest of these elements?
    -   `{2, 3}`
    -   `{3, 2}`

-   Do they resemble something?
    -   No

-   What about permutation of `{2, 3}`?
    -   Yes

-   What is append x on {y, z}?
    -   {x, y, z}

-   What is append x on each {y, z}?
    -   {x, y}
    -   {x, z}

-   Can we say Permutation of `{1, 2, 3}` =
    `{append 1 on each permutation of {2, 3}}, {append 2 on each permutation of {1, 3}}, {append 3 on each permutation of {1, 2}}}`?
    -   Thats a mouthful

-   What is `{append 1 on each permutation of {2, 3}}`?
    -   `{append 1 on each permutation of {2, 3}}`
    -   `{append 1 on each {{2, 3}, {3, 2}} }`
    -   `{{1, 2, 3}, {1, 3, 2}}`

-   What about `{append 2 on each permutation of {1, 3}}, {append 3 on each permutation of {1, 2}}`?
    -   `{{2, 1, 3}, {2, 3, 1}, {3, 1, 2}, {3, 2, 1}}`

-   What do we get when we club them all?
    -   `{{1, 2, 3}, {1, 3, 2}, {2, 1, 3}, {2, 3, 1}, {3, 1, 2}, {3, 2, 1}}`
    -   ie `permutation of {1, 2, 3}`

-   Do you know what permutation is now?
    -   Yes


## Coding it {#coding-it}

We'll be using functional javascript.

<a id="code-snippet--permute"></a>
```js

function permute(array){
	<<permute-base-case>>
	<<permute-reduction-case>>
}
```


### BASE CASE {#basecase}

The base case for permutation is array with 0 or 1 element

| Input | Output |
|-------|--------|
| [ ]   | [ ]    |
| [x]   | [x]    |

<a id="code-snippet--permute-base-case"></a>
```js
if (array.length <= 1){
    return array
}
```


### REDUCTION CASE {#reduction-case}

For the reduction case

-   loop every element in array
-   head = looping element
-   rest = remove head from array
-   rest_permutation = get permutation of rest
-   append head on every rest_permutation

<!--listend-->

<a id="code-snippet--permute-reduction-case"></a>
```js

return array.map(item => {
    const head = item
    const rest = remove(head, array)
    const rest_permutation = permute(rest)
    return rest_permutation.map(x => append(head, x))
})
```

You should not worry about the utility functions `remove`, `append` as they are trivial and you can write them yourself.

Here's the implementation of utility functions

<a id="code-snippet--utils"></a>
```js
function clone(x){
    return JSON.parse(JSON.stringify(x))
}

function remove(item, array){
    const cloned = clone(array)
    cloned.splice(cloned.indexOf(item), 1)
    return cloned
}

function append(item, array){
    return [item].concat(array)
}


```


### Testing {#testing}

Lets test to assert the algorithm correctness


#### Full test code {#full-test-code}

<a id="code-snippet--test"></a>
```js

function clone(x){
    return JSON.parse(JSON.stringify(x))
}

function remove(item, array){
    const cloned = clone(array)
    cloned.splice(cloned.indexOf(item), 1)
    return cloned
}

function append(item, array){
    return [item].concat(array)
}


function flatten(array){
    return array.reduce((acc, val) => acc.concat(val), []);
}


function permute(array){
	if (array.length <= 1){
	    return array
	}

	return array.map(item => {
	    const head = item
	    const rest = remove(head, array)
	    const rest_permutation = permute(rest)
	    return rest_permutation.map(x => append(head, x))
	})
}
```


#### Testing 1 {#testing-1}

```js

return permute([1])

```

```text
[ 1 ]
```


#### Testing 2 {#testing-2}

```js

return permute([1, 2])

```

```text
[ [ [ 1, 2 ] ], [ [ 2, 1 ] ] ]
```

This is odd.

-   Did we miss something?
    -   Yes

-   What should `permutation of {1, 2}` look like?
    -   `[[1, 2], [2, 1]]`

-   If only there was a way to flatten the lists
    <a id="code-snippet--flatten"></a>
    ```js

    function flatten(array){
        return array.reduce((acc, val) => acc.concat(val), []);
    }

    ```

Lets integrate it in the Reduction Case

<a id="code-snippet--new-permute-reduction-case"></a>
```js
return flatten(array.map(item => {
    const head = item
    const rest = remove(head, array)
    const rest_permutation = permute(rest)
    return rest_permutation.map(x => append(head, x))
}))
```

The final code is

<a id="code-snippet--final"></a>
```js

function clone(x){
    return JSON.parse(JSON.stringify(x))
}

function remove(item, array){
    const cloned = clone(array)
    cloned.splice(cloned.indexOf(item), 1)
    return cloned
}

function append(item, array){
    return [item].concat(array)
}


function flatten(array){
    return array.reduce((acc, val) => acc.concat(val), []);
}


function permute(array){
	if (array.length <= 1){
	    return array
	}
	return flatten(array.map(item => {
	    const head = item
	    const rest = remove(head, array)
	    const rest_permutation = permute(rest)
	    return rest_permutation.map(x => append(head, x))
	}))
}

```


#### Testing 2 again {#testing-2-again}

```js



return permute([1, 2])

```

```text
[ [ 1, 2 ], [ 2, 1 ] ]
```


#### Testing 3 {#testing-3}

```js



return permute([1, 2, 3])

```

```text
[
  [ 1, 2, 3 ],
  [ 1, 3, 2 ],
  [ 2, 1, 3 ],
  [ 2, 3, 1 ],
  [ 3, 1, 2 ],
  [ 3, 2, 1 ]
]
```


#### Testing 4 {#testing-4}

```js



return permute([1, 2, 3, 4])

```

```text
[
  [ 1, 2, 3, 4 ], [ 1, 2, 4, 3 ],
  [ 1, 3, 2, 4 ], [ 1, 3, 4, 2 ],
  [ 1, 4, 2, 3 ], [ 1, 4, 3, 2 ],
  [ 2, 1, 3, 4 ], [ 2, 1, 4, 3 ],
  [ 2, 3, 1, 4 ], [ 2, 3, 4, 1 ],
  [ 2, 4, 1, 3 ], [ 2, 4, 3, 1 ],
  [ 3, 1, 2, 4 ], [ 3, 1, 4, 2 ],
  [ 3, 2, 1, 4 ], [ 3, 2, 4, 1 ],
  [ 3, 4, 1, 2 ], [ 3, 4, 2, 1 ],
  [ 4, 1, 2, 3 ], [ 4, 1, 3, 2 ],
  [ 4, 2, 1, 3 ], [ 4, 2, 3, 1 ],
  [ 4, 3, 1, 2 ], [ 4, 3, 2, 1 ]
]
```

-   Did you have fun?
    -   Go have some cookies
