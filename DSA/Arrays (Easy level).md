#### Largest Element in an array

Brute force solution:
```pseudo
arr = [3,2,1,5,2]
sort(arr)
print(arr[lastindex])
```

Optimal solution:
```pseudo
arr = [3,2,1,5,2]
largest = arr[0]
for i <- 1 to n-1
	if arr[i] > largest 
		largest = arr[i]
```

Time complexity:
O(N)
Space complextiy:
O(1)
#### Second Largest element in an array

Brute force solution:
```pseudo
arr = [1,2,4,7,7,5]
sort(arr)    // sort the array
[1,2,3,4,5,7,7]
largest = arr[end]
secLargest = -1
for i <- n-2 to 0   // check for the second largest from right to left
	if arr[i] != largest
		secLargest = arr[i]
		break
```

Better solution
```
arr = arr = [1,2,4,7,7,5]
largest = 1
//first parse
for i <- 0 to n-1
	if arr[i] > largest    // find the largest in first parse
		largest = arr[i]

secLargest = -1
//second parse
for i <- 0 to n-1         // find the second largest in second parse
	if arr[i] > seclargest && arr[i] != largest
		secLargest = arr[i]

```

Optimal solution
```pseudo
arr = [1,2,4,7,7,5]
largest = arr[0]
secondLargest = -1

for i <- 1 to n-1
	if arr[i]>largest
		secondLargest = largest
		largest = arr[i]
	else if arr[i] < largest && arr[i] > secondLargest
		secondLargest = arr[i]
```

Time complexity:
O(N)
Space complexity:
O(1)

#### Check if array is sorted

```
for i <- 0 to n-1
	if arr[i] > arr[i-1]
	else return false
```

Time complexity:
O(N)
Space complexity:
O(1)

#### Remove duplicates in-place from sorted array

Brute force solution:
```
arr = [1,1,2,2,2,3,3]
// first parse
set st
for i <- 0 to n-1    // add all the array elements to set
	st.add(arr[i])

// second parse
index = 0
for i in set        // now add all the set elements to array
	arr[index++] = i
```

Optimal solution:
```
arr = [1,1,2,2,2,3,3]
// pointer approach
i = 0            // set a pointer to 0 index
for j <- 1 to n-1
	if arr[j] != arr[i]    // compare the pointer element
		arr[i+1] = arr[j]  // as its sorted array! found different element? add that element to next pointer index
		i++
return i+1
```

Time complexity:
O(N)
Space complexity:
O(1)

#### Left Rotate an array by one place

Brute force solution:
```
arr = [1,2,3,4,5,6,7]

temp <- arr[0]      // store the first element in temp
for i <- 1 to n-1   // now shift the elements to the left by one place
	arr[i-1] = arr[i]
arr[n-1] = temp     // now last element is replace by temp

```

Time complexity:
O(N)
Space complexity:
O(1)

#### Left Rotate an array by K places

Brute force solution
```
arr = [1,2,3,4,5,6,7]
k = 3
k = k % n // if k is greater the length of array
temp[] = [1,2,3] // strore the first k elements in temp
for i <- k to n-1   // now shift the elements to the left by k places
	arr[i-k] = arr[i]
for j <- n-k to n-1
	arr[j] = temp[j-(n-k)] // now add the temp to the last of the array
```

Time complexity:
O(N)
Space complexity:
O(K)

optimal solution
```
arr = [1,2,3,4,5,6,7]
k = 3
k = k % n // if k is greater than length of array
reverse(arr,0,n) //[7,6,5,4,3,2,1]
reverse(arr,0,k) //[5,6,7,4,3,2,1]
reverse(arr,k+1,n) //[5,6,7,1,2,3,4]
```

Time complexity:
O(N)
Space complexity:
O(1)

code:
```java
public void rotate(int[] nums, int k) {
        int n = nums.length;
        if(nums.length==1 || k==0) return;
        k = k % n;
        reverse(nums,0,n-1);
        reverse(nums,0,k-1);
        reverse(nums,k,n-1);
    }

    public void reverse(int[] nums,int start,int end){
        while(start<=end){
            int temp = nums[start];
            nums[start] = nums[end];
            nums[end] = temp;
            start++;
            end--;
        }
    }
```

#### Move Zeros to end

Brute force solution
```
nums = [0,1,0,3,12]
extract all the non zero numbers
temp = [1,3,12] add remaining zeros at the end
nums = [1,3,12,0,0]

```

