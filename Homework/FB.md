## Facebook Prep

### UnionFind
```java
class UnionFind{
    int father[] = null;
    private int count;
    public UnionFind(int n){
        father = new int[n];
        for(int i=0;i < n; i++) father[i] = i;
    }
    private int find(int x){
        if(father[x] == x){
            return x;
        }
        else{
            father[x] = find(father[x]);
            return father[x];
        }
    }
    // set the total amount of valid points
    // Target of Number of islands
    public void setCount(int total){
        this.count = total;
    }
    public int query(){
        return this.count;
    }
    public void union(int a, int b){
        int root_a = find(a);
        int root_b = find(b);
        if(root_a != root_b){
            father[root_a] = root_b;
            count--;
        }
    }
    
}
```

### 283 Move Zero
```java
public class Solution {
    public void moveZeroes(int[] nums) {
        // Dont care the 0s, just push 1 in the front
        int i = 0; // slow tracker, cares non zeros point
        int j = i; // fast tracer, find all non zeros
        int zeros = 0;
        while(j < nums.length)
        {
            if( nums[j] != 0)
            {
                nums[i] = nums[j];
                i++;
            }
            j++;
        }
        for(; i < nums.length; i++) nums[i] = 0;
    }
```
### 301 Remove Invalid Parentheses
```java
class Solution {
    public List<String> removeInvalidParentheses(String s) {
        List<String> submit = new LinkedList<>();
        int depth = checkRemoval(s.toCharArray());
        if(depth == 0){
            submit.add(s);
            return submit;
        }
        if(depth == s.length()){
            submit.add("");
            return submit;
        } 
        dfs(submit, new HashSet<>(), s, depth);
        return submit;
    }
    
    public void dfs(List<String> submit, Set<String> checker, String result, int depth){
        if(depth == 0){
            if(checkValid(result.toCharArray())){
                submit.add(result);
            }
            return;
        }
        
        for(int i =0; i < result.length(); i++){
            if(result.charAt(i) == '(' || result.charAt(i) == ')'){
                String next = result.substring(0, i) + result.substring(i+1);
                if(!checker.contains(next)) dfs(submit, checker, next, depth -1);
                checker.add(next);
            }
        }
        return;
    }
    
    public boolean checkValid(char[] temp){
        int sum = 0;
        for(int i =0; i < temp.length; i++){
            if(temp[i] == '('){
                sum += 1;
            }
            else if(temp[i] == ')'){
                sum -= 1;
            }
            if(sum < 0) return false;
        }
        return sum == 0;
    }
    
    public int checkRemoval(char[] temp){
        int sum = 0;
        int removal = 0;
        for(int i =0; i < temp.length; i++){
            if(temp[i] == '('){
                sum += 1;
            }
            else if(temp[i] == ')'){
                sum -= 1;
            }
            if(sum < 0){
                removal += 1;
                sum = 0;
            }
        }
        return removal + sum;
    }
}
```
### 273 Integer to English Words
```java
class Solution {
    private final String[] belowTen = new String[] {"", "One", "Two", "Three", "Four", "Five", "Six", "Seven", "Eight", "Nine"};
    private final String[] belowTwenty = new String[] {"Ten", "Eleven", "Twelve", "Thirteen", "Fourteen", "Fifteen", "Sixteen", "Seventeen", "Eighteen", "Nineteen"};
    private final String[] belowHundred = new String[] {"", "Ten", "Twenty", "Thirty", "Forty", "Fifty", "Sixty", "Seventy", "Eighty", "Ninety"};
    
    public String numberToWords(int num) {
        if (num == 0) return "Zero";
        return helper(num); 
    }
    
    private String helper(int num) {
        String result = new String();
        if (num < 10) result = belowTen[num];
        else if (num < 20) result = belowTwenty[num -10];
        else if (num < 100) result = belowHundred[num/10] + " " + helper(num % 10);
        else if (num < 1000) result = helper(num/100) + " Hundred " +  helper(num % 100);
        else if (num < 1000000) result = helper(num/1000) + " Thousand " +  helper(num % 1000);
        else if (num < 1000000000) result = helper(num/1000000) + " Million " +  helper(num % 1000000);
        else result = helper(num/1000000000) + " Billion " + helper(num % 1000000000);
        return result.trim();
    }
}
```
### 325. Maximum Size Subarray Sum Equals k
```java
public class Solution {
    public int maxSubArrayLen(int[] nums, int k) {
       HashMap<Integer, Integer> map = new HashMap<>();
        int sum = 0, max =0;
        map.put(0,-1);
        for(int i =0; i < nums.length; i++){
            sum += nums[i];
            if(map.containsKey(sum-k)) max = Math.max(i-map.get(sum-k),max);
            if(!map.containsKey(sum)) map.put(sum, i);
        }
        return max;
    }
}
```
### 67. Add Binary
```java
public class Solution {
    public String addBinary(String a, String b) {
        StringBuilder sb = new StringBuilder();
        int i = a.length() - 1, j = b.length() -1, carry = 0;
        while (i >= 0 || j >= 0) {
            int sum = carry;
            if (j >= 0) sum += b.charAt(j--) - '0';
            if (i >= 0) sum += a.charAt(i--) - '0';
            sb.append(sum % 2);
            carry = sum / 2;
        }
        if (carry != 0) sb.append(carry);
        return sb.reverse().toString();
    }
}
```
### 621. Task Scheduler
```java
class Solution {
    public int leastInterval(char[] tasks, int n) {
        // Create a priority queue to store the number of tasks, ordered by the number
        if(n == 0) return tasks.length;
        if(tasks.length == 0) return 0;
        int[] curr = new int[26];
        for (char ele : tasks){
            curr[ele - 'A']++;
        }
        PriorityQueue<Integer> que = new PriorityQueue<>(1, Collections.reverseOrder());
        for (int each : curr){
            if(each > 0) que.add(each);
        }
        int submit = 0;
        while(!que.isEmpty()){
            int remainder = n + 1;
            List<Integer> added = new ArrayList<>();
            // Deal with each interval
            while(remainder > 0 && !que.isEmpty()){
                int top = que.poll();
                added.add(top - 1);
                remainder--;
                submit++;
            }
            for (int each : added){
                if(each > 0) que.add(each);
            }
            if(!que.isEmpty()) submit += remainder;
        }
        return submit;
    }
```
### 253. Meeting Roomw II
```java
class Solution {
    public static Comparator<Integer[]> IntervalComparator = new Comparator<Integer[]>(){
        //@override
        public int compare(Integer[] e1, Integer[] e2){
            if(e1[0] - e2[0] != 0){
                return e1[0] - e2[0];
            }
            else{
                return e1[1] - e2[1];
            }
        }
    };
    
    public int minMeetingRooms(Interval[] intervals) {
        // Priority Queue
        if(intervals == null || intervals.length == 0) return 0;
        Queue<Integer[]> queue = new PriorityQueue<>(IntervalComparator);
        for(Interval item : intervals){
            queue.add(new Integer[]{item.start , 1});
            queue.add(new Integer[]{item.end , 0});
        }
        int curroom = 0;
        int maxroom = 0;
        while(!queue.isEmpty()){
            int temp = queue.poll()[1];
            curroom += temp == 1 ? 1 : -1;
            maxroom = Math.max(maxroom, curroom);
        }
        return maxroom;
    }
}
```
### 311.Sparse Matrix Multiplication
```java
public class Solution {
    public int[][] multiply(int[][] A, int[][] B) {
        int Ar = A.length, Ac = A[0].length, Bc = B[0].length;
        int[][] C = new int[Ar][Bc];
        // For each C row
        for(int i = 0; i < Ar; i++){
            // Start Multiplication
            for(int k = 0; k < Ac; k++){
                if(A[i][k] == 0) continue;
                // For each C column
                for(int j = 0; j < Bc; j++){
                    if(B[k][j] != 0) C[i][j] += A[i][k] * B[k][j]; 
                }
            }
        }
        return C;
    }
}
```
### 17. Letter Combination of a Phone Number
```java
public class Solution {
    public List<String> letterCombinations(String digits) {
        if (digits.length() == 0) return new LinkedList<>();
        String[] a_to_z = {"abc","def","ghi","jkl","mno","pqrs","tuv","wxyz"};
        List<String> list1 = new LinkedList<>();
        list1.add("");
        while(!digits.equals(""))
        {
            int number = Integer.parseInt(digits.substring(0,1));
            digits = digits.substring(1);
            List<String> updated_list = new LinkedList<>();
            while(list1.size() != 0)
            {
                String prev_c = list1.get(0);
                String contact = a_to_z[number-2];
                updated_list.add(prev_c + contact.substring(0,1));
                updated_list.add(prev_c + contact.substring(1,2));
                updated_list.add(prev_c + contact.substring(2,3));
                if(contact.length() == 4) updated_list.add(prev_c + contact.substring(3,4));
                list1.remove(0);
            }
            list1 = updated_list;
        }
        return list1;
    }
}
```
### 91.Decode Ways
```java
class Solution {
    public int numDecodings(String s) {
        if(s.length() == 0 || s.charAt(0) == '0') return 0;
        if(s.length() == 1) return 1;
        int[] dp = new int[s.length()+1];
        dp[0] = 1;
        dp[1] = 1;
        for(int i = 2; i < s.length()+1; i++){
            int num = Integer.valueOf(s.substring(i-2, i));
            // case 101, 110, 100, 10
            if(num <= 26 && num >= 10 && s.charAt(i-1) != '0'){
                dp[i] = dp[i-2] + dp[i-1];
            }
            else if(s.charAt(i-1) == '0'){
                if(num > 26 || num < 10) return 0;
                dp[i] = dp[i-2];
            }
            else{
                dp[i] = dp[i-1];
            }
        }
        return dp[s.length()];  
    }
}
```
### 297. Serialize and Deserialize Binary Tree
```java
public class Codec {

    // Encodes a tree to a single string.
    public String serialize(TreeNode root) {
        //BFS Solution to serialize
        String result = "";
        if(root == null) return result;
        StringBuilder sb = new StringBuilder();
        Queue<TreeNode> que = new LinkedList<>();
        que.add(root);
        sb.append(root.val);
        while(!que.isEmpty()){
            TreeNode temp = que.poll();
            // result += " "+String.valueOf(temp.val);
            if(temp.left == null){
                sb.append(" n");
            }
            else{
                sb.append(" " + temp.left.val);
                que.add(temp.left);
            }
            if(temp.right == null){
                sb.append(" n");
            }
            else{
                sb.append(" " + temp.right.val);
                que.add(temp.right);
            }
        }
        return sb.toString();
    }

    // Decodes your encoded data to tree.
    public TreeNode deserialize(String data) {
        // Don't add null node in the queue
        String[] splited = data.split(" ");
        if(splited.length == 0 || splited[0].length() == 0) return null;
        TreeNode root = new TreeNode(Integer.parseInt(splited[0]));
        Queue<TreeNode> que = new LinkedList<>();
        que.add(root);
        int idx = 1;
        for (int i = 1; !que.isEmpty(); i += 2){
            TreeNode temp = que.poll();
            if(!splited[i].equals("n")){
                temp.left = new TreeNode(Integer.parseInt(splited[i]));
                que.add(temp.left);
            }
            if(!splited[i+1].equals("n")){
                temp.right = new TreeNode(Integer.parseInt(splited[i+1]));
                que.add(temp.right);
            }  
        }
        return root;
    }
}
```
### 314. Binary Tree Vertical Order Traversal
```java
class Solution {
    public List<List<Integer>> verticalOrder(TreeNode root) {
        if(root == null) return new LinkedList<>();
        List<List<Integer>>submit = new LinkedList<>();
        Map<Integer, List<Integer>> map = new HashMap<>();
        bfs(root, map);
        submit= map.entrySet().stream()
            .sorted(Map.Entry.comparingByKey())
            .map(each -> each.getValue())
            .collect(Collectors.toList());
        return submit;
    }
    
    public void bfs(TreeNode root, Map<Integer,List<Integer>> map){
        if(root == null) return;
        // Adding val, column in each row...
        Queue<TreeNode> que = new LinkedList<>();
        Map<TreeNode, Integer> nodemap = new HashMap<>();
        que.add(root);
        que.add(null);
        nodemap.put(root, 0);
        while(!que.isEmpty()){
            TreeNode temp = que.poll();
            if(map.containsKey(nodemap.get(temp))){
                map.get(nodemap.get(temp)).add(temp.val);
            }
            else{
                List<Integer> templist = new LinkedList<>();
                templist.add(temp.val);
                map.put(nodemap.get(temp), templist);
            }
            if(temp.left != null){
                que.add(temp.left);
                nodemap.put(temp.left, nodemap.get(temp) - 1);
            }
            if(temp.right != null){
                que.add(temp.right);
                nodemap.put(temp.right, nodemap.get(temp) + 1);
            }
            if(que.peek() == null){
                que.poll();
                if(que.size() != 0) que.add(null);
            }
        }
    }
}
```
### 44. Wildcard Matching
```java
class Solution {
    
    public boolean isMatch(String s, String p) {
        boolean[][] match = new boolean[s.length()+1][p.length()+1];
        match[s.length()][p.length()] = true;
        // Find * 
        for(int i = p.length() -1; i >= 0; i--){
            if(p.charAt(i) != '*'){
                break;
            }
            else{
                // "" -> *
                match[s.length()][i] = true;
            }
        }
        for(int i = s.length() -1; i >= 0; i--){
            for(int j = p.length() - 1; j >= 0; j--){
                if(s.charAt(i) == p.charAt(j) || p.charAt(j) == '?'){
                    // Move to the previous step
                    match[i][j] = match[i+1][j+1];
                }
                else if(p.charAt(j) == '*'){
                    //before star or before the string
                    match[i][j] = match[i+1][j] || match[i][j+1];
                }
                else{
                    match[i][j] = false;
                }
            }
        }
        return match[0][0];
    }
}
```
### 10.Regular Expression Matching
```java
public class Solution {
    public boolean isMatch(String s, String p) {
        if(s.equals(p)) return true;
        
        if(s.length() == 0){
            // a*b*....
            if(p.length() ==1) return false;
            if(p.charAt(1) == '*') return isMatch(s, p.substring(2));
            return false;
        }
        if(p.length() == 0) return false;
        if(p.charAt(0) == '.' || s.charAt(0) == p.charAt(0)){
            if(p.length() > 1 && p.charAt(1) == '*') {
                return (isMatch(s.substring(1),p) || isMatch(s,p.substring(2)));
            }
            else{
                return isMatch(s.substring(1),p.substring(1));
            }
        }
        else{
            if(p.length() > 1 && p.charAt(1) == '*') {
                return isMatch(s,p.substring(2));
            }
            else{
                return false;
            }
        }
    }
}
```
### 15.3 Sum
```java
public List<List<Integer>> threeSum(int[] nums) {
    List<List<Integer>> res = new ArrayList<>();
    Arrays.sort(nums);
    for (int i = 0; i + 2 < nums.length; i++) {
        if (i > 0 && nums[i] == nums[i - 1]) {              // skip same result
            continue;
        }
        int j = i + 1, k = nums.length - 1;  
        int target = -nums[i];
        while (j < k) {
            if (nums[j] + nums[k] == target) {
                res.add(Arrays.asList(nums[i], nums[j], nums[k]));
                j++;
                k--;
                while (j < k && nums[j] == nums[j - 1]) j++;  // skip same result
                while (j < k && nums[k] == nums[k + 1]) k--;  // skip same result
            } else if (nums[j] + nums[k] > target) {
                k--;
            } else {
                j++;
            }
        }
    }
    return res;
}
```
### 158. Read N Characters Given Read4 II - Call multiple times
```java
private int buffPtr = 0;
private int buffCnt = 0;
private char[] buff = new char[4];

public int read(char[] buf, int n) {
    int ptr = 0;
    while (ptr < n) {
        // check if the previous is finished?
        if (buffPtr == 0) {
            buffCnt = read4(buff);
        }
        if (buffCnt == 0) break;
        while (ptr < n && buffPtr < buffCnt) {
            buf[ptr++] = buff[buffPtr++];
        }
        //several calls may interrupt the while loop
        if (buffPtr >= buffCnt) buffPtr = 0;
    }
    return ptr;
}
```
### 278.First Bad Version
```java
public class Solution extends VersionControl {
    public int firstBadVersion(int n) {
        int front = 1, back = n;
        if(front == back) return front;
        while(back > front){
            // Sum will cause an overflow
            int middle = (int) (front + 0.5 * (back-front));
            if(middle == front){
                return isBadVersion(front) ? front : back;
            }
            if(isBadVersion(middle)){
                back = middle;
            }
            else{
                front = middle;
            }
        }
        return back;
    }
}
```
### 200.Number of Islands
```java
public class Solution {

private int n;
private int m;

public int numIslands(char[][] grid) {
    int count = 0;
    n = grid.length;
    if (n == 0) return 0;
    m = grid[0].length;
    for (int i = 0; i < n; i++){
        for (int j = 0; j < m; j++)
            if (grid[i][j] == '1') {
                DFSMarking(grid, i, j);
                ++count;
            }
    }    
    return count;
}

private void DFSMarking(char[][] grid, int i, int j) {
    if (i < 0 || j < 0 || i >= n || j >= m || grid[i][j] != '1') return;
    grid[i][j] = '0';
    DFSMarking(grid, i + 1, j);
    DFSMarking(grid, i - 1, j);
    DFSMarking(grid, i, j + 1);
    DFSMarking(grid, i, j - 1);
}
```
### 277.Find the Celebrity
```java
public class Solution extends Relation {
    public int findCelebrity(int n) {
        int celebrity = 0;
        for(int i = 1; i < n; i++){
            if(!knows(i, celebrity)){
                celebrity = i;
            }
        }
        for(int i = 0; i < n; i++){
            if(celebrity != i){
                if(knows(celebrity, i) || !knows(i, celebrity)) return -1;
            }
        }
        return celebrity;
    }
}
```
### 76. Minimum Window Substring
```java
class Solution {
    public String minWindow(String s, String t) {
        // If search pool is empty, then let the pointer to find the nearest match number
        // A keep storage pool, a requirement pool
        if(t.length() > s.length()) return "";
        Map<Character, Integer> map = new HashMap<>();
        Set<Character> set = new HashSet<>();
        for (char each : t.toCharArray()){
            if (map.containsKey(each)) {
                map.put(each, map.get(each) +1);
            }else{
                map.put(each, 1);
                set.add(each);
            }
        }
        // Two pointer
        int front = 0;
        int back = -1;
        int min = Integer.MAX_VALUE;
        char temp;
        int[] idx = new int[]{0,0};
        // Storing the index
        Queue<Integer> que = new LinkedList<>();
        while(front < s.length()){
            temp = s.charAt(front);
            if(map.containsKey(temp)){
                que.add(front);
                if (map.get(temp) - 1 <= 0 && set.contains(temp)) set.remove(temp);
                map.put(temp, map.get(temp) - 1);
            }
            
            while(set.isEmpty()){
                if(back == -1) back = que.poll();
                temp = s.charAt(back);
                if(map.get(temp) + 1 > 0){
                    set.add(temp);
                    if(front - back < min){
                        min = front - back;
                        idx[0] = back;
                        idx[1] = front;
                    }
                }
                map.put(temp, map.get(temp) + 1);
                if(!que.isEmpty()) back = que.poll();
            }
            front++;
        }
        if(min == Integer.MAX_VALUE) return "";
        return s.substring(idx[0], idx[1] +1);
    }
}
```
### 257.Binary Tree Paths
```java
public List<String> binaryTreePaths(TreeNode root) {
    List<String> answer = new ArrayList<String>();
    if (root != null) searchBT(root, "", answer);
    return answer;
}
// Maintain a path and collect once reach to the end node
private void searchBT(TreeNode root, String path, List<String> answer) {
    if (root.left == null && root.right == null) answer.add(path + root.val);
    if (root.left != null) searchBT(root.left, path + root.val + "->", answer);
    if (root.right != null) searchBT(root.right, path + root.val + "->", answer);
}
```
### 282.Expression Add Operation
```java
class Solution {
    public List<String> addOperators(String num, int target) {
        // four option, add, minus, multiply, do nothing...
        List<String> submit = new ArrayList<>();
        if(num.length() == 0){
            return submit;
        }
        submit = dfs(num, target, new long[]{0, 0}, 1, 1);
        return submit;
    }
    
    public List<String> dfs(String num, long target, long[] multi, int remainpos, long minus){
        List<String> submit = new LinkedList<>();
        long presnum = 0;
        if(multi[0] == 1){
            presnum = multi[1] * Long.valueOf(num.substring(0,remainpos)) * minus;
        }
        else{
            presnum = Long.valueOf(num.substring(0,remainpos)) * minus;
        }
        //System.out.println(presnum);
        if(num.substring(remainpos).length() == 0){
            if(presnum == target){
                submit.add(num.substring(0,remainpos));
                //System.out.println(target);
            }
            return submit;
        }
        
        List<String> result;
        //+
        result = dfs(num.substring(remainpos), target - presnum, new long[]{0, 0}, 1, 1);
        for(String each : result){
            submit.add(num.substring(0,remainpos)+ "+" +each);
        }
        //-
        result = dfs(num.substring(remainpos), target - presnum, new long[]{0, 0}, 1, -1);
        for(String each : result){
            submit.add(num.substring(0,remainpos)+ "-" +each);
        }
        //*
        result = dfs(num.substring(remainpos), target, new long[]{1, presnum}, 1, 1);
        for(String each : result){
            submit.add(num.substring(0,remainpos)+ "*" +each);
        }
        if(!num.substring(0, remainpos).equals("0")) submit.addAll(dfs(num, target, multi, remainpos + 1, minus));

        return submit;  
    }
}
```
602. Friend Request II: Who has the most Friends
```sql
select id, sum(num) as num from
(
select accepter_id as id, count(*) as num from request_accepted group by id
union all
select requester_id as id, count(*) as num from request_accepted group by id
) as ri
group by id
order by sum(num) desc
limit 1
```

