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
    int j = 0;
    for(int i = 0; i < nums.length; i++) {
        if(nums[i] != 0) {
            int temp = nums[j];
            nums[j] = nums[i];
            nums[i] = temp;
            j++;
        }
    }
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
### 157. Read N Characters Given Read4
```java
public class Solution extends Reader4 {
    public int read(char[] buf, int n) {
        int readed = 0;
        char[] slot = new char[4];
        boolean eof = false;
        while(!eof && readed < n){
            int temp= read4(slot);
            if(temp < 4) eof = true;
            for(int i = 0; i < temp && readed < n; i++){
                buf[readed] = slot[i];
                readed++;
            }
        }
        return readed;
    }
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
### 602. Friend Request II: Who has the most Friends
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
### 23. Merge k Sorted Lists
```java
public class Solution {
    // Solution: helper: Merge two linkedlist first
    // 2 ways, merge sort (Accepted) or manually merge then one by one (Limit time exceeeded)
    public ListNode mergeKLists(ListNode[] lists) {
        if(lists.length == 0) return null;
        if(lists.length == 1) return lists[0];
        if(lists.length == 2) return merge2Lists(lists[0],lists[1]);
        ListNode[] list1 = new ListNode[lists.length/2];
        ListNode[] list2 = new ListNode[lists.length/2 + lists.length %2];
        for(int i = 0; i< lists.length/2; i++) list1[i] =lists[i];
        for(int j = lists.length/2; j <lists.length;j++) list2[j-lists.length/2] = lists[j];
        return merge2Lists(mergeKLists(list1),mergeKLists(list2));
    }
    
    public ListNode merge2Lists(ListNode n1, ListNode n2)
    {
        if(n1== null) return n2;
        if(n2== null) return n1;
        if(n1.val >= n2.val)
        {
            n2.next = merge2Lists(n1, n2.next);
            return n2;
        }
        else
        {
            n1.next = merge2Lists(n1.next, n2);
            return n1;
        }
    }
}
```
### 121.Best Time to Buy and Sell Stock
```java
public class Solution {
    public int maxProfit(int[] prices) {
        // loop every element, always let the next value minus the smallest value. Keep the local maximum
        if (prices.length == 0) return 0;
        int min = prices[0];
        int max = 0;
        for(int i = 0; i < prices.length; i++)
        {
            min = Math.min(prices[i], min);
            max = Math.max(prices[i] - min, max);
        }
        return max;
    }
}
```
### 121.Best Time to Buy and Sell Stock (Maximum Profit)
```java
public class Solution {
    public int maxProfit(int[] prices) {
        // keep the profit, do transaction when earning
        if(prices.length <= 1) return 0;
        int slow = 0;
        int fast = 1;
        int profit = 0;
        int submit = 0;
        while(fast < prices.length)
        {
            if((prices[fast] - prices[slow]) > profit)
            {
                profit =  prices[fast] -prices[slow];
                fast++;
            }
            else
            {
                submit += profit;
                profit = 0;
                slow = fast;
                fast++;
            }
        }
        return submit + profit;
    }
}
```
### 161.One Edit Distance
```java
class Solution {
    public boolean isOneEditDistance(String s, String t) {
        // Case 1 not same length
        // Case 2 Same length
        int dist = Math.abs(s.length() - t.length());
        if(dist > 1) return false;
        if(dist == 1 && s.length() <= 1 && t.length()<= 1) return true;
        if(s.length() == t.length()){
            if(s.equals(t)) return false;
            for(int i = 0; i < s.length(); i++){
                // case substitution
                if(s.charAt(i) != t.charAt(i)){
                    String sc = s.substring(0, i) + s.substring(i+1);
                    String tc = t.substring(0, i) + t.substring(i+1);
                    if(sc.equals(tc)) return true;
                    return false;
                }
            }
            return false;
        }
        else{
            String ls, ss;
            if(s.length() > t.length()){
                ls = s;
                ss = t;
            }
            else{
                ls = t;
                ss = s;
            }
            for(int i =0; i < ss.length(); i++){
                // case addition
                if(ls.charAt(i) != ss.charAt(i)){
                    String cs = ls.substring(0, i) + ls.substring(i+1);
                    if(cs.equals(ss)) return true;
                }
            }
            if(ss.equals(ls.substring(0, ss.length()))) return true;
        }
        return false;
    }
}
```
### 56.Merge Intervals
```java
public List<Interval> merge(List<Interval> intervals) {
    if (intervals.size() <= 1)
        return intervals;
    
    // Sort by ascending starting point using an anonymous Comparator
    intervals.sort((i1, i2) -> Integer.compare(i1.start, i2.start));
    
    List<Interval> result = new LinkedList<Interval>();
    int start = intervals.get(0).start;
    int end = intervals.get(0).end;
    
    for (Interval interval : intervals) {
        if (interval.start <= end) // Overlapping intervals, move the end if needed
            end = Math.max(end, interval.end);
        else {                     // Disjoint intervals, add the previous one and reset bounds
            result.add(new Interval(start, end));
            start = interval.start;
            end = interval.end;
        }
    }
    
    // Add the last interval
    result.add(new Interval(start, end));
    return result;
}
```
### 173. Binary Search Tree Iterator
```java
public class BSTIterator {
    Stack<TreeNode> s;
    public BSTIterator(TreeNode root) {
        s = new Stack<>();
        dfspusher(root);
    }
    private void dfspusher(TreeNode node){
        if(node != null){
            s.push(node);
            while(node.left!= null){
                s.push(node.left);
                node = node.left;
            } 
        }
    }
    /** @return whether we have a next smallest number */
    public boolean hasNext() {
        return !s.isEmpty();
    }
    /** @return the next smallest number */
    public int next() {
        TreeNode temp = s.pop();
        dfspusher(temp.right);
        return temp.val;
    }
}
```
### 211. Add and Search Word - Data structure design
```java
class TrieNode{
    TrieNode child[];
    boolean hasWord;
    public TrieNode(){
        child = new TrieNode[26];
        hasWord = false;
    }
}
public class WordDictionary {
    private TrieNode root = new TrieNode();
    // Adds a word into the data structure.
    public void addWord(String word) {
        // Write your code here
        TrieNode curr = root;
        for(int i = 0; i < word.length(); i++){
            
            if(curr.child[word.charAt(i) - 'a']==null){
                curr.child[word.charAt(i) - 'a'] = new TrieNode();
            }
            curr = curr.child[word.charAt(i) - 'a'];
        }
        curr.hasWord = true;
    }

