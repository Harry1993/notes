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

## Matplotlib

Before print the figure to PDF, make all words and plots fit:

```
plt.tight_layout(pad=0, h_pad=0, w_pad=0)
```

## Misc
