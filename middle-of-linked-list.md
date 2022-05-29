+++
title = "Middle of Linked List"
author = ["Suraj Yadav"]
date = 2022-05-29
draft = false
+++

## 2 Pointers {#2-pointers}

-   In the list `1->2->3->4->5`, `S` and `F` point to 1. What are `S` and `F` when `S` moves by 1 element and `F` moves by 2 elements?
    -   `S` is slow pointer and `F` is fast pointer

-   Will `S` travel half the distance of `F` at any given point?
    -   Yes, since `F` is twice as fast as `S`

-   Will `S` be at middle of list when `F` is at the end?
    -   Yes

-   Do we have our base case and reduction case?
    -   Yes we do


## Code {#code}

Defining ListNode

<a id="code-snippet--structure-def"></a>
```ruby
class ListNode
  attr_accessor :val, :next
  def initialize(val = 0, _next = nil)
    @val = val
    @next = _next
  end
end
```

Center method

<a id="code-snippet--center"></a>
```ruby
class ListNode
  def center(slow = self, fast = self)
      <<base-case>>
      <<reduction-case>>
  end
end
```


### BASE CASE {#base-case}

If the `fast` pointer is at the end, `slow` is at mid.

<a id="code-snippet--base-case"></a>
```ruby
if not (fast.next and fast.next.next)
  return slow
end
```


### REDUCTION CASE {#reduction-case}

We keep moving `slow` by 1 and `fast` by 2 to reach closer to base case

<a id="code-snippet--reduction-case"></a>
```ruby
return center(slow.next, fast.next.next)
```


## Testing {#testing}

Defining utility to create a LinkedList from array

<a id="code-snippet--util"></a>
```ruby
def list_from_array(array)
  if array.length == 0
    return nil
  end
  node = ListNode.new(array[0])
  node.next = list_from_array(array.drop(1))
  return node
end

```

Test code

<a id="code-snippet--test"></a>
```ruby

class ListNode
  attr_accessor :val, :next
  def initialize(val = 0, _next = nil)
    @val = val
    @next = _next
  end
end

class ListNode
  def center(slow = self, fast = self)
      if not (fast.next and fast.next.next)
        return slow
      end
      return center(slow.next, fast.next.next)
  end
end

def list_from_array(array)
  if array.length == 0
    return nil
  end
  node = ListNode.new(array[0])
  node.next = list_from_array(array.drop(1))
  return node
end


```


### Testing even length list {#testing-even-length-list}

```ruby


   puts list_from_array(Array(1..10)).center.val

```

```text
5
```


### Testing odd length list {#testing-odd-length-list}

```ruby


   puts list_from_array(Array(1..11)).center.val

```

```text
6
```
