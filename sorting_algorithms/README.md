## Merge Sort

### Intuition
Merge sort is a classic divide-and-conquer sorting algorithm:
- **Divide**: Recursively split the array into two halves until subarrays of size 1.
- **Conquer**: Recursively sort each half (base case is size 1, already sorted).
- **Combine (Merge)**: Merge two sorted halves into a single sorted array by repeatedly choosing the smaller front element.

Key properties:
- **Stable**: Equal elements preserve input order.
- **Time complexity**: O(n log n) in all cases (best/avg/worst).
- **Space complexity**: O(n) extra space for the merge buffer (can be reduced with in-place techniques, but those are complex and usually slower in practice).

### Implementation (C++)
The following implementation is based on your snippet with small improvements:
- Pre-allocate the temporary buffer with `reserve` to reduce re-allocations.
- Use two simple tail-copy loops after the main merge loop (clearer and slightly faster than nested conditionals with OR).
- Early-exit optimization: if the largest element of the left half is `<=` the smallest of the right half, skip the merge.

```cpp
void merge(vector<int>& arr, int l, int m, int r) {
    vector<int> sorted;
    sorted.reserve(r - l + 1);

    int left = l;
    int right = m + 1;

    while (left <= m && right <= r) {
        if (arr[left] <= arr[right]) {
            sorted.push_back(arr[left++]);
        } else {
            sorted.push_back(arr[right++]);
        }
    }

    while (left <= m) {
        sorted.push_back(arr[left++]);
    }
    while (right <= r) {
        sorted.push_back(arr[right++]);
    }

    for (int k = 0; k < (int)sorted.size(); ++k) {
        arr[l + k] = sorted[k];
    }
}

void mergeSort(vector<int>& arr, int l, int r) {
    if (l >= r) return;

    int m = l + (r - l) / 2;
    mergeSort(arr, l, m);
    mergeSort(arr, m + 1, r);

    // Optimization: already ordered across the boundary
    if (arr[m] <= arr[m + 1]) return;

    merge(arr, l, m, r);
}

// Convenience wrapper
void mergeSort(vector<int>& arr) {
    if (!arr.empty()) {
        mergeSort(arr, 0, (int)arr.size() - 1);
    }
}
```
### Inversion Problem:

```cpp
int inversions = 0;
    void merge(vector<int>& arr,int left,int right){
        int m = left + (right-left)/2;
        int l = left;
        int r = m+1;
        
        vector<int>temp;
        while (l <= m && r <= right){
            if (arr[l] <= arr[r])
                temp.push_back(arr[l++]);
            else{
                temp.push_back(arr[r++]);
                inversions += (m-l+1);
            }
        }
        
        while (l <= m) temp.push_back(arr[l++]);
        while (r <= right) temp.push_back(arr[r++]);
        
        for (int i=0;i<temp.size();i++)
            arr[left + i] = temp[i];
    }
```
### Practice Problems:
- [Count Inversions Of Array](https://www.geeksforgeeks.org/problems/inversion-of-array-1587115620/1)

```CPP
int findUnsortedSubarray(vector<int>& nums) {
        vector<int>temp(nums.begin(),nums.end());
        sort(temp.begin(),temp.end());
        int start = -1, end = -1;
        for (int i=0;i<nums.size();i++){
            if (nums[i] != temp[i]){
                start = i;
                break;
            }
        }

        if (start == -1) return 0;

        for (int i=nums.size()-1;i>=0;i--){
            if (nums[i] != temp[i]){
                end = i;
                break;
            }
        }
        return (end - start + 1);
    }
```
- [Shortest Unsorted Continous Subarray](https://leetcode.com/problems/shortest-unsorted-continuous-subarray/)