    // Returns if the word is in the data structure. A word could
    // contain the dot character '.' to represent any one letter.
    public boolean search(String word) {
        // Write your code here
        return deepsearch(root, word);
    }
    
    private boolean deepsearch(TrieNode curr, String word){
        if(word.length() == 0) return curr.hasWord;
            if(word.charAt(0) != '.'){
                if(curr.child[word.charAt(0) - 'a']==null){
                    return false;
                }
                else{
                    return deepsearch(curr.child[word.charAt(0) - 'a'], new String(word.substring(1)));
                }
            }
            else{
                for(TrieNode node : curr.child){
                    if(node != null){
                        if(deepsearch(node, new String(word.substring(1)))) return true;
                    }
                }
                return false;
            }
    }
}
```
###  341. Flatten Nested List Iterator
```java
public class NestedIterator implements Iterator<Integer> {
    Queue<Integer> que;
    public NestedIterator(List<NestedInteger> nestedList) {
        que = new LinkedList<>();
        extracter(nestedList);
    }
    private void extracter(List<NestedInteger> list){
        for(int i = 0; i < list.size(); i++){
            if(list.get(i).isInteger()){
                que.add(list.get(i).getInteger());
            }
            else{
                extracter(list.get(i).getList());
            }
        }
    }
    @Override
    public Integer next() {
        return que.poll();
    }
    @Override
    public boolean hasNext() {
        // System.out.println(que.isEmpty());
        return !que.isEmpty();
    }
}
```
### 543. Diameter of Binary Tree
```java
class Solution {
    int max = 0;
    public int diameterOfBinaryTree(TreeNode root) {
        if(root == null) return 0;
        longest(root);
        return max;
    }
    
