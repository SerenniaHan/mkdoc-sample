# Quic Start


## Csharp Code
```csharp
public class Person
{
    public string Name {get;set;}
}
```


## Python Code

The [Python Code] code is a sample python code paragraph.

``` py title="bubble_sort.py" linenums="1" hl_lines="2 3"
def bubble_sort(items):
    for i in range(len(items)):
        for j in range(len(items) - 1 - i):
            if items[j] > items[j + 1]:
                items[j], items[j + 1] = items[j + 1], items[j]
```

[Python Code]: #python-code

## Highlighting inline code blocks

The `#!python range()` function in python is used to generate a sequence of numbers.

## Mermaid Graph
``` mermaid
graph LR
  A[Start] --> B{Error?};
  B -->|Yes| C[Hmm...];
  C --> D[Debug];
  D --> B;
  B ---->|No| E[Yay!];
```
