# Python Notes

How to write Pythonic codes.

## Module

*	`if __name__ == '__main__'` means this module is called as main.

## List

### List Comprehensions

```
squares = []
for x in range(10):
    squares.append(x**2)

squares	
```

is equivalent to

```
squares = [x**2 for x in range(10)]
```

More complicated,

```
combs = []
for x in [1,2,3]:
    for y in [3,1,4]:
        if x != y:
            combs.append((x, y))

combs
```

is equivalent to

```
[(x, y) for x in [1,2,3] for y in [3,1,4] if x != y]
```

## Misc
