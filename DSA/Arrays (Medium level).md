
### 1. Two sum

return the indexes of two numbers  which leads to target sum
eg: arr = [2,6,5,8,11]  target=14

##### Brute:
```java
for(i=0;i<n;i++){
	for(j=i+1;j<n;j++){
		if(i==j) continue;
		if(arr[i] + arr[j] == target){
		 return i and j
		}
	}
}
```

Time complexity:
O(N2)
Space complexity:
O(1)

##### Better:
```java
//lets create a hashmap of element and index
HashMap<element,index> map = new HashMap<>();
for(int i=0;i<arr.length;i++){
	if(map.contains(target - arr[i])){
		return i,map.get(i);
	}else{
		map.add(arr[i],i);
	}
}
```

Time complexity:
O(N)
Space complexity:
O(N)

##### Optimal:
only works for sorted
```java
// we can use two pointer method 
// this pointer solution only works for sorted array

int i = arr[0];
int j = arr[arr.length-1];

while(i<j){
	if(arr[i]+arr[j] == target){
		return i , j
	}else if(arr[i]+arr[j] > target){
		j--;
	}else{
		i++;
	}
}

```

Time complexity:
O(N)
Space complexity:
O(1)


### 2. Sort an arrays of 0's 1's and 2's

##### brute:
```
use sort functions
```

Time complexity: 
Nlog(N)
Space complexity:
N

##### Better:
```
create 3 variables 
int zero,one,two;

run loop and and get the count of all
for(int i=0;i<n;i++){
	if(arr[i] equals 0) zero++
	else if(arr[i] equals 1) one++
	 else two++
}

now 
for(int i=0;i<zero;i++) arr[i]=0;
for(int j=zero;j<zero+one;j++) arr[j]=1;
for(int k=zero+one;k<zero+one+two;k++) arr[k]=2;

```

Time complexity:
O(2N)
Space complexity:
0(1)

##### Optimal:
explaination: [[Duch national flag Alogrithm]]
```java
class Solution {

    public void swap(int[] nums,int a,int b){
        int temp = nums[a];
        nums[a] = nums[b];
        nums[b] = temp;
        
    }

    public void sortColors(int[] nums) {
        int low = 0;
        int mid = 0;
        int high = nums.length - 1;

        while(mid<=high){
            if(nums[mid] == 0){
                swap(nums,low,mid);
                low++;mid++;
            }
            else if(nums[mid] == 1){
                mid++;
            }
            else{
                swap(nums,mid,high);
                high--;
            }
        }
    }
}
```

Time complexity:
O(N)
Space complexity:
O(1)

### 3. Majority Element (>N/2 times):

Given an array `nums` of size `n`, return _the majority element_.

The majority element is the element that appears more than `⌊n / 2⌋` times. You may assume that the majority element always exists in the array.

arr[] = [2, 2, 3, 3, 1, 2, 2]

##### Brute solution:
```java
for(i=0;i<n;i++){
	cnt = 0;
	for(j=0;j<n;j++){
		if(arr[j] == arr[i])
			cnt++;
	}
	if(cnt > n/2) return arr[i]
}
```

Time complexity:
O(N2)
Space complexity:
O(1)

##### Better:
using hashing
```java
HashMap<Integer,Integer> map = new HashMap<Integer,Integer>();

for(int i=0;i<n;i++){
	map.put(arr[i],map.getOrDefault(arr[i], 0) + 1);
}

for(Map.Entry<Integer,Integer> entry : map.entrySet()){
	int key = entry.getKey();
	int value = entry.getValue();
	if(value > arr.length/2){
		return key;
	}
}
return -1;
```

Time complexity:
O(N logN) + O(N)
Space complexity:
O(N)

##### Optimal:

#### Boyer-Moore Majority Voting Algorithm:

When the elements are the same as the candidate element, votes are incremented whereas when some other element is found (not equal to the candidate element), we decreased the count. This actually means that we are decreasing the priority of winning ability of the selected candidate, since we know that if the candidate is in majority it occurs more than N/2 times and the remaining elements are less than N/2. We keep decreasing the votes since we found some different element(s) than the candidate element. When votes become 0, this actually means that there are the equal  number of votes for different elements, which should not be the case for the element to be the majority element. So the candidate element cannot be the majority and hence we choose the present element as the candidate and continue the same till all the elements get finished. The final candidate would be our majority element. We check using the 2nd traversal to see whether its count is greater than N/2. If it is true, we consider it as the majority element.

