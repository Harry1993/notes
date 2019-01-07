# Python Notes

How to write Pythonic codes.

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
    for y in [3,4]:
        if x != y:
            combs.append((x, y))

combs
```

can be one-lined to

```
[(x, y) for x in [1,2,3] for y in [3,4] if x != y]
```

We can also use `itertools`:

```
[(x, y) for x, y in itertools.product(*[[1,2,3], [3,4]])]
```

where `(*[A, B])` results in `(A, B)`.

Sometimes we may want to use `numpy.npindex`:

```
[(x, y) for x, y in numpy.npindex((3, 4))]
```

which is the same to

```
out = []
for x in range(3):
	for y in range(4):
		out.append((x, y))
```

## Matplotlib

Before print the figure to PDF, make all words and plots fit:

```
plt.tight_layout(pad=0, h_pad=0, w_pad=0)
```

## zip

Use `zip` to iterate multiple lists at the same time. 

```
alist = ['a1', 'a2', 'a3']
blist = ['b1', 'b2', 'b3']

for a, b in zip(alist, blist):
    print(a, b)
```

An elegant way (from
[here](http://locallyoptimal.com/blog/2013/01/20/elegant-n-gram-generation-in-python/))
to generate n-gram using `zip` is

```
input_list = ['thisisasentencewithoutspace']

def find_ngrams(input_list, n):
  return zip(*[input_list[i:] for i in range(n)])
```

## pip

### `ImportError: No module named pip` error

Run `rm /usr/local/bin/pip*` and then `apt-get install python-pip`.

## Misc

*	`if __name__ == '__main__'` means this module is called as main.
