### 출처 - https://www.acmicpc.net/problem/2468
## 사용언어 - C++
## 해설 - https://velog.io/@estelle17/%EB%B0%B1%EC%A4%80-2468%EB%B2%88-%EC%95%88%EC%A0%84-%EC%98%81%EC%97%AD

```C++17
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

void checkSafePlace(vector<vector<int>> &map, int x, int y, int depth)
{
	int dir[2][4] = {{1, -1, 0, 0},
		      {0, 0, 1, -1}};
	map[x][y] = -1;
	for (int i = 0; i < 4; i++)
	{
		if (x + dir[0][i] < 0 || x + dir[0][i] >= map.size() || y + dir[1][i] < 0 || y + dir[1][i] >= map.size())
			continue;

		if (map[x + dir[0][i]][y + dir[1][i]] > depth)
		{
			checkSafePlace(map, x + dir[0][i], y + dir[1][i], depth);
		}
	}
}

int main()
{
	int n, answer;
	int min = 101, max = 0;
	vector<vector<int>> map, checkMap;
	cin >> n;

	map.assign(n, vector<int>(n, 0));

	for (int i = 0; i < n; i++)
	{
		for (int j = 0; j < n; j++)
		{
			cin >> map[i][j];
			if (map[i][j] < min)	min = map[i][j];
			if (map[i][j] > max)	max = map[i][j];
		}
	}

	answer = 0;
	int cnt = 0;

	for (int depth = min - 1; depth < max + 1; depth++)
	{
		checkMap = map;
		for (int i = 0; i < n; i++)
		{
			for (int j = 0; j < n; j++)
			{
				if (checkMap[i][j] > depth)
				{
					checkSafePlace(checkMap, i, j, depth);
					cnt++;
				}
			}
		}
		if (answer < cnt)	answer = cnt;

		cnt = 0;
	}

	cout << answer;
}
```