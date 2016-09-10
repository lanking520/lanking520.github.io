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
Always finding the possible maximum result.

Two Pointer means: there is always two pointer reduce the arraysize to find the best result.

#### Remember to place "()" in your formula, it's important! => "(max_b - min_b) >= 1" Not the same as "max_b - min_b >= 1"

### Easy 3 Medium 2
"Do not allocate extra space for another array" Statement means keep the original memory (no new array, set, etc)
#### Q26 Two pointer
Always keeps a slow tracker, wait until the fast tracker reach to some different value and move to the next tracker to replace with the fast tracker
#### Q 15 3 Sums
My solution
```java
public class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        Map<Integer, Integer> map = HashMap<>();
        List<List<Integer>> solution = new List<List<Integer>>();
        for (Integer i : nums)
        {
            if(map.containsKey(i))
            {
                map.put(i,map.get(i) +1);
            }
            else
            {
                map.put(i, 1);
            }
        }
        int[] nums_list = new int[]{map.keySet()};
        for (Integer i : nums_list)
        {
            for(int counter = map.get(i); counter > 0; counter--)
            {
                List<Integer> temp = new List<>();
                if (counter == 3)
                {
                    if (i * counter == 0)
                    {
                        temp.add(i);
                        temp.add(i);
                        temp.add(i);
                        solution.add(temp);
                    }
                }
                if (counter == 2)
                {
                    if(map.containsKey(0 - i * counter))
                    {
                        temp.add(map.get(0 - i * counter));
                        temp.add(i);
                        temp.add(i);
                    }
                }
                if (counter == 1)
                {
                    int result = i;
                    
                }
            }
            map.remove(i);
        }
    }
}
```
For the official Soltion
##### Algorithm
- Sort the array First
- Let i be the slowest tracker (however to avoid duplication)
- j to be the smaller tracker and k to be bigger tracker
- i <= j <= k
- use < > = to let them get close to the target
- O(n^2)
```java
public class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        List<List<Integer>> result = new LinkedList<>();
        int target = 0;
        if(nums.length < 3)
        {
            return result;
        }
        Arrays.sort(nums);
        // O(nlogn) already
        boolean i_add = false;
        for(int i = 0; i < nums.length -1; i++)
        {
            int k = nums.length - 1;
            int j = (i_add) ? i-1 : i+1;
            i_add = false;
            if(nums[i] != nums[i+1])
            {
                while (j < k)
                {
                    int sum = nums[i] + nums[j] + nums[k];
                    if (sum < target && j < k)
                    {
                        j++;
                    }
                    if (sum > target && j < k)
                    {
                        k--;
                    }
                    if (sum == target && j < k)
                    {
                        List<Integer> temp = new LinkedList<>();
                        temp.add(nums[i]);
                        temp.add(nums[j]);
                        temp.add(nums[k]);
                        result.add(temp);
                        j++;
                        k--;
                    }
                }
            }
            else
            {
                i_add = true;
            }
        }
        return result;
    }
}
```
##### My solution
- Sort the array into {i : nums_of_i} Hash table (Expected O(n))
- Create a nSum DP
- For nSum DP, set the target as looping value i->n, reduce the num_of_i in the next nSum function (Dimension Reduced)
- until reach to the dimension of two sum, loop through all variables (Worst case possible n), do Set_contains Function (O(n))
- Send result to the array
- Get the returned result and add i in each result
- If there is n sum (Complexity O(n^(n-1)))

### Question 31 Next Permutation:
numbers such as: 
123 could be rearranged like 132 (which is the next bigger number)
if the case could not be maintained keep the minimum (i.e: asending order)
