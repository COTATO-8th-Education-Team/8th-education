# 8th-education
IT 연합동아리 '코테이토' 8기 교육팀 발표 자료 폴더입니다.

교육팀은 CS지식, 기술 면접에 자주 출제되는 주제를 매주 공부하고 이를 바탕으로 '코테이토 8기 정규세션'에서 해당 주제들을 발표합니다. 세션간 정규 교육은 6개월간 13회 진행될 계획입니다.



# Members
|                           <img src="https://github.com/Youthhing.png" width=120/>                           |                          <img src="https://github.com/J0onYEong.png" width=120/>                           |                       <img src="https://github.com/euvely.png" width=120 />                        |                         <img src="https://github.com/GGHDMS.png" width=120/>                          |                         <img src="https://github.com/WONYOUNG-HC.png" width=120/>                          |
|:-----------------------------------------------------------------------------------------------------------:|:-------------------------------------------------------------------------------------------------------:|:-------------------------------------------------------------------------------------------------:|:-------------------------------------------------------------------------------------------------------:|:-------------------------------------------------------------------------------------------------------:|
|                                     [신유승](https://github.com/Youthhing)                                     |                                 [최준영](https://github.com/J0onYEong)                                  |                                  [최유진](https://github.com/euvely)                                  |                                   [허석문](https://github.com/GGHDMS)                                    |                                   [조원영](https://github.com/WONYOUNG-HC)                                    |
| [ 6기 BE ] 교육팀장  | [ 6기 ios ] 교육부팀장 | [ 7기 FE ] 교육팀원 | [ 8기 BE ] 교육팀원 | [ 8기 FE ] 교육팀원 |


# What we Studied

|#|                       주제                       | 발표자         |      날짜      |
|:-:|:----------------------------------------------:|:-----------|:------------:|
|1주차| Git | 신유승 | 2023년 9월 08일 |
|2주차| 소프트웨어 개발방법론: 애자일 프로세스 | 신유승 | 2023년 9월 15일 |
|3주차| 동기와 비동기, 블로킹과 논블로킹 | 최준영 | 2023년 9월 22일 |
|4주차| 클라우드 서비스, AWS | 최유진 | 2023년 10월 06일 |
|5주차| 네트워크 IP주소 | 조원영 | 2023년 10월 13일 |
|6주차| SQL vs NoSQL | 허석문 | 2023년 11월 10일 |
|7주차| 파일시스템 | 최준영 | 2023년 11월 24일 |
|8주차| 브라우저 렌더링 | 조원영 | 2023년 12월 01일 |
|9주차| CORS | 신유승 | 2023년 12월 22일 |




# Rules
- 발표자는 발표전 주 목요일 정기 회의때 주제를 선정해 팀원들에게 안내한다.
- 발표자는 발표주차 수요일자정까지 발표자료 초안, 대본을 팀원들에게 필수로 공유한다.
- 교육팀원들은 매주 목요일 회의전까지 발표자료를 검토하고, 관련된 CS퀴즈 3문제를 제작한다.
- 매주 목요일 정규회의때 CS퀴즈 10문제를 선정한다.



# Directory Structure
```plainText
│
├─ cotato-8th-education-team
│     │
│     ├─ Week1 (dir)
│     │     │ 
│     │     ├─  Youthhing (dir) // 해당 주차 발표자
│     │     │    ├─ 주제.md // 해당 주차 정리 내용. 파일명은 확장자는 `.md` 
│     │     │    └─ `기타 소스 코드 및 자료들`
│     │     │
│     │     ├─  J0onYEong (dir) // 발표자 외의 팀원들 (Option)
│     │     │    ├─ 주제.md 
│     │     │    └─ `기타 자료`//참고한 자료들
│     │     │
│     │     ├─  Member3 (dir) 
│     │     │    ├─ WIL.md 
│     │     │    └─ `기타 자료`
│     │     │
│     │     └─ 최종 발표 자료.pdf //확장자명은 .pdf
│     │   
│     │   
│     ├─ .. 이하 동일
│ 
│
```

## Git Rule
- git clone <주소>를 통해 Repository를 clone 받는다.
- `git checkout -b 본인 핸들명` 명령어를 통해 본인 브랜치를 생성한다.
- 해당 브랜치에서 작업 후 커밋한다. (절대! dev 브랜치에서 작업하지 않는다.)
- 작업 후 develop 브랜치로 PR을 날린다.
- 작업 후 로컬에서 `git checkout develop`로 브랜치를 돌린 후 `git branch -d 본인 핸들명`을 통해 브랜치를 삭제한다.
- 작업 후 `git push origin -d 본인 핸들명`을 통해 원격 브랜치를 삭제한다.
- 다시 작업을 시작하고 싶으면 `git checkout -b 본인 핸들명`으로 브랜치를 로컬에서 생성해 작업한다.

## Convention
발표자료 업로드: `docs: [신유승] n주차 발표자료 업로드`<br>
발표외 공부자료 업로드: `docs: [OOO] n주차 공부자료 업로드`<br>


### Pull request convention
[OOO] n주차 __자료 제출합니다. (발표 or 공부)


