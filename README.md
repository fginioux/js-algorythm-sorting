# js-algorythm-sorting
List of Algorythms to sort Array in Javascript with Benchmark

## Generic code

#### Array generator

```typescript
declare type Sortable = {
  length: number;
};

function ShuffledArrayGenerator (nbSortableItems: number, rangeLimit: number = 1001): Sortable {
  const T = [];
  for (let i = 0; i < nbSortableItems; i++) {
    T.push(Math.floor(Math.random() * rangeLimit));
  }

  return T;
}
```

#### Benchmark function

```typescript
function sortBenchmark (nbSortableItem: number, nbRoundOfTest: number, sortFn: Function): number {
  let result = 0;

  for (let i = 0; i < nbRoundOfTest; i++) {
    let T = sortableGenerator(nbSortableItem);
    let start = Date.now();
    sortFn(T);
    result += Date.now() - start;
  }

  return (result / 1000).toFixed(3);
}
```

#### Sorting utils methods

```typescript
function swap (obj: Sortable, i: number, j: number): void {
  [obj[i], obj[j]] = [obj[j], obj[i]];
}

function compare (a: any, b: any) {
  return a < b ? -1 : a > b ? 1 : 0;
}
```

## Sorting natif javascript

```js
(function() {
  sortBenchmark(10000, 10, (obj: Sortable) => {
    obj.sort((a, b) => a - b);
  });
})();
```
