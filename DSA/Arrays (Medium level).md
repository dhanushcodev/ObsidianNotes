
### Two sum

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


### Sort an arrays of 0's 1's and 2's

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

### Majority Element (>N/2 times):

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

### Maximum subarray sum:

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

