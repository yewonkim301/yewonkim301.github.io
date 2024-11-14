---
title: "[Git] Pull Request 란?"
date: 2024-11-13
categories: [Git]
tags: [TIL, Git, PR]
---

## 📍Pull Request란?
- 내가 수정한 코드가 있으니, 내 branch를 가져가 검토 후 병합해달라고 요청을 보내는 것.
- PR을 통해 코드 충돌을 최소화할 수 있다.
- 병합 승인 절차를 강제하면 구성원이 코드 병합에 대해 검토를 진행할 수 있다.
<br /><br />

## 📍PR 하는 방법
1. 원격에 내 branch를 커밋 & 푸시하기
<br />

2. **Pull Request** 탭에서 PR 만들기
    2-1. `New pull request` 버튼을 누른다.
    ![img](/assets/img/til/pr1.png)
<br />

    2-2. **base**(근본이 되는 브랜치)와 **compare**(내 브랜치) 를 선택하고 `Create pull request` 버튼을 눌러서 새로운 PR을 만든다.
    ![img](/assets/img/til/pr2.png)
<br />

    2-3. PR에 필요한 코멘트를 작성하고 `Create pull request`를 눌러 PR을 완료한다.
    ![img](/assets/img/til/pr3.png)

<br /><br />

참고 : 
- [[GIT] ⚡️ 깃헙 Pull Request 보내는 방법 - 알기 쉽게 정리](https://inpa.tistory.com/entry/GIT-%E2%9A%A1%EF%B8%8F-%EA%B9%83%ED%97%99-PRPull-Request-%EB%B3%B4%EB%82%B4%EB%8A%94-%EB%B0%A9%EB%B2%95-folk-issue)
- [[Git 삽질기록] 'PR을 올리다'? Pull Request에 대해서](https://holika.tistory.com/entry/Git-%EC%82%BD%EC%A7%88%EA%B8%B0%EB%A1%9D-PR%EC%9D%84-%EC%98%AC%EB%A6%AC%EB%8B%A4-Pull-Request%EC%97%90-%EB%8C%80%ED%95%B4%EC%84%9C)
- [[Git] Git 의 Issue 와 Pull Request, Review 기능 사용하기 (merger금지, 커밋과 이슈연결 등)](https://skylarcoding.tistory.com/225)
- [강의자료] [특강 1] Git 그리고 PR(Pull Request)