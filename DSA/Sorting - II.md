
#### Merge Sort

Merge sort is a divide and conquer algorithm:
1. Divides the array into halves recursively.
2. Sorts each half.
3. Merges the sorted halves into one sorted array.

Example: `[3,1,2,4,1,5,2,6,4]`

Divide:
```
[3,1,2,4,1,5,2,6,4]

-> [3,1,2,4,1]     ||    [5,2,6,4]
 
-> [3,1,2]   [4,1]    ||   [5,2]   [6,4]

-> [3,1][2]  [4][1]  ||   [5][2]  [6][4]

-> [3][1]   ||

now sort and merge:

-> [3][1]       ||

-> [3,1][2]  [4][1]  ||   [5][2]  [6][4] 

-> [1,2,3]    [1,4]    ||  [2,5]   [4,6]

-> [1,1,2,3,4]     ||    [2,4,5,6]

-> [1,1,2,2,3,4,4,5,6]

```

pseudo code:

```pseudo code
mergeSort(arr,low,high){
	if (low == high) return  //stop when there is single element
	mid = (low+high)/2
	mergeSort(arr,low,mid)
	mergeSort(arr,mid+1,high)
	merge(arr,low,mid,high)
}
merge(arr,low,mid,high){
	temp -> []       // create a temp variable
	left = low       // first index of left array
	right = mid+1    // first index of right array
	
	while(left <= mid && right <= high){  // iterate through the index of both array
		if(arr[left] <= arr[right]){  //compare elements of left and right array
			temp.add(arr[left])   //sorted temp array
			left++
		}
		else{
			temp.add(arr[right])   //sorted temp array
			right++
		}
	}
	while(left <= mid){   // if remaining left elements can be directly added as it will be sorted
		temp.add(arr[left])
		left++
	}
	while(right <= high){   // if remaining right elements can be directly added as it will be sorted
		temp.add(arr[right])
		right++
	}
	for(i = low -> high){  // transfer from temp array to main array
		arr[i] = temp[i-low]
	}
}
```

code:
```java
public static void mergeSort(int[] arr, int left, int right){  
    if(left==right) return;  
    int mid = (left + right)/2;  
    mergeSort(arr,left,mid);  
    mergeSort(arr,mid+1,right);  
    merge(arr,left,mid,right);  
}  
  
public static void merge(int[] arr, int low, int mid ,int high){  
    int[] temp = new int[high-low+1];  
    int left = low;  
    int right = mid+1;  
    int tempIndex = 0;  
  
    while(left <= mid && right <=high){  
        if(arr[left] <= arr[right]){  
            temp[tempIndex++] = arr[left];  
            left++;  
        }else{  
            temp[tempIndex++] = arr[right];  
            right++;  
        }  
    }  
    while(left <= mid){  
        temp[tempIndex++] = arr[left++];  
    }  
    while(right <= high){  
        temp[tempIndex++] = arr[right++];  
    }  
    for(int i=low;i<=high;i++){  
        arr[i] = temp[i-low];  
    }  
}
```

Time complexity:
O(n log n)
Space complexity:
O(n)


#### Quick Sort

Quick sort is a highly efficient, comparison-based, divide-and-conquer sorting algorithm.
1. **Choose a pivot** element (commonly first, last, middle, or random).
2. **Partition** the array so that:
    - All elements < pivot, go to the left.    
    - All elements > pivot, go to the right.
3. **Recursively apply** the above steps to the left and right subarrays.
4. The base case is when the array has 0 or 1 element (already sorted).

Example: `[4,6,2,5,7]`

explaination:
```
[4,6,2,5,7,1]
lets choose pivot = 4
-> partition: [2,1] |4| [6,5,7]
recur on next
[2,1] choose pivot = 2  [6,5,7] choose pivot = 6
partition: [1] |2|  |4|    partition: [5] |6| [7]
sorted array [1,2,4,5,6,7]
```

pseudo code:
```pseudo code
quickSort(arr,low,high){
	if(low<high){ // we sort the array only it has more than one element
		pindex = partition(arr,low,high) // place pivot at correct place and return its index
		quickSort(arr,low,pindex-1)
		quickSort(arr,pindex+1,high)
	}
}

int partition(arr,low,high){
	pivot = arr[low] // first element is considered as pivot
	// two pointers for start and end
	i=low // firt element
	j=high // last element

	while(i<j){ // till i and j cross each other
		while(arr[i] <= pivot && i<high) i++ // check if start index element is less that pivot i.e left side is less that pivot then move left pointer to next element. stop if we get element greater than pivot in left side.
		while(arr[j] > pivot && j>low) j-- // check if right side element are greater than pivot i.e stop if we get element less that pivot in right side.
		if(i<j) swap(arr[i],arr[j]) // now we have left and right index which has opposite values ie left has large and right has small so exchange it, check if either i or j is not in wrong area, i.e if j crosses i it means right side area is all correct i has all large elements already now j its in left hand area so its small than pivot. viseversa
	}
	swap(arr[low],arr[j]) // when i or j is crossed it means everything is correct but the pivot is still at first index, now it need to be placed in correct place.
	why switch pivot with j index? because as the pivot we chose was first element i.e left hand side, in left hand side all elements should be smaller, so now j crossed i and its is in smaller side region so we need to swap it with j index element, if we had chosen right most element then we would have chosen i to swap with.
	return j
}
```

other method for partition logic:
```
partition(arr,start,end){
	pivot <- arr[end]
	pIndex <- start
	for i <- start to end-1
	{
		if(arr[i] <= pivot){
			swap(arr[i],arr[pIndex])
			pIndex++
		}
	}
	swap(arr[end],arr[pIndex])
	return pIndex
}
```

imaginary run:
```
pivot 4
[4,6,2,5,7,1]
   i       j
[4,1,2,5,7,6]
     j i 
i j crossed
so swap 4 with j
[2,1,4,5,7,6]
     p
now continue recur for two partitioned array
```

code:
```java
public static void quickSort(int[] arr,int start,int end){  
    if(start>=end) return;  
    int pIndex = partition(arr,start,end);  
    quickSort(arr,start,pIndex-1);  
    quickSort(arr,pIndex+1,end);  
}  
  
public static int partition(int[] arr,int start,int end){  
    int pivot = arr[start];  
    int i = start;  
    int j = end;  
  
    while(i<j){  
        while(arr[i]<=pivot && i<end) i++;  // i < end because j is already at end
        while(arr[j]>pivot && j>start) j--;  // j > start because i is already at start
        if(i<j) swap(arr,i,j);  
    }  
    swap(arr,start,j);  
    return j;  
}  
  
public static void swap(int[] arr,int i,int j){  
    int temp = arr[i];  
    arr[i] = arr[j];  
    arr[j] = temp;  
}
```

Time Complexity:
O(N log N)
Space Complexity:
O(1)