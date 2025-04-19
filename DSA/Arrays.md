## Easy

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