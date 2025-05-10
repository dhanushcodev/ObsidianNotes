
##### Two sum
return the indexes of two numbers  which leads to target sum
eg: arr = [2,6,5,8,11]  target=14

Brute:
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

Better:
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

Optimal:
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


#### Sort an arrays of 0's 1's and 2's

brute:
```
use sort functions
```

Time complexity: 
Nlog(N)
Space complexity:
N

Better:
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

Optimal:
explaination: [[Sort an arrays of 0's 1's and 2's]]
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