    private int longest(TreeNode root){
        if(root == null) return 0;
        int leftlong = longest(root.left);
        int rightlong = longest(root.right);
        if(root.left != null) leftlong += 1;
        if(root.right != null) rightlong += 1;
        max = Math.max(max, leftlong +rightlong);
        return Math.max(leftlong, rightlong);
    }
}
```
### 636. Exclusive Time of Functions
```java
class Solution {
    public int[] exclusiveTime(int n, List<String> logs) {
        int[] time = new int[n];
        int prevtime = 0;
        Stack<Integer> stack = new Stack<>();
        for(int i =0; i< logs.size(); i++){
            String[] temp = logs.get(i).split(":");
            // Default condition
            if(!stack.isEmpty()) time[stack.peek()] += Integer.parseInt(temp[2]) - prevtime;
            prevtime = Integer.parseInt(temp[2]);
            if(temp[1].equals("start")){
                // only push start in
                stack.push(Integer.parseInt(temp[0]));
            }
            else{
                // Start to End case +1
                time[stack.pop()]++;
                prevtime++;
            }
        }
        return time;
    }
}
```
### 139.Word Break
```java
public class Solution {
    public boolean wordBreak(String s, List<String> wordDict) {
        // loop i from 0 to end
        // loop j from i-1 to 0
        // like "leetcode" if i at "c" point will loop from "c", "tc", "etc" to check the case
        if(wordDict.size() == 0) return false;
        if(wordDict.contains(s)) return true;
        boolean[] dp = new boolean[s.length()+1];
        Arrays.fill(dp, false);
        dp[0] = true;
        for(int i = 1; i <= s.length();i++)
        {
            for(int j = i-1; j>= 0; j--)
            {
                if(dp[j])
                {
                    if(wordDict.contains(s.substring(j,i)))
                    {
                        dp[i] = true;
                        break;
                    }
                }
            }
        }
        return dp[s.length()];
    }
```
### 252. Meeting Rooms
```java
class Solution {
    public boolean canAttendMeetings(Interval[] intervals) {
        if(intervals.length < 2) return true;
        List<Integer[]> result = new ArrayList<>();
        result = Arrays.stream(intervals)
            .sorted((o1, o2) -> o1.start - o2.start)
            .map(each -> new Integer[]{each.start, each.end})
            .collect(Collectors.toList());
        for(int i =1; i < result.size(); i++) if(result.get(i)[0] < result.get(i-1)[1]) return false;
        return true;
    }
}
```
### 285. Inorder Successor in BST
```java
public class Solution {
    public TreeNode inorderSuccessor(TreeNode root, TreeNode p) {
        // Key: Find the smallest value that larger than p in the tree.
        // It doesn't matter whether p is in the Tree
        // Case p is on the right hand side of root
        while(root!= null && root.val <= p.val) root = root.right;
        if(root == null) return root;
        //Case p is in the left hand side of root
        TreeNode left = inorderSuccessor(root.left,p);
        return (left != null && left.val > p.val) ? left : root;
    }
}
```
### 43. Multiply Strings
```java
class Solution {
    public String multiply(String num1, String num2) {
        int[] result = new int[num1.length() + num2.length()];
        for(int i = num1.length() - 1; i >= 0; i--){
            if(num1.charAt(i) == '0') continue;
            int num1temp = num1.charAt(i) - '0';
            for(int j = num2.length() -1 ; j >= 0; j--){
                if(num2.charAt(j) == '0') continue;
                int num2temp = num2.charAt(j) - '0';
                result[i+j+1] += num1temp * num2temp;
            }
        }
        int carry = 0;
        String submit = "";
        // Do carry and Sum up here
        for(int i = result.length - 1; i >= 0; i--){
            int temp = result[i] + carry;
            carry = temp / 10;
            submit  = String.valueOf(temp % 10) + submit;
        }
        
        for(int i = 0; i < submit.length(); i++){
            if(submit.charAt(i) != '0') return submit.substring(i);
        }
        return "0";
    }
}
```
### 98. Validate Binary Search Tree
```java
public boolean isValidBST(TreeNode root) {
   if (root == null) return true;
   Stack<TreeNode> stack = new Stack<>();
   TreeNode pre = null;
   while (root != null || !stack.isEmpty()) {
      while (root != null) {
         stack.push(root);
         root = root.left;
      }
      root = stack.pop();
      if(pre != null && root.val <= pre.val) return false;
      pre = root;
      root = root.right;
   }
   return true;
}
```
### 78.Subsets
```java
public class Solution {
    public List<List<Integer>> subsets(int[] nums) {
        List<List<Integer>> list = new ArrayList<>();
        Arrays.sort(nums);
        backtrack(list, new ArrayList<>(), nums, 0);
        return list;
    }
    private void backtrack(List<List<Integer>> list, List<Integer> temp, int[] nums, int loc){
        list.add(new ArrayList<>(temp));
        for(int i = loc; i < nums.length; i++){
            temp.add(nums[i]);
            backtrack(list, temp, nums, i+1);
            temp.remove(temp.size()-1);
        }
    }
}
```
### 75.Sort Colors
```java
public class Solution {
    public void sortColors(int[] nums) {
        if(nums.length <= 1) return;
        int red = 0;
        int white = 0;
        int blue = 0;
        while(blue < nums.length){
            if(nums[blue] == 0){
                swap(nums, blue, white);
                swap(nums, white, red);
                blue++;
                red++;
                white++;
            }else if(nums[blue] == 1){
                swap(nums, blue, white);
                blue++;
                white++;
            }else{
                blue++;
            }
        }
    }
    
