```dataviewjs
await dv.view("tasksCalendar", {
	pages:"dv.pages().file.where(f=>f.folder === 'Planner/2024-05/Daily').tasks",
	view: "month",
	firstDayOfWeek: "1", 
	options: "style1",
	dailyNoteFormat: "YYYY-MM-DD",
})
```
