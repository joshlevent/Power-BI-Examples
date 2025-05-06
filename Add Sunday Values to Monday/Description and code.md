---
---

# Add Sunday Values to Monday
## Description
This code adds the value of the previous Sunday to the value of Monday.

### What's the purpose of this?

Let's say you have a report that shows the number of orders per day. When people order on the website on Sunday, they do not get processed until Monday. So, you want to always count Sunday orders as Monday orders.

### How does this work?

Like most things in Power BI, it can be done in Power Query or DAX. In this case, Power Query is preferred because it is a kind of data cleaning operation. I try to keep data cleaning and transformation operations in Power Query and use DAX only for calculating metrics and KPIs. However, for the sake of completeness, I will show how to do this in DAX as well.

In general, this kind of operation is difficult because it requires moving information from one row to another.

In Power Query, we use an index column to be able to look up the previous row's value. In DAX, we instead use the date.

## Power Query M Code
```m
let
    StartDate = #date(2024, 1, 1),
    EndDate   = #date(2025, 12, 31),

    Dates = List.Dates( StartDate,
                        Duration.Days( EndDate - StartDate ) + 1,
                        #duration(1,0,0,0) ),

    T0   = #table( type table [Date = date],
                   List.Transform( Dates, each { _ } ) ),

    T1   = Table.AddColumn( T0, "Value",
                            each Int64.From( Number.RandomBetween(0,20) ),
                            Int64.Type ),

    T2   = Table.AddColumn( T1, "Weekday",
                            each Date.ToText( [Date], "dddd" ),
                            type text ),

    T3   = Table.AddIndexColumn( T2, "Idx", 0, 1, Int64.Type ),

    Buf  = Table.Buffer( T3 ),

    T4   = Table.AddColumn(
             Buf, "Adjusted Value",
             each let
                     i   = [Idx],
                     v   = [Value],
                     wd  = [Weekday],
                     pr  = if i > 0 then Buf{i-1} else null,
                     adj = if      wd = "Sunday"                  then 0
                           else if wd = "Monday"
                                   and pr <> null
                                   and pr[Weekday] = "Sunday"     then v + pr[Value]
                           else                                   v
                  in  adj,
             Int64.Type ),

    Result = Table.RemoveColumns( T4, {"Idx"} )
in
    Result
```

## DAX Code
```DAX
Adjusted Value DAX =
VAR PrevDate    = Data[Date] - 1
VAR PrevSunday  =
    LOOKUPVALUE (
        Data[Value],
        Data[Date],     PrevDate,
        Data[Weekday],  "Sunday"
    )
RETURN
SWITCH (
    TRUE(),
    Data[Weekday] = "Sunday", 0,
    Data[Weekday] = "Monday", Data[Value] + COALESCE ( PrevSunday, 0 ),
    Data[Value]
)
```
