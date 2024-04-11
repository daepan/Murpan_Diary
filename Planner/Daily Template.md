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
* 
## 내일 기억할 일
- 
## 오늘 기억할 일
* 


## 아침

### 오늘의 목표

- [ ] 
- [ ] 

### 할 일 추가하기

- [ ] 

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