# Leetcode Assignment

## Algorithm

### Question 1 2016-08-29

#### My Submission
```java
public class Solution {
    public int[] twoSum(int[] nums, int target) {
        int sum = target + 1;
        int[] result = {0,0};
        for (int i = 0; sum != target; i++)
        {
            for (int j = i+1; j< nums.length; j++)
            {
                sum = nums[i]+nums[j];
                if (sum == target)
                {
                    result[0] = i;
                    result[1] = j;
                    break;
                }
                else
                {
                    sum = 0;
                }
            }
        }
        return result;
    }
}
```

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

### Question 3 2016-09-07 (Algorithm First)

#### Do this project into the following steps:
- Think the algorithm first
- Write Pseudo code 
- Start coding

#### Algorithm Thinking
```

Take the small half of the substring: length = (int) string.length()/2

for length; length >= 1; length—
	for i =0; i+length < size; i++
		for j = i+2length; j <size; j++
		compare string (i—i+length) and string (i+length—i+2length) (if i+ 2length) < size
			compare success>=return the length
		else: break
	
return 0


public String substring(int beginIndex, int endIndex)
```
#### Howerver Read the question carefully!!!
Solution for finding no more than 2 longest substring
```java
public class Solution {
    public int lengthOfLongestSubstring(String s) {
        int length = (int) (s.length() / 2);
        int counter = 0;
        for (;length >= 1;length--)
        {
            for(int i = 0; i+length <= s.length();i++)
            {
                for(int j= i; j+length <= s.length(); j++)
                {
                    if(s.substring(i, i+length).equals(s.substring(j, j+length)))
                    {
                        counter += 1;
                    }
                }
                if (counter == 2)
                {
                    return length;
                }
                counter=0;
            }
        }
    return s.length();    
    }
}
```
#### Working but time exceeded!!!!

Thinking:

Starting from the longest substring
find duplicated letter, if there is, drop this
loop until 0 letter remains 

Considering we have massive string list
```
asldaslkdjaslkjdaksldklasjdlsjdlkajld...
```
Brute Force: We will loop it until the final combination! (o(n^4))!!!!!
```java
public class Solution {
    public int lengthOfLongestSubstring(String s) {
        int length = s.length();
        boolean repeat = false;
        for(;length >=1; length--)
        {
            for (int i = 0; i+ length<=s.length(); i++)
            {
                String my_string = s.substring(i, i+length);
                for (int j = 0; j < my_string.length() && repeat == false; j++)
                {
                    for (int k = j+1; k < my_string.length() && repeat == false;k++)
                    {
                        if(my_string.charAt(j) == my_string.charAt(k))
                        {
                            repeat = true;
                        }
                    }
                }
                if (!repeat)
                {
                    return length;
                }
                repeat = false;
            }
        }
        return 0;
    }
}
```
#### Sliding Window
Use Hashset to open and close the boundary. Keep a maximum of the length, move through all of the character
```java
Set<Character> set = new HashSet<>();
```
[i,j] two variable control the front and backside of the boundary. Check if j* contains in the set and decide whether i++

### Question 11 Find the large container

#### Brute Force Solution

For a1 in array()

	for a2 in array()
	
		int ans = Math.max(Math.min(a1,a2) * (a2i - a1i));

complex n^2 (Time Limit Exceeded)

#### (Answer) Two Pointer Approach
Always finding the possible maximum result
#### Remember to place "()" in your formula, it's important! => "(max_b - min_b) >= 1" Not the same as "max_b - min_b >= 1"
