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
In the `hash_table_v1_add_entry` function, 
A mutex is added to the struct hash_table_v1. It is initialized in hash_table_v1_create and destroyed during hash_table_v1_destroy. 
This mutex locks the entire hash table once the process starts looking for entries and unlocks just before returning, which happens once it has either modified an existing value or has added a new value. 
This completely covers the critical section as only one thread will be able to modify or search the hash table at once, so there will be no races. 

### Performance
running 
```shell
./hash-table-tester
```
we see that the base verion runs for around 50,000 usec while hash_table_v1 runs for 120,000 usec. This makes sense as the mutex we implemented is coarse and will completely lock down the entire hash table. The program essentially has no parallelism and is therefore slower. 


## Second Implementation
In the `hash_table_v2_add_entry` function, 
An array of mutexes with length equal to the number of hash table entries is added to the struct hash_table_v2. Each mutex is initialized in hash_table_v2_create and destroyed during hash_table_v2_destroy. 
When running hash_table_v2_add_entry, the hash key is calculated to get the index of the hash table entry to be modified.Then, the mutex associated with that index of the hash table is locked once the thread begins to search the linked list. 
The mutex is unlocked just before the funciton is going to return, which occurs either after it has modified an existing value or has added a new value. 
This completely covers the critical section as only one thread will be able to modify or search that index of the hash table at once. 
Since we can use multiple mutexes, our lock is much more fine-grained, as it can allow other threads to use the hash table while it is running.

One other modification made to add_entry was that the reuse of the searching list_entry had been instead turned into its own list entry. The list entry would be created outside of the lock, would either be added or free()ed after unlocking depeneding on if the program would replace a value or not. Make the node (but not adding it to the linked list) is not part of the critical section, so it does not need to be locked for accuracy reason. 


### Performance
running 
```shell
./hash-table-tester
```
we see that compared to the base verion of 50,000 us, hash_table_v2 runs only 16,000us. This makes sense as our fine-grained lock allows for multiple threads to use the table at once, increasing parallelism and speed. 

 
## Cleaning up
to clean
```shell
make clean

```