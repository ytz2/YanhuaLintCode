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

695. Max Area of Island

// 这道题有个变种， 给一堆数字，找出最多的凑一起的一堆， 遍历的时候记得count++ 就好
// T = O(N), S= O(N)

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

## 210. Course Schedule II

T = O\(V + E\) , v is number of vertices and E is number of edges

There are a total of`n`courses you have to take labelled from`0`to`n - 1`.

Some courses may have`prerequisites`, for example, if `prerequisites[i] = [ai, bi]` this means you must take the course`bi`before the course`ai`.

Given the total number of courses `numCourses`and a list of the`prerequisite`pairs, return the ordering of courses you should take to finish all courses.

If there are many valid answers, return**any**of them. If it is impossible to finish all courses, return**an empty array**.

**Example 1:**

```
Input:
 numCourses = 2, prerequisites = [[1,0]]

Output:
 [0,1]

Explanation:
 There are a total of 2 courses to take. To take course 1 you should have finished course 0. So the correct course order is [0,1].
```

**Example 2:**

```
Input:
 numCourses = 4, prerequisites = [[1,0],[2,0],[3,1],[3,2]]

Output:
 [0,2,1,3]

Explanation:
 There are a total of 4 courses to take. To take course 3 you should have finished both courses 1 and 2. Both courses 1 and 2 should be taken after you finished course 0.
So one correct course order is [0,1,2,3]. Another correct ordering is [0,2,1,3].
```

**Example 3:**

```
Input:
 numCourses = 1, prerequisites = []

Output:
 [0].
```

```cpp
class Solution {
public:
    vector<int> findOrder(int numCourses, vector<vector<int>>& prerequisites) {
        vector<int> result;
        unordered_map<int, unordered_set<int>> graph;
        // build graph;
        for (const auto& each : prerequisites) {
            for (int i = 1; i < each.size(); i++) {
                graph[each[i]].insert(each[i - 1]);
            }
            if (!graph.count(each[0]))
                graph[each[0]] = unordered_set<int>();
        }
        for (int i = 0; i < numCourses; i++) {
            if (!graph.count(i))
                graph[i] = unordered_set<int>();
        }
        // indegrees;
        unordered_map<int, int> indegrees;
        for (const auto& node : graph) {
            for (const auto& child : node.second) {
                indegrees[child]++;
            }
        }

        queue<int> q;
        // topsort
        for (const auto& node : graph) {
            if (!indegrees.count(node.first)) {
                q.push(node.first);
            }
        }

        // BFS
        while(!q.empty()) {
            auto v = q.front();
            q.pop();
            result.push_back(v);
            for (const auto& child : graph[v]) {
                if (indegrees.count(child) && --indegrees[child] == 0) {
                    q.push(child);
                    indegrees.erase(child);
                }
            }
        }

        return numCourses == result.size() ? result : vector<int>();
    }
};


#pragma once
#include <queue>
#include <iostream>
#include <vector>
#include <unordered_set>
#include <unordered_map>

using namespace std;

class Dependency {
public:
    Dependency() = default;

    void test() {
        cout << "DD: dependency" << endl;
        vector<vector<int>> inputNoCycle{
            { 1, 3}, {3, 5}, {4}, { 2, 3}
        };
        bool res = hasCycle(inputNoCycle);
        if (res) {
            cout << "expect to have no cycle but found, fail" << endl;
        }
        else {
            cout << "PASS, expect no cycle and get no cycle" << endl;
        }

        vector<vector<int>> inputCycle{
            { 1, 3}, {3, 5}, {4}, { 2, 3}, {2, 1}, {4, 2}, {3, 1}
        };
        res = hasCycle(inputCycle);
        if (res) {
            cout << "expect to have cycle and found cycle, pass" << endl;
        }
        else {
            cout << "expect to have cycle but found no cycle, fail" << endl;
        }
    }


    bool hasCycle(vector<vector<int>>& input) {
        unordered_map<int, unordered_set<int>> graph;
        // build graph,  {0, 1} : 0 -> 1 or 0 depends on 1
        for (const auto& each : input) {
            if (each.size()  == 2) {
                graph[each[1]].insert(each[0]);
            }
            if (!graph.count(each[0]))
                graph[each[0]] = unordered_set<int>();
        }

        // build indegrees
        unordered_map<int, int> indegrees;
        for (const auto& each : graph) {
            for (const auto child : each.second) {
                indegrees[child]++;
            }
        }

        // seed the q for bfs
        queue<int> q;
        unordered_set<int> visited;
        for (const auto& node : graph) {
            if (!indegrees.count(node.first)) {
                q.push(node.first);
            }
        }

        int count = 0;
        while (!q.empty()) {
            auto k = q.front();
            q.pop();
            count++;
            for (const auto& child : graph[k]) {
                if (indegrees.count(child) && --indegrees[child] == 0) {
                    q.push(child);
                    indegrees.erase(child);
                }
            }
        }
        return count != graph.size();
    }
};
```

## 759. Employee Free Time

We are given a list`schedule`of employees, which represents the working time for each employee.

Each employee has a list of non-overlapping`Intervals`, and these intervals are in sorted order.

