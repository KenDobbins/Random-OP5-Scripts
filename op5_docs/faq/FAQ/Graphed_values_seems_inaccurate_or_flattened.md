# Graphed values seems inaccurate or flattened

## Question

* * * * *

Performance data values generated by check plugins, that are inserted into RRD files and then exposed as host and service graphs, do sometimes seem to not represent the actual values that the host/service check actually resulted in.

For example, a network traffic check might show a very large network usage at one point and then show normal values again the next times it runs. This would probably be due to a temporary network load. In the resulting graph, however, this short-lived network usage peak could seem to be lost or flattened.

## Answer

* * * * *

A check plugin is usually run every couple of minutes. Every time the check runs, new performance data is generated. This is called a measurement point. To understand why graphs look the way they do, several things should be kept in mind.

-   To generate a graph which displays all measurement points fully intact, the image of the graph would need to be wide enough to cover all these measurement points. If only a single image pixel is needed for every measurement point, and the check is run every 3 minutes, the image of a 7 days' graph would end up as (7\*24\*60)/3=3360 pixels wide. In most cases, your computer display will use a much lower resolution. Instead, a much smaller image is used, and this requires calculating averages of measurement point values, effectively flattening the graphed values.
-   Due to the RRD format, which the graph data is stored within, values are spread out over time. If new graph values are inserted into the RRD file at 17:00:24, and then again at 17:03:26, this will be processed and rewritten as if values were entered at exactly 17:00, 17:01, 17:02, 17:03, etc.
-   The different time periods (a day, a week, a month, etc.) are stored as different "containers" in the RRD file. The different containers stores the values with different accuracy. The larger the time period is, the less accurate it is, since averages of the values are computed in a much greater sense.
