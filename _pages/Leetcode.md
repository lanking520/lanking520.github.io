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

### Question 2 2016-09-06 (Need to rework Later)

Struggling 3 hours. Remember, Algorthm and Pseudo Code first, coding later.
Linked-List Question
update by 
```java
Node a.next = new Node();
a = a.next
```
however, to do some sum up question, you have to go through all of the digit. Or it could be chaos if you add some crack-filling, too much and you cannot get it solved.
```java
int x = (p != null) ? p.val : 0;
//the same as
if (p != null)
{
  int x = p.val;
}
else
{
  int x = 0;
}
```
