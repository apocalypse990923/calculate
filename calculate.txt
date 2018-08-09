#include<iostream>

#include<string.h>

#include<string>

#include <sstream>

#include <math.h>

#include<stack>

#include<cstdlib>

#include<algorithm>

#include<cstring>

using namespace std;



string DoubleToString(double Input)

{

    stringstream Oss;

    Oss<<Input;

    return Oss.str();

}

double StringToDouble(string Input)

{

    double Result;

    stringstream Oss;

    Oss<<Input;

    Oss>>Result;

    return Result;

}



bool ifbrace(string s)//true=brace,false=no brace

{

	for(int i=0;i<s.length();i++)

	{

		if(s[i]=='('|| s[i]==')')

		{

			return true;

		}

	}

	return false;

}



int ifmul(string s)//1=*,2=/,0=nothing

{

	for(int i=0;i<s.length();i++)

	{

		if(s[i]=='*')

		{

			return 1;

		}

		else if(s[i]=='/')

		{

			return 2;

		}

	}

	return 0;

}



int ifplus(string s)//1=+,2=-,0=nothing

{

	int flag=0;

	for(int i=0;i<s.length();i++)

	{

		if(s[i]=='+')

		{

			return 1;

		}

		else if(s[i]=='-')

		{

			if(i==0)

			{

				flag++;

				continue;

			}

			if(i!=0 || flag!=0)

			{

				return 2;

			}

		}

	}

	return 0;

}



string calculate_without_brace(string s)

{

	string str=s;

	while(ifmul(str)>0)

	{

		if(ifmul(str)==1)

		{

				int pos=str.find('*');

				int tppos=pos;

				while(str[tppos-1]>='0' && str[tppos-1]<='9' || str[tppos-1]=='.')

				{

					tppos--;

				}

				int dpos=tppos;

				string num;

				num.assign(str,tppos,pos-tppos);

				double num1=StringToDouble(num);

				tppos=pos+1;

				while(str[tppos]>='0' && str[tppos]<='9' || str[tppos]=='.')

				{

					tppos++;

				}

				num.assign(str,pos+1,tppos-(pos+1));

				double num2=StringToDouble(num);

				string ans=DoubleToString(num1*num2);

				str.replace(dpos,tppos-dpos,ans);

		}

		else if(ifmul(str)==2)

		{

			int pos=str.find('/');

			int tppos=pos;

			while(str[tppos-1]>='0' && str[tppos-1]<='9' || str[tppos-1]=='.')

			{

				tppos--;

			}

			int dpos=tppos;

			string num;

			num.assign(str,tppos,pos-tppos);

			double num1=StringToDouble(num);

			tppos=pos+1;

			while(str[tppos]>='0' && str[tppos]<='9' || str[tppos]=='.')

			{

				tppos++;

			}

			num.assign(str,pos+1,tppos-(pos+1));

			double num2=StringToDouble(num);

			string ans=DoubleToString(num1/num2);

			str.replace(dpos,tppos-dpos,ans);

		}

	}

	while(ifplus(str)>0)

	{

		if(ifplus(str)==1)

		{

				int pos=str.find('+');

				int tppos=pos;

				while(str[tppos-1]>='0' && str[tppos-1]<='9' || str[tppos-1]=='.')

				{

					tppos--;

				}

				int dpos=tppos;

				string num;

				num.assign(str,tppos,pos-tppos);

				double num1=StringToDouble(num);

				tppos=pos+1;

				while(str[tppos]>='0' && str[tppos]<='9' || str[tppos]=='.')

				{

					tppos++;

				}

				num.assign(str,pos+1,tppos-(pos+1));

				double num2=StringToDouble(num);

				string ans=DoubleToString(num1+num2);

				str.replace(dpos,tppos-dpos,ans);

		}

		else if(ifplus(str)==2)

		{

			int pos=str.find('-');

			int tppos,dpos;

			double num1;

			string num;

			tppos=pos;

			while(str[tppos-1]>='0' && str[tppos-1]<='9' || str[tppos-1]=='.')

			{

				tppos--;

			}

			dpos=tppos;

			num.assign(str,tppos,pos-tppos);

			num1=StringToDouble(num);

			tppos=pos+1;

			while(str[tppos]>='0' && str[tppos]<='9' || str[tppos]=='.')

			{

				tppos++;

			}

			num.assign(str,pos+1,tppos-(pos+1));

			double num2=StringToDouble(num);

			string ans=DoubleToString(num1-num2);

			str.replace(dpos,tppos-dpos,ans);

		}

	}

	return str;

}



string simplify(string s)

{

	string str=s;

	while(ifbrace(str))

	{

		stack<int> brace;

		for(int i=0;i<str.length();i++)

		{

			if(str[i]=='(')

			{

				brace.push(i);

			}

			else if(str[i]==')')

			{

				int dpos=brace.top();

				brace.pop();

				string tp;

				tp.assign(str,dpos+1,i-(dpos+1));

				string ans=calculate_without_brace(tp);

				str.replace(dpos,i+1-dpos,ans);

				break;

			}

		}

	}

	return str;

}



int main()

{

	string a;

	cin>>a;

	cout<<calculate_without_brace(simplify(a))<<endl;

	return 0;

}
