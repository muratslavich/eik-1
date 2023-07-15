# BinarySearch

Итеративно

```
public int rank(int[] nums, int key) { 
        int lo = 0;
        int hi = nums.length-1;          
        while (lo<=hi) { 
            int mid = lo + (hi-lo)/2; 
            if (nums[mid] > target)          hi = mid-1; 
            else if (nums[mid] < target)     lo = mid+1; 
            else return mid; 
        } 
         
        return -1; 
}
```

Recursion

```
public int rank(Key key, int lo, int hi) { 
     if(hi<lo) return lo; 
     int mid = lo + (hi - lo)/2; 
     int cmp = key.compareTo(keys[mid]); 
     if (cmp < 0)          return rank(key, lo, mid-1); 
     else if (cmp > 0)     return rank(key, mid+1, hi); 
     else                  return mid; 
}
```