Time complexity:
O(N)
O(2N)

optimal solution
```java
  public void moveZeroes(int[] nums) {
        int i = 0; // start of the array
        for(int j=0;j<nums.length;j++){ // check for non zero from start of the array
            if(nums[j] != 0){ // if non zero found place in i index and increment i
                int temp = nums[j];
                nums[j] = nums[i];
                nums[i] = temp;
                i++;
            }
        }
    }
```

#### Find the Union

union of two arrays is the unique elements of both the arrays

Brute force solution
```
set st;
for(i=0;i<n;i++){        O(n2)
	st.insert(arr1[i])   O(log n)
}                        O(n1 log n)

for(j=0;j<n;j++){        O(n2)
	st.insert(arr2[j])   O(log n)
}                        O(n2 log n)

union[st.size()]
i=0
for(auto it: st){
	union[i++] = it;     O(n1 + n2)
}                        
```

Time complexity:
O(n1 log n + n2 log n) + O(n1 + n2)
Space complexity:
O(n1 + n2) + O(n1 + n2)

optimal solution
```java
//well be using two pointer approach
public static int[] findUnion(int a[], int b[]) {
        // code here
        int n1 = a.length;
        int n2 = b.length;
        
        int i = 0; //first pointer to first element of a
        int j = 0; //second pointer to first element of b
        
        ArrayList<Integer> union = new ArrayList<>(); //empty list
        
        while(i<n1 && j<n2){  //until any one of the array gets exhausted
            if(a[i] <= b[j]){ // if a[i] < b[j] add a[i] to union
                if(union.size() == 0 || union.get(union.size()-1) != a[i]){
                    union.add(a[i]);
                    i++;
                }
            }
            else{ // if b[j] < a[i] add b[j] to union
                if(union.size() == 0 || union.get(union.size()-1) != b[j]){
                    union.add(b[j]);
                    j++;
                }
            }
        }
        
        while(i<n1){ // check for the remaining element after exuastion
            if(union.size() == 0 || union.get(union.size()-1) != a[i]){
                    union.add(a[i]);
                    i++;
            }
        }
        
        while(j<n2){ // check for the remaining elements after exuastion
            if(union.size() == 0 || union.get(union.size()-1) != b[j]){
                    union.add(b[j]);
                    j++;
                }
        }
        
        return union.toArray(); // convert it to array note: dono wether it works
    }
```

Time complexity:
O(n1 + n2)
Space complexity:
O(n1 + n2)

#### Intersection of two sorted arrays

Intersection is common element in both the array

brute force:
```
A[] = [1,2,2,3,3,4,4,6]
B[] = [2,3,3,5,6,6,7]
vis[]=[0,0,0,0,0,0,0]
arr[] = []

take first element from A[] i.e 1 
check if the element is present in B
if present make visited 1 add the number to arr

arr[2,3,6]

for i -> 0 to n1
	for j -> 0 -> n2
		if A[i] == B[j] && vis[j] != 1
			arr.add(A[i])
			vis[j] = 1
			break;
		if B[j] > A[i] break; // as the array are sorted there is no point in chekcing after b[j]>a[i] 

```

Time complexity:
O(n1 x n2)
Space complexity:
O(n2)

Optimal solution:
```
// well be using two pointer approach
// this solution only works for sorted array
A[] = [1,2,2,3,3,4,4,6]
B[] = [2,3,3,5,6,6,7]
inter[]
i = 0
j = 0

while(i<n1 && j<n2) 
	if A[i] == B[j]
		if(!inter.contains(A[i]))
		 inter.add(A[i])
		i++;
		j++;
	else 
		i++;
```

Time complexity:
O(min(n1,n2))
Space complexity:
O(min(n1,n2))

optimal code:
works for sorted and non sorted
```java
//this solution works for both sorted and non sorted
public int[] intersection(int[] nums1, int[] nums2) {

        Set<Integer> set1 = new HashSet<>();
        Set<Integer> resultSet = new HashSet<>();

        for(int num: nums1) set1.add(num);
        
        for(int num: nums2){
            if(set1.contains(num)) resultSet.add(num);
        }
        
        int[] result = new int[resultSet.size()];
        int i = 0;
        
        for(int num: resultSet) result[i++] = num;
        return result;

    }
```

Time complexity:
O(N + M)
Space complexity:
O(1)