    private void swap(int[] nums, int inxa, int inxb){
        int temp = nums[inxa];
        nums[inxa] = nums[inxb];
        nums[inxb] = temp;
    }
}
```
### 57. Insert Interval
```java
class Solution {
    public List<Interval> insert(List<Interval> intervals, Interval newInterval) {
        List<Interval>result = new LinkedList<>();
        int i = 0;
        // Get all prior
        while(i < intervals.size() && intervals.get(i).end < newInterval.start){
            result.add(intervals.get(i));
            i++;
        }
        // Get all overlapping
        while(i < intervals.size() && intervals.get(i).start <= newInterval.end){
            newInterval = new Interval(Math.min(intervals.get(i).start, newInterval.start),Math.max(intervals.get(i).end, newInterval.end));
            i++;
        }
        result.add(newInterval);
        // Get all post
        while(i < intervals.size()){
            result.add(intervals.get(i));
            i++;
        }
        return result;
    }
}
```
### 33. Search in Rotated Sorted Array
```java
public class Solution {
    public int search(int[] nums, int target) {
        int first = 0;
        int last = nums.length;
        int mid = first + (last - first) /2;
        while(first != last)
        {
            mid = first + (last - first) /2;
            if(nums[mid] == target) return mid;
            if(nums[mid] >= nums[first])
            {
                if(nums[first] <= target && nums[mid] > target)
                {
                    last = mid;
                }
                else
                {
                    first = mid +1;
                }
            }
            else
            {
                if(nums[last -1] >= target && nums[mid] < target)
                {
                    first = mid +1;
                }
                else
                {
                    last = mid;
                }
            }
        }
        return -1;
    }
}
```
### 133.Clone Graph
```java
public class Solution {
    HashMap<UndirectedGraphNode, UndirectedGraphNode> visited = new HashMap<>();
    public UndirectedGraphNode cloneGraph(UndirectedGraphNode node) {
        if(node == null) return null;
        if(visited.containsKey(node)) return visited.get(node);
        UndirectedGraphNode root = new UndirectedGraphNode(node.label);
        visited.put(node, root);
        List<UndirectedGraphNode> neighbors = new ArrayList<>();
        for(UndirectedGraphNode temp : node.neighbors){
            neighbors.add(cloneGraph(temp));
        }
        root.neighbors = neighbors;
        return root;
    }
}
```
### 79.Word Search
```java
public class Solution {
    int height = 0;
    int width = 0;
    public boolean exist(char[][] board, String word) {
        // time O(n^2)
        // two ways, O(1) space to destroy board, O(n) space to store the visited point
        if (board.length == 0) return false;
        if (word.equals("")) return true;
        height = board.length;
        width = board[0].length;
        for (int i = 0; i < height; i++)
        {
            for(int j = 0; j < width; j++)
            if(board[i][j] == word.charAt(0))
            {
                board[i][j]='0';
                boolean result = finder(i, j, word.substring(1),board);
                board[i][j]= word.charAt(0);
                if(result) return true;
            }
        }
        return false;  
    }
    public boolean finder(int i, int j, String word, char[][] visited)
    {
        
        if(word.equals("")) return true;
        char c = word.charAt(0);
        if(i > 0 && visited[i-1][j] == c)
        {
            visited[i-1][j] = '0';
            if (finder(i-1,j,word.substring(1),visited)) return true;
            visited[i-1][j] = c;
        }
        if(i < height-1 && visited[i+1][j] == c)
        {
            visited[i+1][j] = '0';
            if (finder(i+1,j,word.substring(1),visited)) return true;
            visited[i+1][j] = c;
        }
        if(j > 0 && visited[i][j-1] == c)
        {
            visited[i][j-1] = '0';
            if (finder(i,j-1,word.substring(1),visited)) return true;
            visited[i][j-1] = c;
        }
        if(j < width-1 && visited[i][j+1] == c)
        {
            visited[i][j+1] = '0';
            if (finder(i,j+1,word.substring(1),visited)) return true;
            visited[i][j+1] = c;
        }
        return false;
    }
}
```
### Reverse Linkedlist
```java
public class Solution {
    public ListNode reverseList(ListNode head) {
        // Hard point, you cannot modify head until if reach to its next (same as the other Node represent it)
        if(head == null)
        {
            return head;
        }
        ListNode temp = head;
        head = head.next;
        temp.next = null;
        while(head!= null)
        {
            ListNode temp2 = head;
            head = head.next;
            temp2.next = temp;
            temp = temp2;
        }
        return temp;
    }
}
```
### 670.Maximum Swap
```java
=class Solution {
    public int maximumSwap(int num) {
        if(num < 12) return num;
        char[] curr = String.valueOf(num).toCharArray();
        char smaller = curr[0];
        char target = 'a';
        int index = -1;
        for(int i = 1; i < curr.length; i++){
            if(smaller >= curr[i]) {
                smaller = curr[i];
            }
            else{
                target = curr[i];
                index = i;
                break;
            }
        }
        if(target != 'a'){
            for(int i = index; i < curr.length; i++){
                if(curr[i] >= target){
                    target = curr[i];
                    index = i;
                }
            }
            for(int i = 0; i < curr.length; i++){
                if(curr[i] < target){
                    curr[index] = curr[i];
                    curr[i] = target;
                    break;
                }
            }
        }

        return Integer.valueOf(String.valueOf(curr));
    }
}
```
### Lowest Common Ancestor of a Binary Tree
```java
public class Solution {
    TreeNode target = null;
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
      dfs(root, p, q);
      return target;
    }
    
    private int dfs(TreeNode root, TreeNode p, TreeNode q){
        if(target!= null) return 2;
        if(root == null) return 0;
        int result = 0;
        if(root == p || root == q){
            result = 1;
        }
        result += dfs(root.left, p, q);
        result += dfs(root.right, p, q);
        if(result == 2 && target == null) target = root;
        return result;
    }
}
```
### 494.Target Sum
```java
class Solution {
    public int findTargetSumWays(int[] nums, int S) {
        if(nums.length == 0) return 0;
        return findtarget(nums, 0, S);
    }
    
    private int findtarget(int[] nums, int pos, int sum){
        if(pos == nums.length -1) return (sum == -nums[pos] ? 1 : 0) + (sum == nums[pos] ? 1 : 0);
        return findtarget(nums, pos + 1, sum - nums[pos]) + findtarget(nums, pos + 1, sum + nums[pos]);
    }
    
}
```
### 398. Random Pick Index
```java
class Solution {
    Map<Integer, List<Integer>> map;
    Random rand;
    public Solution(int[] nums) {
        map = new HashMap<>();
        rand = new Random();
        for(int i =0; i < nums.length; i++){
            if(map.containsKey(nums[i])){
                map.get(nums[i]).add(i);
            }
            else{
                List<Integer> temp = new LinkedList<>();
                temp.add(i);
                map.put(nums[i], temp);
            }
        }
    }
    
    public int pick(int target) {
        List<Integer> result = map.get(target);
        return result.get(rand.nextInt(result.size()) + 0);
    }
}
```
### 680.Valid Palindrome II
```java
public boolean validPalindrome(String s) {
    int l = -1, r = s.length();
    while (++l < --r) 
        if (s.charAt(l) != s.charAt(r)) return isPalindromic(s, l, r+1) || isPalindromic(s, l-1, r);
    return true;
}

