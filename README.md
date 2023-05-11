# Hash Hash Hash
This lab creates a hash table by adding methods using threads. The table is an array of linked lists(SLIST). It will compare the speed of and creating a hash table between a base case, a case using one mutex, and a case using multiple mutexes. 

## Building
to build, run:
```shell
make

```

## Running
after building, to run: 
```shell
./hash-table-tester
```
options: 
-t [int]
this option will dictate the number of threads involved. it has a default value of 4

-s [int]
this option dictates the numbe rof entries to be added by each thread. It has a default value of 25000

an example of how this code could be run: 
```shell
./hash-table-tester -t 8 -s 50000
```

## First Implementation
In the `hash_table_v1_add_entry` function, xxx

### Performance
```shell

```

This time version 1 is xxx

## Second Implementation
In the `hash_table_v2_add_entry` function, xxx

### Performance
```shell

```

This time the speed up is xxx

## Cleaning up
to clean
```shell
make clean

```