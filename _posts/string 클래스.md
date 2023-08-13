---
title: "<string> 과 문자열"
layout: post
date: 2023-08-13 16:54
tag:
- 개념 정리
- 코테
- c++
description: string 클래스와 문자열 관리 개념 정리
---

# string 클래스란?

string 클래스는 c++에서 문자열을 다루는데 필요한 자료형이다. 코테를 풀다보면 문자열을 파싱, 추출, 변환 등 문자열을 다루는 부분이 굉장히 많은만큼 필수적으로 알아둬야 하는 자료형이라고 할 수 있다.  
string에는 굉장히 많은 서브함수가 존재한다. 어차피 내가 찾아보려고 쓰는 것이니 쓰든 안쓰든 다 적어보자면  
str1.at(index)  
> index 위치의 char 문자를 반환한다. 범위를 벗어나면 예외를 반환한다.

str1.operator[index]
> 여러 다른 배열과 같이 대괄호로 접근하여 index위치의 문자를 반환한다.  
at와 달리 예외를 반환하지 않는다.

str1.front()
> 문자열의 가장 앞에 위치한 문자를 반환한다.

str1.back()
> 문자열의 맨 뒤의 문자를 반환한다.

str1.size() && str1.length()
> 문자열의 길이를 반환한다. size는 문자열이 차지하는 메모리 크기,  
 length는 문자열의 길이를 반환하는 차이가 있다지만 항상 같은값을 반환하므로  
 실사용의 차이는 없다.

str1.capacity()
> 문자열에 할당된 메모리의 크기를 반환한다.  
 size와 length와는 다른 개념이다.

str1.resize(n)
> 문자열을 n의 크기로 맞춘다. 남은 문자열은 버리고  
n이 문자열보다 크다면 빈 공간으로 부족한 부분을 채운다.

str1.shrink_to_fit()
> 문자열의 capacity를 문자열의 길이에 맞게 맞춰준다.

str1.reserve(n)
> 문자열에 capacity를 미리 할당하여 문자열이 길어짐에  
 따라 capacity가 재할당되는 것을 방지한다.

str1.clear()
> 문자열을 비운다. capacity는 남는다.

str1.empty()
> 문자열이 비었는지 확인한다.

str1.substr(index, len)
> 문자열의 index부터 len만큼을 반환한다. 굉장히 자주 쓰이는 함수 중 하나이다.

str1.replace(index, len, str)
> 문자열의 index부터 len만큼을 str로 대체한다.

str1.compare(str)
> 문자열을 매개변수 문자열과 비교하여 같으면 0 크면 양수 작으면 음수를 반환한다.

str1.copy(str, len, index)
> 이름 그대로 복사를 하는 함수이다. 특이하게 len이 2번째 index가 3번째이다.  
str에 문자열의 index부터 len만큼 복사하고 복사한 길이를 반환한다.

str1.find(str)
> 문자열에서 매개변수와 일치하는 문자열이 있는지 확인한다.  
일치한다면 일치한 부분의 첫 부분의 index를 반환한다.  
이 또한 굉장히 자주 쓰인다.

str1.push_back() && str1.pop_back()
> vector에서 자주 쓰이는 그것이다. 기능도 똑같이 맨 뒤에 문자를 추가하고 맨 뒤의 문자를 제거한다.

이와 같이 문자열을 관리하는데 쓰이는 여러 string의 멤버 함수를 정리해보았다. string과는 별개로 문자열을 파싱할 때 매우 유용하게 사용되는 자료형이 있는데 istringstream이 그것이다.  
<sstream> 헤더를 필요로 하는 자료형인데 예를 들어 문자열 "123 456 789"를 공백을 기준으로 파싱해야한다고 할 때,  
string str="123 456 789";  
istringstream iss(str);  
iss >> str1 >> str2 >> str3;  
위와 같이 사용하면 str1, str2, str3에 파싱된 문자열이 들어가게 된다.  
추후 문제를 풀 때 매우 도움이 될 자료라고 확신하는 바이다.