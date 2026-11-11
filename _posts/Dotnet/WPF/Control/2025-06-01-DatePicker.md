---
categories: WPF
---

# 日期选择器

DisplayDate 显示日期,当你点开的时候,高亮的那个

SelectedDate 选择日期,已经预选了一个

SelectedDateFormat 格式

BlackoutDates 排除日期
```xml
<DatePicker>
        <DatePicker.BlackoutDates>
                <CalendarDateRange Start="2026-4-10" End="2026-4-18"></CalendarDateRange>
        </DatePicker.BlackoutDates>
</DatePicker>
```
