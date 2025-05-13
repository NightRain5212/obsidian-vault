- 与暴力匹配算法不同的是，kmp算法不会回退指向主串的指针，减少冗余匹配，建立部分匹配表(next数组)，不匹配时将指向模式串的指针回退到上一个匹配位置之后，减少冗余匹配。



```cpp
//求next数组(s[0,1,...,i]的最长公共前后缀长度)
void getNext(string& s,vector<int>& next)
{
	for(int i=1;i<next.size();i++) {
		//j指向前一个匹配的最小前缀的后一项
		int j=next[i-1];
		//不匹配时则继续更新j，使得j指向上一个匹配最小前缀的后一项，直到j=0
		while(j>0 && s[i]!=s[j]) j = next[j-1];
		//如果该位置的字符与前缀的后一个字符匹配，则next[i]=next[j]+1;
		if(s[i]==s[j]) j++;
		next[i]=j;
	}
}

int main()
{
	string s1,s2;
	cin>>s1>>s2;
	int n1=s1.size();
	int n2=s2.size();
	vector<int> next(n2);
	getNext(s2,next);
	int p1=0;int p2=0;int ans=-1;
	while(p1<n1)
	{
		//字符匹配，两个指针均前进
		if(s1[p1]==s2[p2]) {
			p1++;p2++; 
		}
		//字符不匹配
		else {
			//更新p2，指向更小的匹配位置,next数组
			if(p2>0) p2=next[p2-1];
			//否则p1前进
			else p1++;
		}
		//查找到子串
		if(p2==n2) {
			//存储子串第一个字符的位置
			ans=p1-p2;
			break;
		}
	}
	cout<<ans;
	return 0;
}

```