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
## 나의 할일

- [ ] #task 포토폴리오 제작
- [ ] #task 매주 금요일 레거시 그룹회의
- [ ] #task 취준반 스터디
- [ ] #task 알고리즘 기말 (6/17)

## 오늘의 할 일
- [ ] 

## 기술 관련 할꺼 메모

- [ ] #task 포폴이랑 이력서 업데이트
- [ ] #task React-Portal 전역화 기능 관련 글 정리
- [ ] #task 네부캠 코테 대비
- [ ] #task React DeepDive 스터디
- [ ] #task 영어 졸업 맞추기
- [ ] #task 네부캠 탈락 시 오픈소스 컨트리뷰터 지원
- [ ] #task 포폴 업데이트 이후 토스 서류 넣어보기
- [ ] #task React.memo 100번 써보기
- [ ] #task React-query와 zustand, jotai 라이브러리의 차이점이랑 장점 공부하기
- [ ] #task 모노레포 구현 + Skin-ai-server


### 할 일 추가하기


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


### 오늘 완료한 일
```tasks
done <% tp.file.title.slice(0,10) %>
```

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