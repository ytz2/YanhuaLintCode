## 200. Number of Islands



Given an`m x n`2d`grid`map of`'1'`s \(land\) and`'0'`s \(water\), return_the number of islands_.

An**island**is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.



**Example 1:**

```
Input:
 grid = [
  ["1","1","1","1","0"],
  ["1","1","0","1","0"],
  ["1","1","0","0","0"],
  ["0","0","0","0","0"]
]

Output:
 1

```

**Example 2:**

```
Input:
 grid = [
  ["1","1","0","0","0"],
  ["1","1","0","0","0"],
  ["0","0","1","0","0"],
  ["0","0","0","1","1"]
]

Output:
 3
```

```cpp
class Solution {
public:
    int numIslands(vector<vector<char>>& grid) {
        if (grid.empty() || grid[0].empty())
            return 0;
        int m = grid.size(), n = grid[0].size();
        queue<pair<int /*x*/, int /*y*/>> q;
        static vector<int> dx{-1, 1, 0, 0};
        static vector<int> dy{0, 0, -1, 1};
        int count = 0;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (grid[i][j] == '0')
                    continue;
                count++;
                q.emplace(i, j);
                grid[i][j] = '0';
                while(!q.empty()) {
                    auto pos = q.front();
                    q.pop();
                    for (int k = 0; k < 4; k++) {
                        auto nx = pos.first + dx[k];
                        auto ny = pos.second + dy[k];
                        if (nx < 0 || nx >= m || ny < 0 || ny >= n || grid[nx][ny] == '0')
                            continue;
                        grid[nx][ny] = '0';
                        q.emplace(nx, ny);
                    }
                }
            }
        }
        return count;
    }
};

// 这道题有个变种， 给一堆数字，找出最多的凑一起的一堆， 遍历的时候记得count++ 就好

#pragma once
#include <iostream>
#include <queue>
#include <vector>
#include <cassert>
using namespace std;

class Connected {
public:
	Connected() = default;
	~Connected() = default;
	void test() {
		vector<vector<int>> input{
			{1, 1, 2, 2, 3},
			{1, 1, 0, 3, 3},
			{2, 4, 2, 1, 1},
			{3, 2, 1, 4, 3}
		};
		int exp = 4;
		auto res = connected(input);
		assert(res == exp);
		cout << "Connected pass" << endl;
	}

	int connected(vector<vector<int>>& input) {
		int m = input.size();
		int n = input[0].size();
		int res = 0;
		static vector<int> dx{ -1, 1, 0, 0 };
		static vector<int> dy{ 0, 0, -1, 1 };
		vector<vector<bool>> visited(m, vector<bool>(n, false));
		for (int i = 0; i < m; i++) {
			for (int j = 0; j < n; j++) {
				if (visited[i][j])
					continue;
				int count = 0;
				queue<pair<int,int>> q;
				q.emplace(i, j);
				int v = input[i][j];
				visited[i][j] = true;
				while (!q.empty()) {
					count++;
					int x = q.front().first;
					int y = q.front().second;
					q.pop();
					for (int k = 0; k < 4; k++) {
						auto nx = x + dx[k];
						auto ny = y + dy[k];
						if (nx < 0 || nx >= m || ny < 0 || ny >= n || visited[nx][ny] || input[nx][ny] != v )
							continue;
						visited[nx][ny] = true;
						q.emplace(nx, ny);
					}
				}
				res = max(res, count);
			}
		}
		return res;
	}
};
```

## 106. Closest Binary Search Tree Value

Given inorder and postorder traversal of a tree, construct the binary tree.

