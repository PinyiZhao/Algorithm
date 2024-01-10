# Algorithm
notes of algorithm and data structure

#interval problem

#backtracking
1.permutations problem
No need to set a postion variable(int start), but use a boolean[] to memorize data used or not.
  leetcode 46:
  for (int i = 0; i < len; i++) {
        if (!isUsed[i]) {
            isUsed[i] = true;
            temp.add(nums[i]);
            helper(nums, res, temp, isUsed, i);
            isUsed[i] = false;
            temp.remove(temp.size() - 1);
        }
    }
leetcode 47:
Input: nums = [1,1,2]
Output:
[[1,1,2],
 [1,2,1],
 [2,1,1]]

 key step: avoid repetitve number
  if (i != 0 &&  nums[i] == nums[i - 1] && !isUsed[i - 1]) {
      continue;
  }


# 单调栈
take care of :
index or number was push into stack, which must be consistent
