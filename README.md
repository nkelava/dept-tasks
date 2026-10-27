# DEPT tasks

Solutions to three small JavaScript coding exercises: splitting an array into three groups with roughly equal sums, and printing two kinds of number pyramids. Each task is a single self-contained Node.js script with no dependencies — run it and it prints its results to the console.

## Prerequisites

- [Node.js](https://nodejs.org/) (any current LTS release; the scripts use only plain ES6 and no npm packages)

## Getting started

```bash
git clone https://github.com/nkelava/dept-tasks.git
cd dept-tasks

node task_1.js
node task_2.js
node task_3.js
```

There is nothing to install and no configuration or environment variables.

## Project structure

```text
dept-tasks/
├── task_1.js   # Task 1: partition an array into 3 groups with near-equal sums (includes a small self-check)
├── task_2.js   # Task 2: print a pyramid of repeated numbers
├── task_3.js   # Task 3 (bonus): print an odd-numbered "mountain" pyramid
└── README.md
```

## Tasks

### Task 1 — Partition into three groups

Split an array into 3 groups whose sums are approximately the same.

```text
arr: [1, 3, 1, 7, 5, 6, 6, 7, 4, 2]
res: [7, 5, 1, 1], [7, 4, 3], [6, 6, 2]  ->  sums: 14, 14, 14

arr: [8, 1, 5, 2, 4, 1, 9, 8]
res: [9, 2, 1, 1], [8, 4], [8, 5]        ->  sums: 13, 12, 13
```

**How it works:** the target is `round(total / 3)`. The array is sorted in descending order, then two groups are filled greedily, skipping any value that would push a group over the target. Whatever remains becomes the third group. `sort_array(array)` returns the three groups and throws if the array has fewer than 3 elements.

The script runs `sort_array` over a built-in list of sample arrays and checks that every group's sum lies within ±1 of the target:

```text
$ node task_1.js
0:  Assertion passed.
1:  Assertion passed.
2:  Assertion passed.
3:  Error -> Invalid array length. Array has 2 element(s) but target requires at least 3.
4:  Assertion failed.
```

Cases 3 and 4 (`[8, 1]` and `[1, 3, 1]`) show how invalid or unbalanceable input is reported. To try your own input, add an array to `array_list` at the bottom of the file.

> Note: the descending sort uses JavaScript's default (string) comparison, so it orders single-digit values correctly but not multi-digit ones.

### Task 2 — Number pyramid

Given a height `n`, print `n` rows. Row `k` contains the number `n - k + 1` repeated `k` times, centered:

```text
$ node task_2.js
 2
1,1

  3
 2,2
1,1,1

     6
    5,5
   4,4,4
  3,3,3,3
 2,2,2,2,2
1,1,1,1,1,1
```

The script runs heights `0, 2, 3, 6`; a height of `0` produces nothing. Change the list at the bottom of the file to try other values.

### Task 3 (bonus) — Odd "mountain" pyramid

The height must be an odd number, and each row starts and ends with an odd number while the values climb toward the middle:

```text
$ node task_3.js
  3
1,2,1

    5
  3,4,3
1,2,3,2,1

      7
    5,6,5
  3,4,5,4,3
1,2,3,4,3,2,1
```

The script runs `0, 2, 3, 5, 6, 7`; zero and even values are rejected and print nothing.

In both pyramid scripts, `generate_pyramid(height, 1)` builds the rows as nested arrays (recursively) and `display_pyramid` handles the padding and printing, so you can reuse the generator on its own.
