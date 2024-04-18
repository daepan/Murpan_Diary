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
* 기사 시험 준비해야된단다 대관아
## 내일 기억할 일
- 
## 오늘 기억할 일
* 


## 아침

### 오늘의 목표

- [ ] 

### 할 일 추가하기

- [ ] #task 서버 배포 및 현재 기능들 완성하기
- [ ] #task 스트리밍? 방송 관련 기능들에 대한 서버 공부 및 스택 준비하기
- [ ] #task 운체 공부
- [ ] #task 포토폴리오 제작
- [ ] #task 정처기 기사 실기 공부
- [ ] #task logger NPM 올리기 
- [ ] #task 22일 운체 중간고사
- [ ] #task 24일 디공실 시험
- [ ] #task 24일 컴구조 시험
- [ ] #task 매주 금요일 레거시 그룹회의


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