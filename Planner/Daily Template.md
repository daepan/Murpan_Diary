---
date_daily: <% tp.file.title.slice(0,10) %>
achievement: 
emotion: 
important_date: false
tags:
  - daily
---
<%*
    const currentMoment = moment(tp.file.title, "YYYY-MM-DD");
    tR += '❮ ';
	tR += '[[' + currentMoment.format('YYYY|YYYY년') + ']]' + ' / ';
	tR += '[[' + currentMoment.format('YYYY-MM|MM월') + ']]' + ' / ';
	tR += '[[' + currentMoment.format('gggg-[W]ww') + '|' + currentMoment.format('ww[주]') + ']]';
	tR += ' ❯';
	tR += '\n';
    tR += '❮❮ ';
    currentMoment.add(-1,'days');
    tR += '[[' + currentMoment.format('YYYY-MM-DD(ddd)') + ']]' + ' | ';
    currentMoment.add(1,'days');
    tR += currentMoment.format('YYYY-MM-DD(ddd)') + ' | ';
    currentMoment.add(1,'days');
    tR += '[[' + currentMoment.format('YYYY-MM-DD(ddd)') + ']]';
    currentMoment.add(-1,'days');
    tR += ' ❯❯';
%>

<% tp.web.daily_quote() %>
## 나의 현재 볼륨
* 졸작 페이지 만들기 원격 진료 시스템 구상하고 다음주 수요일까지 준비하기
* 디공 페이지 관련해서 배포랑 메인 페이지 3D 이미지 관련 만져야함
## 내일 기억할 일
- 
## 오늘 기억할 일
* 


## 아침

### 오늘의 목표

- [ ] 

### 할 일 추가하기

- [ ] #task 포토폴리오 제작
- [ ] #task 매주 금요일 레거시 그룹회의
- [ ] #task 취준반 스터디
- [ ] #task 졸업연구작품 판넬용 PPT 제출 일정   - 첨부파일에서 양식 다운 받은 후 작성하여 제출   - 판넬샘플 파일 참고하여 작성할 것!   - **2024년 06월 10일(월)** 자정까지 학부홈페이지에 업로드하여 제출 
- [ ] #task 작품집 사진촬영 일정   - **2024년 06월 04일(화) 오전 10시 ~ 12시 : 업체에서 촬영(원하는 팀들에 한함)**
- [ ] #task 컴구조 과제(6/5)
- [ ] #task 직능개훈 기말 과제 (6/18)
- [ ] #task 운체 기말 과제 쓰레드 관련
- [ ] #task 네이버 부스트 캠프 지원서 (6/10)
- [ ] #task 운체 기말(6/10)
- [ ] #task 알고리즘 기말 (6/17)

## 오늘 끝내야 할 일
```tasks
due on or before <% tp.file.title.slice(0,10) %>
filter by function task.file.folder.includes("10. Planner")
filter by function !task.file.folder.includes("templates")
not done
sort by priority
```
```tasks
tag include #업무 
```

### 앞으로 해야할 일


### 오늘 완료한 일
```tasks
done <% tp.file.title.slice(0,10) %>
```

## 운동
- 

## 하루 마무리
### 오늘 배운 것들
- 
- 
### 오늘 감사한 일
>[!note]
>
### 일기

## 오늘 작성한 노트
```dataview
List FROM "" WHERE file.cday = date("<% tp.date.now('YYYY-MM-DD') %>") SORT file.ctime desc

```

## 오늘 수정한 노트
```dataview
List FROM "" WHERE file.mday = date("<% tp.date.now('YYYY-MM-DD') %>") SORT file.mtime desc


```