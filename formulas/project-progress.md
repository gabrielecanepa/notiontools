## Project Progress

### Requirements

- `Due date`, `End date`, `Status`, and `Completion` properties
- `Tasks` relation with a `Status` property with value "Done" or "Archived"

### Formula

```js
if(
	["Done", "Archived"].includes(prop("Status")),
  style("Completed on " + formatDate(prop("End date"), "MMMM DD, YYYY"), "grey"),
  join(
    [
      if(
        prop("Tasks").length() > 0,
        let(
          doneTasks,
          prop("Tasks").filter(current.prop("Status").test("(Done|Archived)")).length(),
          style(doneTasks + "/", if(doneTasks == prop("Tasks").length(), "b", "")) + style(prop("Tasks").length(), "bold")
        ),
        ""
      ),
      let(
        days,
        dateBetween(parseDate(formatDate(prop("Due date"), "YYYY-MM-DD")), parseDate(formatDate(now(), "YYYY-MM-DD")), "days"),	
        if(
          !prop("Due date"),
          "",
          if(
            days < 0,
            if(
              prop("Completion") == 1,
              "",
              style(if(days == -1, "Yesterday", abs(days) + " days ago"), "red")
            ),
            if(
              days == 0,
              style("Today", "red"),
              if(
                days > 1,
                style(days + " days left", if (days < 7, "yellow", "grey")),
                style("1 day left", "orange")
              )
            )
          )
        )
      )
    ].filter(current != ""),
    " • "
  )
)
```
