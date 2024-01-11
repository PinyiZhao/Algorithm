# Algorithm
notes of algorithm and data structure

#interval problem

# backtracking
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

# DFS
huawei test:
假设有M*N个式子水管组成矩阵，水管有三种情况，矩阵0表示水管堵死不能流出流入，1表示水管中有干净的水，2表示水管中有污水，污水会流向上下左右的水管，污染干净的水管，除非遇到堵死的水管则不能流动。
驾驶至少一根水管中已经有污水，我们仅有一个开关， 可以关闭任意一个干净的水管，已经污染的水管不能关闭来阻断污染扩散。请规划醉驾的水管关闭，是的最终污染的水管最少，输出最终污染的水管数

public class WaterPipes {
    private static final int BLOCKED = 0;
    private static final int CLEAN = 1;
    private static final int POLLUTED = 2;

    public static void main(String[] args) {
        int[][] matrix = {
            {1, 2, 1},
            {1, 1, 1},
            {2, 1, 1}
        };
        
        System.out.println("Minimum polluted pipes: " + findMinPolluted(matrix));
    }

    private static int findMinPolluted(int[][] matrix) {
        int minPolluted = Integer.MAX_VALUE;
        for (int i = 0; i < matrix.length; i++) {
            for (int j = 0; j < matrix[i].length; j++) {
                if (matrix[i][j] == CLEAN) {
                    int[][] copy = deepCopy(matrix);
                    copy[i][j] = BLOCKED;
                    spreadPollution(copy);
                    minPolluted = Math.min(minPolluted, countPolluted(copy));
                }
            }
        }
        return minPolluted;
    }

    private static void spreadPollution(int[][] matrix) {
        boolean[][] visited = new boolean[matrix.length][matrix[0].length];
        for (int i = 0; i < matrix.length; i++) {
            for (int j = 0; j < matrix[i].length; j++) {
                if (matrix[i][j] == POLLUTED && !visited[i][j]) {
                    dfs(matrix, i, j, visited);
                }
            }
        }
    }

    private static void dfs(int[][] matrix, int i, int j, boolean[][] visited) {
        if (i < 0 || i >= matrix.length || j < 0 || j >= matrix[0].length || matrix[i][j] == BLOCKED || visited[i][j]) {
            return;
        }
        visited[i][j] = true;
        if (matrix[i][j] == CLEAN) {
            matrix[i][j] = POLLUTED;
        }
        dfs(matrix, i + 1, j, visited);
        dfs(matrix, i - 1, j, visited);
        dfs(matrix, i, j + 1, visited);
        dfs(matrix, i, j - 1, visited);
    }

    private static int countPolluted(int[][] matrix) {
        int count = 0;
        for (int[] row : matrix) {
            for (int cell : row) {
                if (cell == POLLUTED) {
                    count++;
                }
            }
        }
        return count;
    }

    private static int[][] deepCopy(int[][] original) {
        int[][] copy = new int[original.length][];
        for (int i = 0; i < original.length; i++) {
            copy[i] = Arrays.copyOf(original[i], original[i].length);
        }
        return copy;
    }
}

leetcode: 79 word search
class Solution {
    int m;
    int n;
    int[][] directions = new int[][]{{1,0},{-1,0},{0,1},{0,-1}};
    public boolean exist(char[][] board, String word) {
        m = board.length;
        n = board[0].length;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if(isWordExist(board, word, i, j, 0, new boolean[m][n])) {
                    return true;
                }
            }
        }
        return false;
    }
    private boolean isWordExist(char[][] board, String word, int i, int j, int pos, boolean[][] isUsed) {
        if (pos == word.length() - 1 && word.charAt(pos) == board[i][j]) {
            return true;
        }
        if (word.charAt(pos) != board[i][j]) {
            return false;
        }
        isUsed[i][j] = true;
        for (int[] dir : directions) {
            int row = i + dir[0];
            int col = j + dir[1];
            
            if (row >= 0 && row < m && col >= 0 && col < n) {
                if (!isUsed[row][col]) {
                    boolean isRestOK = isWordExist(board, word, row, col, pos + 1, isUsed);
                    if (isRestOK) {
                        return true;
                    }
                }
            }
            
        }
        isUsed[i][j] = false;
        return false;
    }
}
