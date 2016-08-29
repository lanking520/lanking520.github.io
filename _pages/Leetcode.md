# Leetcode Assignment

## Algorithm

### Question 1 2016-08-29

Remember to throw the exception if there is one:

```java
throw new IllegalArgumentException("No two sum solution");
```

Also remember, Java is not supporting group assigning afterwards

```java
do
return new int[] { i, j };
```

or assign them manually

Hashmap would reduce the searching time of an element to O(1).

```java
Map<Integer, Integer> map = new HashMap<>() //Creating a hashmap
map.put(nums[i], i) //put key and value to it
map.containsKey(complement) //check if there is a key regarding to complement
map.get(complement) //get the value of this key
```