public boolean isPalindromic(String s, int l, int r) {
    while (++l < --r) 
        if (s.charAt(l) != s.charAt(r)) return false;
    return true;
}
```
### Total hamming Distance
```java
class Solution {
    public int totalHammingDistance(int[] nums) {
        int total = 0, n = nums.length;
        for (int j=0;j<32;j++) {
            int bitCount = 0;
            for (int i=0;i<n;i++) 
                bitCount += (nums[i] >> j) & 1;
            total += bitCount*(n - bitCount);
        }
        return total;
    }
}
```
### Group Anagrams
```java
public class Solution {
    public List<List<String>> groupAnagrams(String[] strs) {
        // O(n^2) solution, loop every element to compare if they have the same value
        // O(n) solution, use hashmap<String, LinkedList> to do the same thing
        Map<String, List<String>> map = new HashMap<>();
        for(String str: strs)
        {
             char[] temp = str.toCharArray();
             Arrays.sort(temp);
             String comparison = new String(temp);
             List<String> container = new LinkedList<>();
             if(map.containsKey(comparison))
             {
                 container = map.get(comparison);
                 container.add(str);
                 map.put(comparison, container);
             }
             else
             {
                 container.add(str);
                 map.put(comparison, container);
             }
        }
        return new LinkedList<List<String>>(map.values());
    }
}
```
### 88.Merge Sorted Array
```java
public class Solution {
    public void merge(int[] nums1, int m, int[] nums2, int n) {
        // From the back to the front
        if(m == 0)
        {
            for(int i = 0; i < n; i++) nums1[i] = nums2[i];
        }
        else
        {
            while(m >= 1 && n >= 1)
            {
                if(nums1[m-1] > nums2[n-1])
                {
                    nums1[m+n-1] = nums1[m-1];
                    nums1[m-1] = 0;
                    m--;
                }
                else
                {
                    nums1[m+n-1] = nums2[n-1];
                    n--;
                }
            }
            while(n >= 1)
            {
                    nums1[n-1] = nums2[n-1];
                    n--;
            }
        }
    }
}
```
### 218. The Skyline Problem
```java
class Solution {
    public List<int[]> getSkyline(int[][] buildings) {
        // Priority Queue could remove element by their instance
        List<int[]> height = new ArrayList<>();
        List<int[]> result = new ArrayList<>();
        if(buildings == null || buildings.length == 0) return result;
        for(int[] item : buildings){
            height.add(new int[]{item[0], -item[2]});
            height.add(new int[]{item[1], item[2]});
        }
        height = height.stream()
            .sorted((o1, o2) -> o1[0] - o2[0] + (o1[0] != o2[0] ? 0 : o1[1] - o2[1]))
            .collect(Collectors.toList());
        // Front the largest to the smallest
        Queue<Integer> que = new PriorityQueue<>((o1, o2) -> o2 - o1);
        que.add(0);
        int prev = 0;
        for(int[] item : height){
            if(item[1] < 0){
                que.add(-item[1]);
            }
            else{
                que.remove(item[1]);
            }
            int curr = que.peek();
            if(prev != curr){
                // prevent corner case same height applied
                result.add(new int[]{item[0], curr});
                prev = curr;
            }
        }
        return result;
    }
}
```
### 146.LRU Cache
```java
class ListNode{
    int value;
    int key;
    ListNode prev;
    ListNode next;
    ListNode(int key, int value){
        this.key = key;
        this.value = value;
    }
}

public class LRUCache {
    // optimization, use a hashmap to store the nodes and the searching speed is O(1)
    // Optimally, the adding speed is also O(n)
    int max_cap;
    int node_size;
    HashMap<Integer, ListNode> nodeset;
    ListNode tail;
    ListNode head;
    public LRUCache(int capacity) {
        this.max_cap = capacity;
        this.node_size =0;
        nodeset = new HashMap<>();
        tail = new ListNode(-1,-1);
        head = new ListNode(-1, -1);
        head.next = tail;
        tail.prev = head;
    }
    
    public int get(int key) {
        if(!nodeset.containsKey(key)) return -1;
        int value = nodeset.get(key).value;
        remove(key);
        add(key, value);
        return value;
}
    
    public void put(int key, int value) {
       if(!nodeset.containsKey(key)){
           // kill the tail
           if(nodeset.size() == max_cap){
                int rmkey = tail.prev.key;
                tail.prev.prev.next = tail;
                tail.prev = tail.prev.prev;
                nodeset.remove(rmkey);
           }
           
       }
       else
       {
           remove(key);
       }
        add(key, value);
    }
    
    public void add(int key, int value){
        ListNode temp = new ListNode(key, value);
        temp.prev = head;
        temp.next = head.next;
        head.next.prev = temp;
        head.next = temp;
        nodeset.put(key, temp);
    }
    
