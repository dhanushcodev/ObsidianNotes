
#### Selection Sort

selecting and keeping it in order
filling the index in order with correct value

- find the min element in the array range 0 - n
- swap the min element with index 0 element
- now the index 0 is sorted
- now find the min element in range 1 - n
- swap the min element with index 1 element
- now index 0 and 1 are sorted
- continue the steps...

pseudo code:

```pseudo
selectionSort(arr,n)
{
	for i <- 0 to n-2
	{
		imin <-  i //store the index of min element
		for j <- i+1 to n-1
		{
			if(arr[j] < arr[imin])
				imin <- j             //logic to find the min element
		}
		temp <- arr[i]              //swap logic
		arr[i] <- arr[imin]
		arr[imin] <- temp
	}
}
```

code:
```cpp
void SelectionSort(int A[],int n){
	for(int i = 0; i<n-1; i++){ // we need to n-2 passes
		int iMin = i; 
		for(int j = i+1; j<n; j++){ // ith positon: elements from i till n-1
			if(A[j] < A[iMin])
				iMin = j; // update the index of minimum element
		}
		int temp = A[i];  //swap logic
		A[i] = A[iMin];
		A[iMin] = temp;
	}
}
```

Time complexity:
O(n2)

#### Bubble Sort

push the large element to right most side

eg:
2 7 4 1 5 3
steps:
- check 2 7 is sorted i.e index 0, 1 (yes it is)
- check 7 4 is sorted i.e index 1, 2 (no) swap it
- 2 4 7 1 5 3
- check 7 1 is sorted i.e index 2, 3 (no ) swap it
- 2 4 1 7 5 3
- check 7 5 sorted i.e index 3, 4 (no) swap it 
- 2 4 1 5 7 3
- check 7 3 sorted i.e index 4, 5 (no) swap it
- 2 4 1 5 3 7 now 7 is sorted i.e index 5 is sorted
- repeat the steps for all

pseudo code:
```pseudo
for k <- 1 to n-1
{
	for i <- 0 to n-2
	{
		if(arr[i] > arr[i+1]){
			swap(arr[i],arr[i+1])
		}
	}
}
```

Time complexity:
O(n2)

optimized solution:

```pseudo
for k <- 1 to n-1
{
	didswap = 0    // flag to check wether the swap has taken place
	for i <- 0 to n-2
	{
		if(arr[i] > arr[i+1]){
			swap(arr[i],arr[i+1])
			didswap = 1
		}
	}
	if didswap == 0 break;
}
```

if there is no swap that means its already sorted

Time complexity:
best: O(n)
worst: O(n2)

#### Insertion Sort

From left to right check whether the element is in correct position.

ex : 7 2 4 1 5 3

steps: 
- first consider array till index 0,  [7] only one element. its already in correct place.
- now consider array till index 1, [7 2] is 2 is in correct place ? no swap 2 till it is in correct place
- 2 7 4 1 5 3
- now consider array till index 2, [2 7 4] is 4 in correct place? no swap 7 and 4
- 2 4 7 1 5 3
- now consider array till index 3, [2, 4, 7, 1] is 1 in correct place? no swap 1 till its in its correct place
- 2 , 4, 1, 7, 5, 3  is 1 in correct place? no swap
- 2, 1, 4, 7, 5, 3 no swap
- 1, 2, 4, 7, 5, 3 
- now consider array till index 4, [1,2,4,7,5] is 5 is in correct place? no swap until its in correct place
- 1,2,4,5,7,3
- now consider array till index 5, [1,2,4,5,7,3] in 3 in correct place? no swap until it is
- 1,2,3,4,5,7

pseudo code:
```pseudo
InsertionSort(arr,n){
	for i <- 1 to n-1
	{
		j <- i
		while( j > 0 && arr[j-1] > arr[j])
		{
			swap(arr[j],arr[j-1])
			j <- j-1
		}
	}
}
```

code:
```cpp
void insertionSort(int arr[], int n){
	for(int i = 1;i<n;i++){
		int j = i;
		while(j>0 && arr[j-1] > arr[j]){
			int temp = arr[j-1];
			arr[j-1] = arr[j];
			arr[j] = temp;
			
			j--;
		}
	}
}
```

Time complexity:
worst: O(n2)
average: O(n2)
Best: O(n)