---
title: Python 설치
parent: 파이썬 튜토리얼
nav_order: 1
has_toc: false
---
# Python 설치

## Python 다운로드  

1. 홈페이지 접속 [python.org](https://python.org) -> Download -> All releases  
![](Python_설치_001.png)
2. Looking for a specific release? 에서 원하는 버전 Download 선택  
![](Python_설치_002.png)
3. Files -> Windows 의 경우 Windows Installer (64-bit) 선택하여 다운로드  
![](Python_설치_003.png)

## Python 설치  

1. Python 설치  
![](Python_설치_004.png)  
Add python.exe to PATH 를 선택하면 현재 설치하는 버전의 Python 이 환경변수에 설정된다.<br><br>
![](Python_설치_005.png)  
![](Python_설치_006.png)  
![](Python_설치_007.png)  
설치 완료 시 Disable path length limit 을 선택해준다.<br><br>  
![](Python_설치_008.png)  
설치 완료<br><br>

## 설치 버전 확인  
1. 버전 확인  
```
C:\Users\ME>python --version
Python 3.12.9
```
2. 설치된 모든 버전 확인  
```
C:\Users\ME>py -0
 -V:3.13 *        Python 3.13 (64-bit)
 -V:3.12          Python 3.12 (64-bit)
```
3. 설치된 모든 버전과 설치 경로 확인  
```
C:\Users\ME>py -0p
 -V:3.13 *        C:\Users\ME\AppData\Local\Programs\Python\Python313\python.exe
 -V:3.12          C:\Users\ME\AppData\Local\Programs\Python\Python312\python.exe
```