    public void remove(int key){
        ListNode rmNode = nodeset.get(key);
        rmNode.prev.next = rmNode.next;
        rmNode.next.prev = rmNode.prev;
        nodeset.remove(key);
    }
}
```
### Minimum Size Subarray Sum
```java
public class Solution {
    public int minSubArrayLen(int s, int[] nums) {
        // write your code here
        if(s ==0) return 1;
        if(nums == null || nums.length == 0) return 0;
        int j = 0;
        int size = nums.length+1;
        int sum = 0;
        for(int i =0; i < nums.length; i++){
            while(j < nums.length && sum < s){
                sum += nums[j];
                j++;
            }
            if(sum >= s){
                size = Math.min(size, j -i);
            }
            sum -= nums[i];
        }
        if(size == nums.length+1) return 0;
        return size;
    }
}
```
### Product of Array Except Self
```java
public class Solution {
    public int[] productExceptSelf(int[] nums) {
        // Calculate from Front to the back Multiplication
        // Calculate from Back to the Front Multiplication
        int[] arr= new int[nums.length];
        arr[0] = nums[0];
        for(int i =1; i < nums.length-1; i++){
            arr[i] = nums[i] * arr[i-1];
        }
        arr[nums.length -1] = arr[nums.length -2];
        arr[nums.length -2] = nums[nums.length -1];
        for(int i = nums.length -2; i > 0; i--){
            int prearr = arr[i];
            arr[i] = arr[i-1] * arr[i];
            arr[i-1] = prearr * nums[i];
        }
        return arr;
    }
}
```
### 286.Walls and Gates
```java
public class Solution {
    int kernel[][] = new int[][]{{1,0},{-1,0},{0,1},{0,-1}};
    public void wallsAndGates(int[][] rooms) {
        // Use Gate as the DFS starter node
        int m = rooms.length;
        if(m==0)return;
        int n = rooms[0].length;
        if(n==0) return;
        LinkedList<int[]> queue = new LinkedList<>();
        for(int i =0; i < m; i++){
            for(int j =0; j < n; j++){
                if(rooms[i][j] == 0) queue.add(new int[]{i,j});
            }
        }
        int level = 0;
        do{
            queue.add(new int[]{-1,-1});
            level++;
            while(queue.get(0)[0] != -1){
                int temp[] = queue.remove(0);
                for(int[] ker: kernel){
                    int x = temp[0]+ker[0];
                    int y = temp[1]+ker[1];
                    if(x >= 0 && y>= 0 && x < m && y < n && rooms[x][y] == 2147483647){
                        rooms[x][y] = level;
                        queue.add(new int[]{x,y});
                    }
                }
                
            }
            queue.remove(0);
        }while(queue.size()!=0);
        
    }
}
```
### 71.Simplify Path
```java
class Solution {
    public String simplifyPath(String path) {
        //Split by '/' and start working
        // Use stack to store
        String[] splitted = path.split("/+");
        // for(String item : splitted) System.out.println(item);
        Stack<String> result = new Stack<>();
        for(int i = 0; i < splitted.length; i++){
            if(splitted[i].equals("..")){
                if(!result.isEmpty()) result.pop();
            }
            else{
                if(!splitted[i].equals(".") && !splitted[i].equals("")) result.add(splitted[i]);
            }
        }
        String submit = "";
        while(!result.isEmpty()){
            submit = "/" + result.pop() + submit;
        }
        if(submit.length() == 0) return "/";
        return submit;
    }
}
```
### Binary Tree Level Order Traversal
```java
public class Solution {
    public List<List<Integer>> levelOrder(TreeNode root) {
        List<List<Integer>> result = new ArrayList<>();
        if(root == null) return result;
        List<TreeNode> queue = new LinkedList<>();
        queue.add(root);
        queue.add(null);
        while(queue.size() != 0){
            List<Integer> temp = new ArrayList<>();
            while(queue.get(0)!= null){
                TreeNode tmp = queue.remove(0);
                if(tmp.left!= null) queue.add(tmp.left);
                if(tmp.right!= null) queue.add(tmp.right);
                temp.add(tmp.val);
            }
            queue.remove(0);
            result.add(temp);
            if(queue.size() == 0) break;
            queue.add(null);
        }
        return result;
    }
}
```
### 128. Longest Consecutive Sequence
```java
public int longestConsecutive(int[] num) {
    int res = 0;
    HashMap<Integer, Integer> map = new HashMap<Integer, Integer>();
    for (int n : num) {
        if (!map.containsKey(n)) {
            int left = (map.containsKey(n - 1)) ? map.get(n - 1) : 0;
            int right = (map.containsKey(n + 1)) ? map.get(n + 1) : 0;
            // sum: length of the sequence n is in
            int sum = left + right + 1;
            map.put(n, sum);
            
            // keep track of the max length 
            res = Math.max(res, sum);
            
            // extend the length to the boundary(s)
            // of the sequence
            // will do nothing if n has no neighbors
            map.put(n - left, sum);
            map.put(n + right, sum);
        }
        else {
            // duplicates
            continue;
        }
    }
    return res;
}
```
### 523.Continuous Subarray Sum
```java
class Solution {
    public boolean checkSubarraySum(int[] nums, int k) {
        // DP Solution, the previous number of digit can combine to k
        // Use new sum - target k * n to see if there is a match previous sum
        if(nums.length < 2) return false;
        for(int i = 1; i < nums.length; i++){
            if(nums[i] == 0 && nums[i-1] == 0) return true;
        }
        if(k == 0) return false;
        if(k < 0) k = -k;
    
        Map<Integer, Integer> map = new HashMap<>();
        map.put(0, -1);
        int sum = 0;
        for(int fast = 0; fast < nums.length; fast++){
            sum += nums[fast];
            for(int j = (sum / k) * k; j >= k; j -= k){
                // make sure index > 1, and has previous sum
                if(map.containsKey(sum - j) && fast - map.get(sum -j) > 1) return true;
            }
            if(!map.containsKey(sum)) map.put(sum, fast);
        }
        return false;
    }
}
```
90. Subsets II
```java
class Solution {
    public List<List<Integer>> subsetsWithDup(int[] nums) {
        Set<List<Integer>> set = new HashSet<>();
        List<List<Integer>> submit = new LinkedList<>();
        if(nums.length == 0){
            submit.add(new LinkedList<>());
            return submit;
        }
        Arrays.sort(nums);
        helper(set, nums, 0);
        submit = new ArrayList<>(set);
        return submit;
    }
    
