### Transient Logging

### Transient Logging Use Case

In a very tight loop when data is changing and you want to see the data change, its often very hard to work this out from a long log file - instead Bilge and FlimFlam provide transient logging for this scenario.

There are two types of output currently supported by FlimFlam - STATs and GRIDs

### Stat Logging

STAT logging is designed for statistics and numbers that have a single value and change over time.  To write a STAT out to the output stream prefix it with [STAT] and give it a name also surrounded in square brackets.

```
b.Direct.Write($"[STAT][TurnTick]", $"{Turn.ToString()}.{Tick.ToString()}");
```


### GRID Logging

GRID logging is similar but designed for visual data ( in text ).   In the example in here there is a map being updated in gaming code. This map is a view from a player and changes repeatedly.  Its very hard to visualize this in logging data without custom support.

To write grid data use the direct logger.  Start the direct log wit [GRID] and provide a name for the grid output [NAME] then write each line of the grid in to the secondary message. Use | character to enter newlines into the grid data.

```
b.Direct.Write($"[GRID][NoughtsAndCrosses]", $"oxo|o x|oxx|");
// Output
oxo
o x
oxx
```

In the example here the grid logging quickly out the first letter of the colour that the engine is using to draw the pictorial representation of the data.  In the output gif below you can see how this renderes in FlimFlam.

```
 for (int yDelta = result.Height + result.LowestYValue; yDelta > result.LowestYValue; yDelta--) {
    map.Append("|");

    for (int xDelta = 0; xDelta < result.Width; xDelta++) {
        var srPosX = xDelta + result.LowestYValue;
        var srPosY = yDelta; 

        var res = result.GetResultAtPosition(new Point(srPosX, srPosY));

        Color c;
        switch (res) {
            case ScanTileResult.Unscanned:
                c = Color.DarkGray;
                break;

            case ScanTileResult.Unoccupied:
                c = Color.LightGreen;
                break;

            case ScanTileResult.SolidWall:
                c = Color.Black;
                break;

            case ScanTileResult.You:
                c = Color.Aqua;
                break;

        }
        map.Append(c.Name.ToLower()[0]);
                        
        xDraw++;
    }
    yDraw++;
    xDraw = 0;
}
b.Direct.Write("[GRID][MAP]", map.ToString());
```

Transient Logging in action

![Transient Logging](..\..\images\transientdata.gif)