# 300. Longest Increasing Subsequence

[Problem Link](https://www.geeksforgeeks.org/problems/maximum-sum-increasing-subsequence4749/1)


## Recurssive Approach - Top Down Approach

1. The main idea of this problem is to find the maximum sum of Increasing Subsequences
2. [5, 3, 9, 11, 2, 6, 19] => LIS = [ 5, 9, 11, 19] -- 44
3. As this is a subsequence, we have two options, take and not take
4. When we want to take, check the option whether the current number is greater than previous LIS nubmer. Increase answer by current Index sum
5. Else, dont take that index
6. return longest increasing subsequence length

```Java

class Solution {
  public:
    
    int helper(int ind, int prev, vector<int>& arr) {
        if (ind == arr.size()) return 0;
        
        int take = 0;
        int not_take = 0;
        
        if (prev == -1 || arr[ind] > arr[prev]) {
            take = arr[ind] + helper(ind + 1, ind, arr);
        }
        
        not_take = helper(ind+1, prev, arr);
        
        return max(take, not_take);
    }
    
    int maxSumIS(vector<int>& arr) {
        // code here
        return helper(0, -1, arr);
    }
};


```

## Complexity Analysis

### Time complexity: O(2^N) exponential

### Space Compleixty: O(N)


## Memoization - Top down Approach

```Java

class Solution {
  public:
    
    int helper(int ind, int prev, vector<int>& arr, vector<vector<int>> &dp) {
        if (ind == arr.size()) return 0;
        
        int take = 0;
        int not_take = 0;
        
        if(dp[ind][prev+1] != -1) {
            return dp[ind][prev+1];
        }
        
        if (prev == -1 || arr[ind] > arr[prev]) {
            take = arr[ind] + helper(ind + 1, ind, arr, dp);
        }
        
        not_take = helper(ind+1, prev, arr, dp);
        
        return dp[ind][prev+1] = max(take, not_take);
    }
    
    int maxSumIS(vector<int>& arr) {
        // code here
        int n= arr.size();
            
        vector<vector<int>> dp(n, vector<int> (n+1, -1));
        
        return helper(0, -1, arr, dp);
    }
};

```

## Complexity Analysis

### Time complexity: O(N*N)

### Space Compleixty: O(N*N) + O(N)


## Tabulation - bottom up approach

```Java

class Solution {
    
    public int lengthOfLIS(int[] nums) {
        
        int n = nums.length;
        int dp[][] = new int[n+1][n+1];
        
        for(int ind = n-1; ind>=0; ind--) {
            for(int prev = ind-1; prev >= -1; prev--) {
                
                 int notTake = dp[ind+1][prev + 1];
                
                int take = 0;
                
                if(prev == -1 || nums[ind] > nums[prev])
                    take = 1 + dp[ind+1][ind + 1];
                
                dp[ind][prev+1] = Math.max(notTake, take);
            }
        }
        
        return dp[0][0];
    }
}

```

## Complexity Analysis

### Time complexity: O(N*N)

### Space Compleixty: O(N*N) 


## Space Optimization

```Java

class Solution {
    
    public int lengthOfLIS(int[] nums) {
        
        int n = nums.length;
        int curr[] = new int[n+1];
        int after[] = new int[n+1];
        
        
        for(int ind = n-1; ind>=0; ind--) {
            for(int prev = ind-1; prev >= -1; prev--) {
                
                 int notTake = after[prev + 1];
                
                int take = 0;
                
                if(prev == -1 || nums[ind] > nums[prev])
                    take = 1 + after[ind + 1];
                
                curr[prev+1] = Math.max(notTake, take);
            }
            
            after = curr;
        }
        
        return curr[0];
    }
}


```


## Complexity Analysis

### Time complexity: O(N*N)

### Space Compleixty: O(N) 


## Optimized Solution

```Java

class Solution {
    public int lengthOfLIS(int[] nums) {
        int n = nums.length;
        int dp[] = new int[n];
        int ans = 1;
        
        Arrays.fill(dp, 1);
        
        for(int ind=0; ind<n; ind++) {
            for(int prev = 0; prev < ind; prev++) {
                if(nums[prev] < nums[ind]) {
                    dp[ind] = Math.max(dp[ind], 1 + dp[prev]);
                    ans = Math.max(ans, dp[ind]);
                }
            }
        }
        
        return ans;
    }
}

```

## Complexity Analysis

### Time complexity: O(N*N)

### Space Compleixty: O(N) 