    private void helper(Set<List<Integer>> set, int[] nums, int pos){
        if(pos == nums.length) {
            List<Integer> temp = new LinkedList<>();
            set.add(temp);
            return;
        }
        helper(set, nums, pos + 1);
        List<List<Integer>> submit = new LinkedList<>();
        for(List<Integer> item : set){
            List<Integer> temp = new LinkedList<>(item);
            temp.add(nums[pos]);
            submit.add(temp);
        }
        for(List<Integer> item : submit) set.add(item);
        
    }
}
```
### 334. Increasing Triplet Subsequence
```java
class Solution {
    public boolean increasingTriplet(int[] nums) {
        int small = Integer.MAX_VALUE, big = Integer.MAX_VALUE;
        for(int num : nums){
            if (num <= small){
                // If first small been found
                small = num;
            }
            else if (num <= big) {
                // If first large been found
                big = num;
            }
            // if larger than both existed
            else return true;
        }
        return false;
    }
}
```
### Implement Trie
```java
class TrieNode {
    // Initialize your data structure here.
    private boolean isWord;
    HashMap<Character, TrieNode> triemap;
    
    public TrieNode(){
        triemap = new HashMap<Character, TrieNode>();
        isWord = false;
    }
    public void setWord(){
        isWord = true;
    }
    public boolean isWord(){
        return isWord;
    }
    public void addchild(char a, TrieNode node){
        triemap.put(a, node);
    }
    public boolean haschild(char a){
        return triemap.containsKey(a);
    }
    public TrieNode get(char a){
        return triemap.get(a);
    }
    public void set(char a, TrieNode node){
        triemap.put(a, node);
    }
}

public class Trie {
    private TrieNode root;

    public Trie() {
        root = new TrieNode();
    }

    // Inserts a word into the trie.
    public void insert(String word) {
        char curr[] = word.toCharArray();
        TrieNode currnode = root;
        for(int i=0; i < curr.length; i++){
            if(!currnode.haschild(curr[i])){
                currnode.set(curr[i], new TrieNode());
            }
            currnode = currnode.get(curr[i]);
        }
        currnode.setWord();
    }

    // Returns if the word is in the trie.
    public boolean search(String word) {
        char curr[] = word.toCharArray();
        TrieNode currnode = root;
        for(int i=0; i < curr.length; i++){
            if(!currnode.haschild(curr[i])){
                return false;
            }
            currnode = currnode.get(curr[i]);
        }
        return currnode.isWord();
    }

    // Returns if there is any word in the trie
    // that starts with the given prefix.
    public boolean startsWith(String prefix) {
        char curr[] = prefix.toCharArray();
        TrieNode currnode = root;
        for(int i=0; i < curr.length; i++){
            if(!currnode.haschild(curr[i])){
                return false;
            }
            currnode = currnode.get(curr[i]);
        }
        if(currnode.isWord() || currnode.triemap.size() != 0){
            return true;
        }
        return false;
    }
}
```
### Word Ladder
```java
public class Solution {
    public int ladderLength(String beginWord, String endWord, List<String> wL) {
        if (beginWord.equals(endWord)) return 1;
        Set<String> wordList = new HashSet<>();
        for(String item : wL) wordList.add(item);
        if(!wordList.contains(endWord)) return 0;
        List<String> myList = new LinkedList<>();
        Set<String> visited = new HashSet<>();
        myList.add(beginWord);
        myList.add(null);
        int level = 1;
        while(myList.size()!=0)
        {
            String curr = myList.get(0);
            myList.remove(0);
            if(curr!=null)
            {
                char[] curr_char = curr.toCharArray();
                for(int i =0; i < curr_char.length;i++)
                {
                    char original = curr_char[i];
                    for(char j ='a';j <='z'; j++)
                    {
                        curr_char[i] = j;
                        String temp = new String(curr_char);
                        if(endWord.equals(temp)) return level+1;
                        if(wordList.contains(temp) && !(visited.contains(temp)))
                        {
                            myList.add(temp);
                            visited.add(temp);
                        }
                    }
                    curr_char[i] = original;
                }
            }
            else
            {
                level++;
                if(myList.size()!=0) myList.add(null);
            }
        }
        return 0;
    }
}
```
### 377. Combination Sum IV
```java
class Solution {
    Map<Integer, Integer> map = new HashMap<>();
    public int combinationSum4(int[] nums, int target) {
        if(nums.length == 0 || target < 0) return 0;
        if(target == 0) return 1;
        // If there exist a value contains all combination of numbers
        // return that one
        if(map.containsKey(target)) return map.get(target);
        int count = 0;
        // Loop through all numbers to find the result...
        for(int num : nums){
            count += combinationSum4(nums, target - num);
        }
        map.put(target, count);
        return count;
    }
    
}
```
### 117. Populating Next Right Pointers in Each Node II
```java
public class Solution {
    public void connect(TreeLinkNode root) {
        if(root == null) return;
        connect(connect2right(root));
    }
    
