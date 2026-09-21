# Thinkorswim thinkScript
# Time-of-Day Relative Volume — 2-minute chart
# Compares each current 2-minute bar with the same time slot
# across the previous 10 regular trading sessions.
declare upper;

input comparisonDays = 10;

def isTwoMinuteChart =
    GetAggregationPeriod() == AggregationPeriod.TWO_MIN;

def regularSession =
    SecondsFromTime(0930) >= 0 and
    SecondsTillTime(1600) > 0;

def barsPerRegularSession = 195;

def sameTimeVolumeTotal =
    fold dayNumber = 1 to comparisonDays + 1
    with total = 0
    do total + GetValue(
        volume,
        dayNumber * barsPerRegularSession,
        comparisonDays * barsPerRegularSession
    );

def averageSameTimeVolume =
    sameTimeVolumeTotal / comparisonDays;

def todRVol =
    if regularSession and averageSameTimeVolume > 0
    then volume / averageSameTimeVolume
    else Double.NaN;

AddLabel(
    yes,
    if !isTwoMinuteChart
    then "TOD RVOL: USE 2-MIN CHART"
    else if !regularSession
    then "TOD RVOL: OUTSIDE RTH"
    else "TOD RVOL  " + AsText(Round(todRVol, 2)) + "x",
    if !isTwoMinuteChart then Color.RED
    else if !regularSession then Color.DARK_GRAY
    else if todRVol >= 3 then Color.MAGENTA
    else if todRVol >= 2 then Color.CYAN
    else if todRVol >= 1 then Color.GREEN
    else Color.GRAY,
    Location.TOP_LEFT,
    FontSize.X_LARGE,
    yes
);
