### 출처 - https://www.acmicpc.net/problem/1920
## 사용언어 - C++
## 해설 - 

```C++17
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

int n, m;
vector<int> v;

int solve(long long int num)
{
	int high = n - 1;
	int low = 0;

	while (low <= high)
	{
		int mid = (high + low) / 2;

		if (v[mid] == num)
			return 1;

		if (v[mid] > num)
		{
			high = mid - 1;
		}
		else
		{
			low = mid + 1;			
		}
	}
	return 0;
}

int main()
{
	//입촐력 객체 가속(더 빠르게 출력할 수 있음)
	ios_base::sync_with_stdio(false);
	cin.tie(0);

	long long int t;
	cin >> n;

	for (int i = 0; i < n; i++)
	{
		cin >> t;
		v.push_back(t);
	}

	sort(v.begin(), v.end());

	cin >> m;

	for (int i = 0; i < m; i++)
	{
		cin >> t;
		printf("%d\n", solve(t));
	}
}
```