### 출처 - https://www.acmicpc.net/problem/1931
## 사용언어 - C++
## 해설 - https://velog.io/@estelle17/%EB%B0%B1%EC%A4%80-1931%EB%B2%88-%ED%9A%8C%EC%9D%98%EC%8B%A4-%EB%B0%B0%EC%A0%95

```C++17
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

bool compare(vector<int> a, vector<int> b)
{
	if (a[1] < b[1])
		return true;
	else if (a[1] == b[1])
		return a[0] < b[0];
	
	return false;
}

int main()
{
	int n, answer;
	vector<vector<int>> meetings;

	cin >> n;
	meetings.assign(n, vector<int>(2, 0));

	for (int i = 0; i < n; i++)
	{
		cin >> meetings[i][0] >> meetings[i][1];
	}

	sort(meetings.begin(), meetings.end(), compare);

	answer = 0;

	int currentTime = 0;

	for (int i = 0; i < n; i++)
	{
		if (currentTime <= meetings[i][0])
		{
			currentTime = meetings[i][1];
			answer++;
		}
	}

	cout << answer;
}
```