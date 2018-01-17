---
title: New Questions on Leetcode
date: 2015-06-06 16:23:49
tags:  
	- 刷题总结
categories:
	  - 刷题&面试
---

There several new questions post on Leetcode. I picked 5 of them to practice yesterday. The solutions are pretty straightforward. Therefore, I just post my comments on after the code, instead of posting a long paragraph to explain.
<!--more-->

### No.1 Happy Number
>Write an algorithm to determine if a number is "happy". A happy number is a number defined by the following process: Starting with any positive integer, replace the number by the sum of the squares of its digits, and repeat the process until the number equals 1 (where it will stay), or it loops endlessly in a cycle which does not include 1.  Those numbers for which this process ends in 1 are happy numbers.

>Example: 19 is a happy number
1^2 + 9^2 = 82
8^2 + 2^2 = 68
6^2 + 8^2 = 100
12 + 0^2 + 0^2 = 1

####My Solution

{% codeblock Happy_Number lang:C %}
class Solution {
	public:
    //Trick 1: map for small scale exponential operations
    int expoMap[10]={0,1,4,9,16,25,36,49,64,81};
    bool isHappy(int n) {
       	if(n < 10){
          	if(n == 1 || n == 7){ //Base case
          		return true;
          	}else{     
          		return false;
           }
        }
        int m = breakSum(n);
        return isHappy(m);
    }

    int breakSum(int n){
       	int sum = 0;
       	while(n){   //classic part in coding interview
      		int a = n%10;
       		sum += expoMap[a];
        	n = n/10;
        }
    	return sum;
    }
};
{% endcodeblock %}

***
### No.2 Isomorphic Strings
>Given two strings s and t (same length), determine if they are isomorphic. Two strings are isomorphic if the characters in s can be replaced to get t. All occurrences of a character must be replaced with another character while preserving the order of characters. No two characters may map to the same character but a character may map to itself.
>
For example,
Given "egg", "add", return true.
Given "foo", "bar", return false.
Given "paper", "title", return true.

####My Solution

{% codeblock Isomorphic_Strings lang:C %}
class Solution {
public:
    bool isIsomorphic(string s, string t) {
        int a[256] = {0}, b[256] = {0};
        int len = s.size();
        for(int i = 0; i < len; i++){
            if(a[s[i]] > 0 || b[t[i]] > 0){
                if(a[s[i]] == b[t[i]]){ //means they appear at the same position at the 1st time
                    continue;
                }else{
                    return false;
                }
            }else{
                a[s[i]]=i+1; //record the first time they show up
                b[t[i]]=i+1; //+1 is the way to distinguish them from 0
            }
        }
        return true;
    }
};
{% endcodeblock %}

***

### No.3 Count Prime Numbers
#####Question:
>Count the number of prime numbers less than a non-negative number, n.

>This solution is based on Sieve of Eratosthenes, which might sound too academic, but actually pretty straightforward.

####My Solution  [Time: O(n)    Space:O(n) ]

{% codeblock lang:C %}
class Solution {
public:
    int countPrimes(int n) {
        if(n <= 2) {      //Base case;
            return 0;
        }
        bool checkPrime[n];              
        for(int i = 0; i < n; i++) {
            checkPrime[i]=true;
        }
        int prime = 2;
        while(prime <= sqrt(n)){    //If a number is not a prime, this number at least has a factor <= n/2
            for(int x = 2; prime*x < n; x++){
                checkPrime[x*prime] = false;
            }
            prime++; //move to the next number
            while(prime <= n/2 && checkPrime[prime] == false) {  //if the number has been marked as not a prime, jump over it.
                prime++;
            }
        }
        int count = 0;
        for(int i = 2; i < n; i++){  //Trap: remember start from 2.
            if(checkPrime[i]) count++;
        }
        return count;
    }
};
{% endcodeblock %}
