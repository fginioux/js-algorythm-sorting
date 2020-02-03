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

### Selection Sort
Principe: run loop to browse all the element of a sortable object. On every item, loop on every higher indexes to find the lowest value. Compare this value to the current item value. If the current item value is higher than the lowest value find, swap these two value. Move the pointer to the next item.

```typescript
function selectionSort (obj: Sortable): void {
  const L = obj.length;
  for (let i = 0; i < L; i++) {
    let min = i;
    for (let j = i; j < L; j++) {
      if (compare(obj[j], obj[min]) === -1) {
        min = j;
      }
    }

    if (min !== i) {
      swap(obj, i, min);
    }
  }
}
```

### Insertion Sort
Principe: move right to left position into the array the lowest values. More the table is ordered more the algorythm performances are improved.

```typescript
function insertionSort (obj: Sortable): void {
  const L = obj.length;
  for (let i = 0; i < L; i++) {
    for (let j = i; j > 0; j--) {
     if (compare(obj[j], obj[j-1]) === -1) {
       swap(obj, j, j - 1);
     } else {
       break;
     }
    }
  }
}
```

### Shell Sort
Principe: the goal is to pre-ordered part of the table on an N interval with insertion sort and reduce the interval and re-apply the insertion sort on every loop to have the best performances.

```typescript
function shellSort (obj: Sortable): void {
  // For N item to sort find the largest sorting interval
  const N = obj.length;
  let sortingInterval = 1;
  while (sortingInterval < N/3) {
    sortingInterval = 3*sortingInterval + 1;
  }
  
  while (sortingInterval >= 1) {
    for (let i = sortingInterval; i < N; i++) {
      for (let j = i; j >= sortingInterval && compare(obj[j], obj[j-h]) === -1; j -= h) {
        swap(obj, j, j-sortingInterval);
      }
    }
    
    sortingInterval = Math.round(sortingInterval/3);
  }
}
```

## Sorting Benchmark

```js
(function() {
  // Javascript Sort result 0.023 seconds
  // Best performance in the sort algorythm <> shell sort
  sortBenchmark(10000, 10, (obj: Sortable) => {
    obj.sort((a, b) => a - b);
  });
})();

(function() {
  // Selection Sort result 0.371 seconds
  // No performance variation from data into the sortable obj
  sortBenchmark(10000, 10, (obj: Sortable) => {
    selectionSort(obj);
  });
})();

(function() {
  // Insertion Sort result 1.736 seconds
  // Preformance variation depending on sortable object data
  sortBenchmark(10000, 10, (obj: Sortable) => {
    insertionSort(obj);
  });
})();

(function() {
  // Shell Sort result 0.027 seconds
  // Minimise the performence variation of the insertion sort
  // Closest performence from native javascript sort method
  sortBenchmark(10000, 10, (obj: Sortable) => {
    shellSort(obj);
  });
})();
```