    public TreeLinkNode connect2right(TreeLinkNode next){
        if(next == null) return null;
        if(next.left == null){
            if(next.right != null){
                next.right.next = connect2right(next.next);
                return next.right;
            }
            else{
                return connect2right(next.next);
            }          
        }
        else{
            if(next.right == null){
                next.left.next = connect2right(next.next);
            }
            else{
                next.left.next= next.right;
                next.right.next = connect2right(next.next);
            }
            return next.left;
        }
    }
}
```
### 85. Maximal Rectangle
```java
class Solution {
    public int maximalRectangle(char[][] matrix) {
        if(matrix == null || matrix.length == 0 || matrix[0].length == 0) return 0;
        int[] height = new int[matrix[0].length+1];
        int maxArea = 0;
        for(int row = 0; row < matrix.length; row++){
            Stack<Integer> s = new Stack<>();
            for(int column = 0; column <= matrix[0].length; column++){
                // Same method used to find the maximal rectangle from histogram
                if(column != matrix[0].length){
                    height[column] = matrix[row][column] == '0' ? 0 : height[column] + 1;
                }
                if(s.isEmpty() || height[column] >= height[s.peek()]){
                    s.push(column);
                }
                else{
                    while(!s.isEmpty() && height[column] < height[s.peek()]){
                        int tp = s.pop();
                        maxArea = Math.max(maxArea, height[tp] * (s.isEmpty() ? column : column - 1 - s.peek()));
                        // System.out.println(maxArea);
                    }
                    s.push(column);
                }
            }
        }
        return maxArea;
    }
}
```
### 210. Course Schedule II
```java
class Solution {
    public int[] findOrder(int numCourses, int[][] prerequisites) {
        if(numCourses == 0) return null;
        int[] prereq = new int[numCourses];
        Queue<Integer> freenode = new LinkedList<>();
        int[] submit = new int[numCourses];
        // Add items into prerequisites
        for(int[] item : prerequisites){
            prereq[item[0]]++;
        }
        // find starting node
        for(int i = 0; i < prereq.length; i++){
            if(prereq[i] == 0) freenode.offer(i);
        }
        int index = 0;
        // Renew Starting nodes
        while(!freenode.isEmpty()){
            int master = freenode.poll();
            for(int[] item : prerequisites){
                if(item[1] == master){
                    prereq[item[0]]--;
                    if(prereq[item[0]] == 0){
                        freenode.offer(item[0]);
                    }
                }
            }
            submit[index] = master;
            index++;
        }
        return index == numCourses ? submit : new int[0];
    }
}
```
### 68. Text Justification
```java
class Solution {
    public List<String> fullJustify(String[] words, int maxWidth) {
        int total = 0;
        List<String> temp = new LinkedList<>();
        List<String> submit = new LinkedList<>();
        for(String word : words){
            if(total + word.length() > maxWidth){
                submit.add(mapper(temp, maxWidth));
                total = 0;
                temp = new LinkedList<>();
            }
            total += word.length() +1;
            temp.add(word);
        }
        if(temp.size() != 0){
            String result = String.join(" ", temp);
            temp = new LinkedList<>();
            temp.add(result);
            submit.add(mapper(temp, maxWidth));
        }
        return submit;
    }
    
    private String mapper(List<String> input, int max){
        int total = 0;
        for(String item : input) total += item.length();
        int slot = input.size() == 1 ? 1 : input.size() -1;
        int reg = (max - total) / slot;
        int extra = (max - total) % slot;
        StringBuilder sb = new StringBuilder();
        for(String item : input){
            sb.append(item);
            for(int i =0; i < reg; i++) sb.append(" ");
            if(extra > 0){
                sb.append(" ");
                extra--;
            }
        }
        return sb.toString().substring(0, max);
    }
}
```
### Maximal Square
```java
public class Solution {
    
    public int maximalSquare(char[][] matrix) {
        // write your code here
        if(matrix == null || matrix.length ==0) return 0;
        int label[][] = new int[matrix.length][matrix[0].length];
        int max = 0;
        for(int i = 0; i < matrix.length; i++){
            for(int j=0; j < matrix[0].length; j++){
                label[i][j] = 0;
                max = Math.max(max, update(matrix, label, i, j));
            }
        }
        return max*max;
    }
    
    private int update(char[][] matrix, int[][] label, int i,  int j){
        if(i==0 || j == 0){
            label[i][j] = (matrix[i][j] == '1') ? 1 : 0;
            return label[i][j];
        }
        if(matrix[i][j] == '1'){
            label[i][j] = Math.min(label[i-1][j-1], Math.min(label[i-1][j], label[i][j-1]))+1;
            return label[i][j];
        }
        return 0;
    }
}
```
### 234.Palindrome Linked List
```java
class Solution {
    public boolean isPalindrome(ListNode head) {
        if(head == null || head.next == null) return true;
        ListNode fast = head.next.next;
        ListNode slow = head;
        boolean odd = false;
        while( fast != null){
            slow = slow.next;
            fast = fast.next;
            if(fast != null){
                fast = fast.next;
            }
        }
        ListNode newhead = slow.next;
        ListNode nextone = newhead.next;
        newhead.next = null;
        while(nextone != null){
            ListNode swap = nextone.next;
            nextone.next = newhead;
            newhead = nextone;
            nextone = swap;
        }

        while(newhead != null){
            if(newhead.val != head.val) return false;
            newhead = newhead.next;
            head = head.next;
        }
        return true;
    }
    
}
```