#### Missing Number

Given an array `nums` containing `n` distinct numbers in the range `[0, n]`, return _the only number in the range that is missing from the array

**Input:** nums = [3,0,1]
**Output:** 2

**Explanation:**
`n = 3` since there are 3 numbers, so all numbers are in the range `[0,3]`. 2 is the missing number in the range since it does not appear in `nums`.

brute force:
```
for(i=1;i<=N;i++){
	flag = 0;
	for(int j=0;j<n-1;j++){
		if(arr[j] == i){
			flag = 1;
			break;
		}
	}
	if(flag == 0) return i;
}
```

Time complexity:
O(n X n)
Space complexity:
O(1)

better solution:
```
arr[] = [1,2,4,5] n=5
hash[n+1] ={0}
for(i=0;i<n;i++){
	hash[arr[i]] = 1;
}
for i = 1 -> n
	if(hash[i] == 0) return i;
```

Time complexity:
O(2n)
Space complexity:
O(N)

optimal solution:
```
for n = 5
sum should be n(n+1)/2
sum = 5(6)/2 = 15

if (sum(arr) != sum )
	result = sum - sum(arr)

alternate solution


```

Time complexity:
O(n)
Space complexity:
O(1)

optimal solution:
xor method
```java
// when same number are xor'd then the the value is zero 
// eg 1^1 = 0 0^1 = 1 hence 1^1^2 = 2
// for arr = [3,0,1]
// 3 ^ 0 ^ 1
// 1 ^ 2 ^ 3
// 3 ^ 0 ^ 1 ^ 1 ^ 2 ^ 3 = 2 missing number
public class Solution {
    public int missingNumber(int[] nums) {
        int n = nums.length;
        int ans = 0;
        for (int i = 1; i <= n; i++) {
            ans = ans ^ i;
        }
        for (int i = 0; i < n; i++) {
            ans = ans ^ nums[i];
        }
        return ans;
    }
}
```

Time complexity:
O(n)
Space complexity:
O(1)

#### Maximum consecutive ones

Given a binary array `nums`, return _the maximum number of consecutive_ `1`_'s in the array_.

**Example 1:**
**Input:** nums = [1,1,0,1,1,1]
**Output:** 3
**Explanation:** The first two digits or the last three digits are consecutive 1s. The maximum number of consecutive 1s is 3.

```java
public int findMaxConsecutiveOnes(int[] nums) {
        int count = 0;
        int max = 0;
        
        for(int i=0;i<nums.length;i++){
            if(nums[i]==1){
                count++;
                max = Math.max(max,count);
            }
            else{
                count = 0;
            }
        }
        return max;
    }
```

Time complexity:
O(N)
Space complexity:
O(1)

#### Find the number that appears once

Given a **non-empty** array of integers `nums`, every element appears _twice_ except for one. Find that single one.

**Example 1:**
**Input:** nums = [2,2,1]
**Output:** 1

brute force:
```
for i = 0 -> n
	num = arr[i]
	count = 0
	for j = 0 -> n:
		if num == arr[j]:
			count++
	if count == 1 return num;
```

Time complexity:
O(n2)
Space complexity:
O(1)

better solution:
```
// we can use hash map
hashmap<Int,Int> hash
	for i = 0 -> n:
		if(hash.containsKey(arr[i])){
			hash.put(arr[i],hash.get(arr[i])+1)
		}else{
			hash.put(arr[i],1)
		}

for(int i : hash.keySet()){
	if (hash.get(i) < 2) return i;
}
```

Time complexity:
O(n)
Space complexity:
O(n)

optimal solution:
```java
  public int singleNumber(int[] nums) {
        int xor = 0;
        for(int i: nums){
            xor ^= i;
        }
        return xor;
    }
```

Time complexity:
O(n)
Space complexity:
O(1)

#### Longest Subarray with given Sum K(Positives)

Problem Statement: Given an array and a sum k, we need to print the length of the longest subarray that sums to k.

Example 1:
Input Format: N = 3, k = 5, array[] = {2,3,5}
Result: 2
Explanation: The longest subarray with sum 5 is {2, 3}. And its length is 2

Brute force:

```
go through the sum of all the sub array
for(i=0; i<n; i++){
	sum = 0
	for(j=i,j<n;j++){
		sum += a[j]
		if(sum == k) len = max(len,j-i+1)
	}
}

return len;
```

