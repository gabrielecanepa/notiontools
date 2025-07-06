## Task Due Date

### Requirements

- `Due date` property
- `Status` property with value "Done", "Cancelled", or "Archived"
- `Project` relation with a `Due date` property

### Formula

```js
let(
  dueDate,
  if(prop("Due date"), prop("Due date"), prop("Project").first().prop("Due date")),
  let(
    let(
      days,
      dateBetween(parseDate(formatDate(dueDate, "YYYY-MM-DD")), parseDate(formatDate(now(), "YYYY-MM-DD")), "days"),
      if(
        ["Done", "Cancelled", "Archived"].includes(prop("Status")),
        "",
        if(
          dueDate,
          if(
            days < 0,
            style(if(days == -1, "Yesterday", abs(days) + " days ago"), "red"),
            if(
              days == 0, 
              style("Today", "red"),
              if(
                days > 1,
                style(if(days > 7, prop("Due date"), if(days == 7, "1 week left", style(days + " days left", if(days > 3, "yellow", "orange"))))),
                style("Tomorrow", "orange")
              )
            )
          ),
          ""
        )
      )
    )
  )
)
```
