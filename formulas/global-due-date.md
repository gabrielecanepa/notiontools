## Global Due Date

### Requirements

- `Due date` property
- `Status` property with value "Done", "Cancelled", or "Archived"
- `Project` relation with a `Due date` property

### Formula

```js
if(
  ["Done", "Cancelled", "Archived"].includes(prop("Status")),
  "",
  let(
    DUE_DATE,
    prop("Due date"),
    let(
      PROJECT_DUE_DATE,
      prop("Project").first().prop("Due date"),
      if(
        DUE_DATE,
        let(
          days,
          dateBetween(parseDate(formatDate(DUE_DATE, "YYYY-MM-DD")), parseDate(formatDate(now(), "YYYY-MM-DD")), "days"),
          if(
            days < 0,
            style(if(days == -1, "Yesterday", abs(days) + " days ago"), "red"),
            if(
              days == 0, 
              style("Today", "red"),
              if(
                days > 1,
                style(if(days > 7, DUE_DATE, if(days == 7, "1 week left", style(days + " days left", if(days > 3, "yellow", "orange"))))),
                style("Tomorrow", "orange")
              )
            )
          )
        ),
        if(
          PROJECT_DUE_DATE,
          let(
            days,
            dateBetween(parseDate(formatDate(PROJECT_DUE_DATE, "YYYY-MM-DD")), parseDate(formatDate(now(), "YYYY-MM-DD")), "days"),
            if(
              days < 0,
              style("Project dued " + if(days == -1, "yesterday", abs(days) + " days ago"), "red"),
              if(
                days == 0, 
                style("Project due today", "red"),
                if(
                  days > 1,
                  style(if(days > 14, "", if(days == 7, "1 week to project deadline", style(days + " days to project deadline", if(days > 3, "yellow", "orange"))))),
                  style("Project due tomorrow", "orange")
                )
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