example:
Input: { 1,7,2,7,8,7,7}

Index 0: majorityElement = 1, count =1  
Index 1: majorityElement = 1, count =0 ( 7 not equal to 1)  
Index 2: majorityElement = 2 (as the count is 0 we initalise majorityElement to current), count =1  
Index 3: majorityElement = 2, count =0 ( 7 not equal to 2)  
Index 4: majorityElement = 8 (as the count is 0 we initalise majorityElement to current), count =1  
Index 5: majorityElement = 8, count =0 ( 7 not equal to 8)  
Index 6: majorityElement = 7 (as the count is 0 we initalise majorityElement to current), count =1

Now we can check for the frequency of 7, i.e, 4 which is > 7/2.

```java
public int majorityElement(int[] nums) {
        int count = 1;
        int majorityElement = nums[0];
        for(int i=1;i<nums.length;i++){
            if(majorityElement != nums[i]) count--;
            else count++;

            if(count==0) {
                majorityElement = nums[i];
                count++;
            }
        }
        // To check wether the majority element is actually correct
        count = 0;
        for(int i=0;i<nums.length;i++){
            if(majorityElement==nums[i]){
                count++;
                if(count > nums.length/2) return majorityElement;
            }
        }
        return -1;
    }
```

Time complexity:
O(N)
Space complexity:
O(1)

### 4. Maximum subarray sum:

Given an integer array `nums`, find the subarray with the largest sum, and return _its sum_.

**Example 1:**

**Input:** nums = [-2,1,-3,4,-1,2,1,-5,4]
**Output:** 6
**Explanation:** The subarray [4,-1,2,1] has the largest sum 6.

##### Brute force:
```java
for(int i=0; i<n; i++){
	for(int j=i;j<n;j++){
		int sum = 0;
		for(int k = i;k<j;k++){
			sum+=arr[k]
			max = max(sum,max)
		}
	}
}
```

Time complexity: O(N3)
Space complexity: O(1)

##### Better solution:
```java
for(int i=0; i<n; i++){
	for(int j=i;j<n;j++){
		int sum +=arr[j];
		max = max(sum,max)
	}
}
```

Time complexity: O(N2)
Space complexity: O(1)

##### Optimal Solution:

Kadane's Algorithm: 
[[Kadane's Algorithm]]

```java

class Solution {
    public int maxSubArray(int[] nums) {
        int max = nums[0];
        int current = nums[0];
        for(int i = 1; i < nums.length; i++){
            current = Math.max(nums[i],nums[i] + current);
            max = Math.max(max,current);
        }
        return max;
    }
}

or 

class Solution {
    public int maxSubArray(int[] nums) {
        int n = nums.length;
        int max = Integer.MIN_VALUE, sum = 0;
        
        for(int i=0;i<n;i++){
            sum += nums[i];
            max = Math.max(sum,max);
            
            if(sum<0) sum = 0;
        }
        
        return max;
    }
}
```

Time complexity:
O(N)
Space complexity:
O(1)

### 5. Best Time to Buy and Sell Stock

You are given an array `prices` where `prices[i]` is the price of a given stock on the `ith` day.

You want to maximize your profit by choosing a **single day** to buy one stock and choosing a **different day in the future** to sell that stock.

Return _the maximum profit you can achieve from this transaction_. If you cannot achieve any profit, return `0`.

**Input:** prices = [7,1,5,3,6,4]
**Output:** 5
##### Brute force:

iterate through all the possible cases and find the max profit

```java
int pofit = 0;
for(int i = 0;i < arr.length-1;i++){
	for(int j = i+1; j < arr.length; j++){
		profit = Math.profit(profit,arr[j]-arr[i]);
	}
}
```

Time complexity:
O(N2)
Space complexity:
O(1)

##### Optimal solution:

[[Best Time to Buy and sell Stock]]
```java
```java
class Solution {
    public int maxProfit(int[] prices) {
        if(prices.length == 1) return 0;
        int min = prices[0];
        int profit =0;
        for(int i=1;i<prices.length;i++){
            if(prices[i]<min) min=prices[i];
            profit = Math.max(profit,prices[i]-min);
        }
        return profit;
    }
}
```

### 6. Rearrange the array in alternating positive and negative items

You are given a **0-indexed** integer array `nums` of **even** length consisting of an **equal** number of positive and negative integers.

You should return the array of nums such that the the array follows the given conditions:

1. Every **consecutive pair** of integers have **opposite signs**.
2. For all integers with the same sign, the **order** in which they were present in `nums` is **preserved**.
3. The rearranged array begins with a positive integer.

Return _the modified array after rearranging the elements to satisfy the aforementioned conditions_.

**Example 1:**

**Input:** nums = [3,1,-2,-5,2,-4]
**Output:** [3,-2,1,-5,2,-4]
**Explanation:**
The positive integers in nums are [3,1,2]. The negative integers are [-2,-5,-4].
The only possible way to rearrange them such that they satisfy all conditions is [3,-2,1,-5,2,-4].
Other ways such as [1,-2,2,-5,3,-4], [3,1,2,-2,-5,-4], [-2,3,-5,1,-4,2] are incorrect because they do not satisfy one or more conditions.

##### Brute force:

create the separate positive and negative arrays 
then pick the positive and negative alternatively and add it to array 
it will create the required array.

```java
class Solution {

    public int[] rearrangeArray(int[] nums) {
    
        int n = nums.length;
        int[] positiveArr = new int[n/2];
        int[] negativeArr = new int[n/2];
        int[] result = new int[n];
        int pIndex = 0;
        int nIndex = 0;
  
        for(int i=0;i<n;i++){
            if(nums[i]>0){
                positiveArr[pIndex++] = nums[i];
            }else{
                negativeArr[nIndex++] = nums[i];
            }
        }

        pIndex = 0;
        nIndex = 0;
        int i = 0;
        
        while(i<n){
            result[i++] = positiveArr[pIndex++];
            result[i++] = negativeArr[nIndex++];
        }
        return result;
    }
}
```

Time complexity:
O(2N)
Space complexity:
O(N)

##### Optimal:

Two pointer method keep positive index and negative index.
keep on updating the positive or negative accordingly.

```java
class Solution {
    public int[] rearrangeArray(int[] nums) {
        int n = nums.length;
        int[] result = new int[n];
        int pIndex = 0;
        int nIndex = 1;

        for(int i=0;i<n;i++){
            if(nums[i]>0){
                result[pIndex] = nums[i];
                pIndex+=2;
            }else{
                result[nIndex] = nums[i];
                nIndex+=2;
            }
        }
        return result;
    }
}
```

Time complexity:
O(N)
Space complexity:
O(N)

### 7. Next Permutation

A **permutation** of an array of integers is an arrangement of its members into a sequence or linear order.

- For example, for `arr = [1,2,3]`, the following are all the permutations of `arr`: `[1,2,3], [1,3,2], [2, 1, 3], [2, 3, 1], [3,1,2], [3,2,1]`.

The **next permutation** of an array of integers is the next lexicographically greater permutation of its integer. More formally, if all the permutations of the array are sorted in one container according to their lexicographical order, then the **next permutation** of that array is the permutation that follows it in the sorted container. If such arrangement is not possible, the array must be rearranged as the lowest possible order (i.e., sorted in ascending order).

- For example, the next permutation of `arr = [1,2,3]` is `[1,3,2]`.
- Similarly, the next permutation of `arr = [2,3,1]` is `[3,1,2]`.
- While the next permutation of `arr = [3,2,1]` is `[1,2,3]` because `[3,2,1]` does not have a lexicographical larger rearrangement.

Given an array of integers `nums`, _find the next permutation of_ `nums`.

The replacement must be in place and use only constant extra memory.

**Example 1:**

**Input:** nums = [1,2,3]
**Output:** [1,3,2]

**Example 2:**

**Input:** nums = [3,2,1]
**Output:** [1,2,3]

**Example 3:**

**Input:** nums = [1,1,5]
**Output:** [1,5,1]

##### Brute force:

1. Generate all the permutations
2. Arrange them in sorted order
3. Just return the next occurring permutation for the given premutation

Time complexity:
For three numbers 3! of permutations for N numbers N! permutations and each one of them is of length N
O(N! x N)

##### Optimal:

1. We need to find the first decreasing element from the right of the list.
2. Then, we need to find the smallest element from the right side that is greater than the first decreasing element.
3. We swap these two elements
4. Finally we reverse the sublist from the first decreasing element to the end of the list.

```java
class Solution {
    public void nextPermutation(int[] nums) {
        int n = nums.length;
        int ind = -1;
        for(int i=n-2;i>=0;i--){
            if(nums[i]<nums[i+1]){
                ind = i;
                break;
            }
        }
        if(ind == -1){
            reverse(nums,0,n-1);
        }else{
            for(int i=n-1;i>ind;i--){
                if(nums[i]>nums[ind]){
                    swap(nums,i,ind);
                    break;
                }
            }
            reverse(nums,ind+1,n-1);
        }
    }

    public void swap(int[] arr, int i, int j){
        int temp = arr[i];
        arr[i] = arr[j];
        arr[j] = temp;
    }

    public void reverse(int[] arr, int start,int end){
        while(start < end){
            swap(arr,start++,end--);
        }
    }
}
```

Time complexity:
O(3N)
Space complexity:
O(1)

### 8. Array Leader

You are given an array **`arr`** of positive integers. Your task is to find all the leaders in the array. An element is considered a leader if it is greater than or equal to all elements to its right. The rightmost element is always a leader.

**Examples:  

**Input:** arr = [16, 17, 4, 3, 5, 2]
**Output:** [17, 5, 2]
**Explanation:** Note that there is nothing greater on the right side of 17, 5 and, 2.

**Input:** arr = [10, 4, 2, 4, 1]
**Output:** [10, 4, 4, 1]  
**Explanation:** Note that both of the 4s are in output, as to be a leader an equal element is also allowed on the right. side.

**Input:** arr = [5, 10, 20, 40]  
**Output:** [40]  
**Explanation:** When an array is sorted in increasing order, only the rightmost element is leader.

**Input:** arr = [30, 10, 10, 5]  
**Output:** [30, 10, 10, 5]  
**Explanation:** When an array is sorted in non-increasing order, all elements are leaders.

##### Brute force:

For each element check if there exist the element greater than that element.

```java
for(int i=0;i<n;i++){
	boolean leader = true;
	for(int j=i+1;j<n;j++){
		if(arr[j] > arr[i] ){
			leader = false;
			break;
		}
	}
	if(leader){
		result.add(arr[i]);
	}
}
```

Time complexity:
O(N2)
Space complexity:
O(N)

##### optimal:

Iterate from the right side keep the last element as max element, and then iterate through the array if you find max element then its the leader also its the max element.

```java
class Solution {
    static ArrayList<Integer> leaders(int arr[]) {
        // code here
        int n = arr.length;
        int max = arr[n-1];
        ArrayList<Integer> result = new ArrayList<>();
        result.add(max);
        for(int i=n-2;i>=0;i--){
            if(arr[i]>=max){
                max = arr[i];
                result.add(max);
            }
        }
        Collections.reverse(result);
        return result;
    }
}
```

Time complexity:
O(N LogN)
Time complexity:
O(N)

### 9. Longest consecutive sequence

Given an unsorted array of integers `nums`, return _the length of the longest consecutive elements sequence._

You must write an algorithm that runs in `O(n)` time.

**Example 1:**

**Input:** nums = [100,4,200,1,3,2]
**Output:** 4
**Explanation:** The longest consecutive elements sequence is `[1, 2, 3, 4]`. Therefore its length is 4

arr = [0,3,7,2,5,8,4,6,0,1]

##### Brute force:
1. sorting of the array [0,0,1,2,3,4,5,6,7]
2. now check for the consecutive items count by logic.
3. logic is keep the first number as min element i.e min = arr[o], consecutive = 1
4. now compare it with next element in loop i.e if min == arr[i] do nothing if min+1 = arr[i] increase the count of consecutive 

```
sort(arr);
int lastsamllest = arr[0]
int count = 1
int longest = 0
for(int i=1;i<n;i++){
		if(arr[i]-1 == min){
			count++;
			lastsmallest = arr[i]
		}else{
			count=1;
			lastsamllest = arr[i]
		}
		longest = max(longest,count);
}
```

Time complexity:
O(logN N)
Space complexity:
O(1)

##### Optimal

1. put everything into set arr[102, 4, 100, 1, 101, 3, 2, 1, 1] 
2. iterate through set now eg: we have 102 check wether 102 - 1 is present in the set or not, if it is present do nothing , if it is not now keep on checking for +1 with count as well
3. u can the the max count as answer

```java
class Solution {
    public int longestConsecutive(int[] nums) {
        if(nums==null || nums.length==0){
            return 0;
        }
        HashSet<Integer> set = new HashSet<Integer>();
        
        int longest = 0;
        for(int i: nums){
            set.add(i);
        }
        for(int num: set){
            if(!set.contains(num-1)){
                int x = num;
                int count = 1;
                while(set.contains(x+1)){
                    count++;
                    x++;
                }
                longest = Math.max(longest,count);
            }
        }
        return longest;
    }
}
```

Time complexity:
O(n)
Space complexity:
O(1)

### 10. Set matrix zeros

Given an `m x n` integer matrix `matrix`, if an element is `0`, set its entire row and column to `0`'s.
You must do it in place

**Example 1:**
**Input:** matrix = `[[1,1,1],[1,0,1],[1,1,1]]`
**Output:** `[[1,0,1],[0,0,0],[1,0,1]]`

**Example 2:**
**Input:** matrix = `[[0,1,2,0],[3,4,5,2],[1,3,1,5]]`
**Output:** `[[0,0,0,0],[0,4,5,0],[0,3,1,0]]`

##### Brute force:
1. First, we will use two loops(_nested loops_) to traverse all the cells of the matrix.
2. If any cell (i,j) contains the value 0, we will mark all cells in row i and column j with -1 except those which contain 0.
3. We will perform step 2 for every cell containing 0.
4. Finally, we will mark all the cells containing -1 with 0.
5. Thus the given matrix will be modified according to the question
note: this doesn't work if the matrix has negative values.

Time complexity:
O((NxM)x(N + M)) + O(NxM)
space complexity:
O(1)

##### Better solution:
1. First, we will declare two arrays: a **row** array of size N and a **col** array of size M and both are initialized with 0.
2. Then, we will use two loops(_nested loops_) to traverse all the cells of the matrix.
3. If any cell (i,j) contains the value 0, we will mark ith index of row array i.e. row[i] and jth index of col array col[j] as 1. It signifies that all the elements in the ith row and jth column will be 0 in the final matrix.
4. We will perform step 3 for every cell containing 0.
5. Finally, we will again traverse the entire matrix and we will put 0 into all the cells (i, j) for which either row[i] or col[j] is marked as 1.
6. Thus we will get our final matrix.

Time complexity:
O(2 x (N x M) )
Space complexity:
0(N) + O(M)

##### Optimal solution:
1. Here we can use the first row and first column as tracker for zeros
2. first row as the trackers for cols and first column as the trackers for rows
3. But `matrix[0][0]` is used by both col0 and row0 tracker 
4. so we separate row0 tracker and well keep `matrix[0][0]` as col0 tacker
5. then fill the zeros accordingly
6. also check for row0 and col0 separately

```java
class Solution {
    public void setZeroes(int[][] matrix) {
        int row0 = matrix[0][0];// we are using matrix[0][0] as col0 tracker
        // so need to have new variable to track row0
        for(int r = 0;r<matrix.length;r++){
            for(int c = 0;c<matrix[0].length;c++){
                if(matrix[r][c] == 0){
                    if(r>0){
                        matrix[r][0] = 0;
                    }else{
                        row0 = 0;
                    }
                    matrix[0][c] = 0;
                }
            }
        }

        for(int r = 1; r < matrix.length; r++){
            for(int c = 1; c < matrix[0].length; c++){
                if(matrix[r][0] == 0 || matrix[0][c] == 0){
                    matrix[r][c] = 0;
                }
            }
        }
        if(matrix[0][0] == 0){
             for(int i = 0;i < matrix.length;i++){
                matrix[i][0] = 0;
            }
        }
        if(row0 == 0){
            for(int j = 0; j < matrix[0].length;j++){
                matrix[0][j] = 0;
            }
        }
    }
}
```

Time complexity:
O(2x(NxM))
Space complexity:
O(1)

