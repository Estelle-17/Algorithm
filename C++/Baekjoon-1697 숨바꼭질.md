### 출처 - https://www.acmicpc.net/problem/1697
## 사용언어 - C++
## 해설 - https://velog.io/@estelle17/%EB%B0%B1%EC%A4%80-%EB%AC%B8%EC%A0%9C-1697%EB%B2%88-%EC%88%A8%EB%B0%94%EA%BC%AD%EC%A7%88

```C++17
#include <iostream>
#include <vector>
#include <queue>
#include <algorithm>

using namespace std;

int main()
{
	int n, k;
	int answer = 0;
	vector<int> checkNum(100001, 0);

	cin >> n >> k;

	queue<int> currentNum;

	currentNum.push(k);

	while (!currentNum.empty())
	{
		int qSize = currentNum.size();

		for (int i = 0; i < qSize; i++)
		{
			int num = currentNum.front();	currentNum.pop();

			if (num == n)
			{
				answer--;
				currentNum = queue<int>();
				break;
			}

			if (checkNum[num] == 0 && num >= 0)
			{
				checkNum[num] = 1;

				if (num % 2 == 0)
				{
					currentNum.push(num / 2);
				}
				currentNum.push(num - 1);
				currentNum.push(num + 1);
			}
		}
		answer++;
	}

	cout << answer;
}
```