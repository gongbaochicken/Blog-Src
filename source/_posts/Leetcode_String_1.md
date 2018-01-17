title: Leetcode String (Part 1)
date: 2015-06-27 20:38:07
tags:
  - 刷题总结
categories:
  - 刷题&面试
---
### Question List:

* Q1: Compare Version Number
* Q2: Valid Palindrome
* Q3: Valid Parentheses
* Q4: Valid Number
<!--more-->

### Question 1: Compare Version Number
Compare two version numbers version1 and version2.
If version1 > version2 return 1, if version1 < version2 return -1, otherwise return 0.

You may assume that the version strings are non-empty and contain only digits and the . character.
The . character does not represent a decimal point and is used to separate number sequences.
For instance, 2.5 is not "two and a half" or "half way to version three", it is the fifth second-level revision of the second first-level revision.

Here is an example of version numbers ordering: 0.1 < 1.1 < 1.2 < 13.37

#### My Solution:

```c++
class Solution {
public:
    int compareVersion(string version1, string version2) {
        int pos1 = 0, pos2 = 0, token1, token2;
        string delimiter = ".";
        pos1 = version1.find(delimiter);
        pos2 = version2.find(delimiter);
        if(pos1 >=0 ) {
        	token1 = stoi(version1.substr(0,pos1));
        }else {
        	token1 = version1.empty()? 0:stoi(version1);
        }
        if(pos2 >=0 ) {
            token2 = stoi(version2.substr(0,pos2));
        } else {
            token2 = version2.empty()? 0:stoi(version2);
        }
        int diff = token1 - token2;
        if(diff == 0){
            if(pos1<0 && pos2<0) return 0;
            version1 = pos1 >= 0 ? version1.erase(0, pos1 + 1):"0";
            version2 = pos2 >= 0 ? version2.erase(0, pos2 + 1):"0";
            return compareVersion(version1, version2);
        }
        else return diff > 0 ? 1:-1;
    }
};
```

### Question 2: Valid Palindrome
 Given a string, determine if it is a palindrome, considering only alphanumeric characters and ignoring cases.

For example,
"A man, a plan, a canal: Panama" is a palindrome.
"race a car" is not a palindrome.

Note:
Have you consider that the string might be empty? This is a good question to ask during an interview.

For the purpose of this problem, we define empty string as valid palindrome.

#### My Solution:

```c++
bool isPalindrome(string s) {
        int start=0;
        int end=s.size()-1;
        while(start<end){
            if(isalnum(s[start])==false) {start++;continue;}
            if(isalnum(s[end])==false){end--;continue;}
            if(tolower(s[start])!=tolower(s[end])){
                return false;
            }else{
            start++;
            end--;
            }
        }
        return true;
    }
}
```

### Question 3: Valid Parentheses
Given a string containing just the characters '(', ')', '{', '}', '[' and ']', determine if the input string is valid.

The brackets must close in the correct order, "()" and "()[]{}" are all valid but "(]" and "([)]" are not.

#### My Solution:

```c++
bool isValid(string s) {
	stack<char> st;
   	for(int i=0;i<s.size();i++){
   		if( s[i]=='(' || s[i]=='{' || s[i]=='['){
   			st.push(s[i]);
   		}
      	else{
     		if(st.empty()) return false;
        	if ((s[i]==')') && (st.top()!='(')) {return false;}
        	if ((s[i]=='{') && (st.top()!='}')) {return false;}
      		if ((s[i]=='[') && (st.top()!=']')) {return false;}
        	st.pop();
     	}
 	}
  	return st.empty();
}
```


### Question 4:Valid Number
Validate if a given string is numeric.

Some examples: "0" => true || " 0.1 " => true || "abc" => false || "1 a" => false ||  "2e10" => true

#### My solutions:
/*1. skip the leading whitespaces;
2. check if the significand is valid. To do so, simply skip the leading sign and count the number of digits and the number of points. A valid significand has no more than one point and at least one digit.
3. check if the exponent part is valid. We do this if the significand is followed by 'e'. Simply skip the leading sign and count the number of digits. A valid exponent contain at least one digit.
4. skip the trailing whitespaces. We must reach the ending 0 if the string is a valid number.*/

```c++
class Solution {
public:
    bool isNumber(const char *s) {
        int i = 0;
    // skip the whilespaces
        for(; s[i] == ' '; i++) {}
    // check the significand
        if(s[i] == '+' || s[i] == '-') i++; // skip the sign if exist
        int n_nm, n_pt;
        for(n_nm=0, n_pt=0; (s[i]<='9' && s[i]>='0') || s[i]=='.'; i++)
            s[i] == '.' ? n_pt++:n_nm++;       
        if(n_pt>1 || n_nm<1) // no more than one point, at least one digit
            return false;
    // check the exponent if exist
        if(s[i] == 'e') {
            i++;
            if(s[i] == '+' || s[i] == '-') i++; // skip the sign
            int n_nm = 0;
            for(; s[i]>='0' && s[i]<='9'; i++, n_nm++) {}
            if(n_nm<1) return false;
        }
    // skip the trailing whitespaces
        for(; s[i] == ' '; i++) {}
        return s[i]==0;  // must reach the ending 0 of the string
    }
};
```