Time complexity:
O(N2)
Space complexity:
O(1)

Better solution:
```
eg:
arr[] = [1,2,3,1,1,1,1,4,2,3]
k = 3
hashmap = {}

here were are finding total sum and then subtracting k
then we get get the remaining value
if the remaining value was already there if can find its index in hashmap and subract it with current index so the we can find the len of the subarray

example 1,2,3 total sum is 6, in map we already have 1 -> 0, 3 -> 1
so the sum 6 - k will become 3
we already had the sum 3 i.e till 0 to 1 index we have sum 3
so now if we subract its index with current well get to know the len 
[1,2]
   1 - index
[1,2,3]
     2 -> current index
2 - 1 = 1
i.e it means len is 1 of the sub array

Dry run:

intitally sum = arr[0]
len = 0

now sum till index 0   sum = 1
hashmap.add(1,0) {sum,index}

now sum till index 1  sum = 3
hashmap.add(3,1)

i.e  if sum == k:
len = hashmap.get(k) + 1 , get the lenght of subarray
len = 2

now sum till index 2   sum = 6
now sum - k = 3
hashmap already has 3
i.e 3 -> 1

index 2 - map(3) 
2 - 1 = 1

it means that the its length is only 1

```

better code:
```java
public int longestSubarray(int[] arr, int k) {
        // code here
    HashMap<Long,Integer> presum = new HashMap<Long,Integer>();
    int n = arr.length;
	long sum = 0;
	int maxLen = 0;
	for(int i=0; i<n; i++){
		sum += arr[i]; //sum till arr[i]
		if(sum == k) { // sum is equal to k , then find its length compare with maxlen
			maxLen = Math.max(maxLen, i+1);
		}
		long rem = sum - k;// get the remaining sum 
		if(presum.containsKey(rem)){ // it should not be end element and if the remaining sum is present in hashmap
			int len = i - presum.get(rem); // then the current index - remaining sum index
			maxLen = Math.max(len,maxLen);// check the lenth from rem sum index to current index , because the sum of rem index to current index is k
		}
		 presum.put(sum,i); // add the current sum to creent index in map
	}
	return maxLen;
    }
```

the above solution will not work for negatives and if the array as 0's and for negative

```
example arr[] = [2,0,0,3] k =3

if we apply the same logic as above

index - > 0
sum = 2
presum.add(2,0)

index -> 1
sum = 2
presum.add(2,1)

index -> 2
sum = 2
presum.add(2,2)

index -> 3
sum = 5

now rem = sum - k = 2
presum.has(2) -> yes

now cureent index i.e 3 and presm[rem] i.e 2
3 -1 = 1
but actually is [0,0,3] will be the answer
we get the index as 1 which will be wrong

```

simple fix for the above problem is that if the sum already exists it should not reupdate 
```java
public int longestSubarray(int[] arr, int k) {
        // code here
    HashMap<Long,Integer> presum = new HashMap<Long,Integer>();
    int n = arr.length;
	long sum = 0;
	int maxLen = 0;
	for(int i=0; i<n; i++){
		sum += arr[i]; //sum till arr[i]
		if(sum == k) { // sum is equal to k , then find its length compare with maxlen
			maxLen = Math.max(maxLen, i+1);
		}
		long rem = sum - k;// get the remaining sum 
		if(presum.containsKey(rem)){ // it should not be end element and if the remaining sum is present in hashmap
			int len = i - presum.get(rem); // then the current index - remaining sum index
			maxLen = Math.max(len,maxLen);// check the lenth from rem sum index to current index , because the sum of rem index to current index is k
		}
		if(!presum.containsKey(sum))
		 presum.put(sum,i); // add the current sum to creent index in map
	}
	return maxLen;
    }
```

Time complexity : 
O(N x 1)
Space complexity:
O(N)

optimal: 
it only works for positive numbers and zeros
```java
public int longestSubarray(int[] arr, int k) {
        // code here
    int maxLen = 0;
    int left = 0;
    int right = 0;
    long sum = arr[0];
    int n = arr.length;
    
    while(right < n){
        while(left<=right && sum>k){
            sum-=arr[left];
            left++;
        }
        while(sum == k){
            maxLen = Math.max(maxLen,right-left +1);
        }
        right++;
        if(right<n) sum+=arr[right];
    }
    
	return maxLen;
    }
```

Time complexity:
O(2N)