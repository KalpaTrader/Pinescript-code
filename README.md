# Pinescript-code
Tradingview code for indicator


//@version=5
indicator("Daily Lowest Low Marker", overlay=true)

// Input for the number of candles to look ahead
lookahead = input.int(4, "Look Ahead", minval=1)
minDistance = input.int(5, "Minimum Distance", minval=1)

// Function to check if the current bar is the start of a new day
newDay() => 
    dayChange = ta.change(time("D"))
    dayChange != 0

// Variable to track the last marked bar
var int lastMarkedBar = na

// Reset the last marked bar at the start of each day
if newDay()
    lastMarkedBar := na

// Check if the current candle is the lowest low among itself and the next lookahead candles
isLowest = low == ta.lowest(low, lookahead + 1)

// Ensure the current candle is at least minDistance candles away from the previously marked candle
shouldMark = isLowest and (na(lastMarkedBar) or (bar_index - lastMarkedBar) >= minDistance)

if shouldMark
    lastMarkedBar := bar_index

// Plot the shape outside of any local scope
plotshape(shouldMark, title="Lowest Low", location=location.belowbar, color=color.green, text="LL", textcolor=color.white, style=shape.labelup, size=size.normal)

// Optional Debug Information
if shouldMark
    label.new(bar_index, high, text="LL: " + str.tostring(low), color=color.green, textcolor=color.white, style=label.style_label_down, yloc=yloc.abovebar)
