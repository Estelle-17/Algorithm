### 출처 - https://www.acmicpc.net/problem/2579
## 사용언어 - C++
## 해설 - https://velog.io/@estelle17/%EB%B0%B1%EC%A4%80-2579%EB%B2%88-%EA%B3%84%EB%8B%A8-%EC%98%A4%EB%A5%B4%EA%B8%B0

```C++17
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

int n;
vector<vector<int>> dp(2, vector<int>(300, 0));
vector<int> stairs(300, 0);

void solve(int currentScore, int height, int continueStep)
{
	if (continueStep >= 2 || height > n - 1)
		return;

	currentScore += stairs[height];

	if (dp[continueStep][height] < currentScore)
		dp[continueStep][height] = currentScore;
	else
		return;

	solve(currentScore, height + 1, continueStep + 1);
	solve(currentScore, height + 2, 0);
}

int main()
{
	cin >> n;

	for (int i = 0; i < n; i++)
	{
		cin >> stairs[i];
	}
	
	solve(0, 0, 0);
	solve(0, 1, 0);

	if(dp[0][n-1] > dp[1][n - 1])
		cout << dp[0][n - 1];
	else
		cout << dp[1][n - 1];
}
```