Return the list of finite intervals representing**common, positive-length free time**forallemployees, also in sorted order.

\(Even though we are representing`Intervals`in the form`[x, y]`, the objects inside are`Intervals`, not lists or arrays. For example,`schedule[0][0].start = 1`,`schedule[0][0].end = 2`, and`schedule[0][0][0]`is not defined\).  Also, we wouldn't include intervals like \[5, 5\] in our answer, as they have zero length.

**Example 1:**

```
Input:
 schedule = [[[1,2],[5,6]],[[1,3]],[[4,10]]]

Output:
 [[3,4]]

Explanation:
 There are a total of three employees, and all common
free time intervals would be [-inf, 1], [3, 4], [10, inf].
We discard any intervals that contain inf as they aren't finite.
```

**Example 2:**

```
Input:
 schedule = [[[1,3],[6,7]],[[2,4]],[[2,5],[9,12]]]

Output:
 [[5,6],[7,9]]
```

[https://leetcode.com/problems/employee-free-time/](https://leetcode.com/problems/employee-free-time/)

```cpp
/*
// Definition for an Interval.
class Interval {
public:
    int start;
    int end;

    Interval() {}

    Interval(int _start, int _end) {
        start = _start;
        end = _end;
    }
};
*/

class Solution {
public:
    vector<Interval> employeeFreeTime(vector<vector<Interval>> schedule) {
        auto sortFunc = [&schedule](const pair<int, int>& l, const pair<int, int>& r) {
            return schedule[l.first][l.second].start  > schedule[r.first][r.second].start;
        };
        priority_queue<pair<int, int>, vector<pair<int, int>>, decltype(sortFunc)> pq(sortFunc);
        for (int i = 0; i < schedule.size(); i++) {
            if (schedule[i].empty()) continue;
            pq.push(make_pair(i, 0));
        }
        vector<Interval> result;
        if (pq.empty())
            return result;
        auto head = pq.top();
        Interval interval = schedule[head.first][head.second];
        while(!pq.empty()) {
            auto pos = pq.top();
            pq.pop();
            const auto& head = schedule[pos.first][pos.second];
            if (head.start > interval.end) {
                result.push_back(Interval(interval.end, head.start));
                interval = head;
            } else if (head.start <= interval.end) {
                interval.end = max(interval.end, head.end);
            }
            if (pos.second != schedule[pos.first].size() - 1) {
                pos.second++;
                pq.push(pos);
            }
        }
        return result;
    }
};//
```

```
#pragma once
#include <algorithm>
#include <iostream>
#include <vector>

using namespace std;
class Meeting {
public :
	Meeting() = default;
	~Meeting() = default;
	void test() {
		vector<vector<int>> input{
			{3, 20}, {-2, 0}, {0, 2}, {16, 17}, {19, 23}, {30,40}, {27, 32}
		};
		auto res = findSlots(input, -5, 27, 2);
		cout << "input: -5, 27, 2" << endl;
		print(res);
		res = findSlots(input, -5, 50, 2);
		cout << "input: -5, 27, 2" << endl;
		print(res);
		res = findSlots(input, 10, 27, 2);
		cout << "input: 10, 27, 2" << endl;
		print(res);
		res = findSlots(input, 1-5, 0, 6);
		cout << "input: -5, 0, 6" << endl;
		print(res);
	}
	void print(vector<vector<int>>& input) {
		for (auto& each : input) {
			cout << each[0] << "-->" << each[1] << endl;
		}
		cout << endl;
	}

	vector<vector<int>> findSlots(vector<vector<int>> input, int from, int to, int minLen) {
		vector<vector<int>> result;
		int last_end = from;
		sort(input.begin(), input.end(), [](const vector<int>& l, const vector<int>& r) {
			return l[0] < r[0];
		});
		for (const auto& each : input) {
			if ( min(to, each[0]) - last_end >= minLen) {
				result.push_back({ last_end, min(each[0], to) });
			}
			last_end = max(last_end, each[1]);
		}
		if (to - last_end >= minLen) {
			result.push_back({last_end, to});
		}
		return result;
	}

	vector<vector<int>>  findSlots2(vector<vector<int>> input, int from, int to, int minLen) {
		vector<vector<int>> result;
		vector<pair<int, bool>> timelines;
		for (const auto& each : input) {
			if (each.size() != 2 || each[0] >= each[1]) // invalid input
				continue;
			if (each[1] < from) continue;
			if (each[0] > to) continue;
			timelines.emplace_back( each[0] < from ? from : each[0] , true);
			timelines.emplace_back( each[1] > to ? to : each[1] , false);
		}
		sort(timelines.begin(), timelines.end(), [](const pair<int, bool>& l, const pair<int, bool>& r) {
			if (l.first == r.first) {
				return l.second;
			}
			return l.first < r.first;
		});

		int count = 0;
		int start = from;
		for (const auto& each : timelines) {
			if (each.second) {
				if (count == 0 && each.first - start >= minLen) {
					result.push_back({ start, each.first });
				}
				count++;
			}
			else {
				count--;
				if (count == 0) {
					start = each.first;
				}
			}
		}
		if (to - start  >= minLen) {
			result.push_back({ start, to });
		}
		return result;
	}
};
```


