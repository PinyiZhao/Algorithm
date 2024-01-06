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



