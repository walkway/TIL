# window

## keyed windows
````
stream
       .keyBy(...)               <-  keyed versus non-keyed windows
       .window(...)              <-  required: "assigner"
      [.trigger(...)]            <-  optional: "trigger" (else default trigger)
      [.evictor(...)]            <-  optional: "evictor" (else no evictor)
      [.allowedLateness(...)]    <-  optional: "lateness" (else zero)
      [.sideOutputLateData(...)] <-  optional: "output tag" (else no side output for late data)
       .reduce/aggregate/apply()      <-  required: "function"
      [.getSideOutput(...)]      <-  optional: "output tag"
````

## Non-Keyed Windows
````
stream
       .windowAll(...)           <-  required: "assigner"
      [.trigger(...)]            <-  optional: "trigger" (else default trigger)
      [.evictor(...)]            <-  optional: "evictor" (else no evictor)
      [.allowedLateness(...)]    <-  optional: "lateness" (else zero)
      [.sideOutputLateData(...)] <-  optional: "output tag" (else no side output for late data)
       .reduce/aggregate/apply()      <-  required: "function"
      [.getSideOutput(...)]      <-  optional: "output tag"
````

## Tumbling Windows
- 지정된 window 크기의 window에 각 요소를 할당한다. 크기가 고정되어 있으며 겹치지 않는다.
````
DataStream<T> input = ...;

// tumbling event-time windows
input
    .keyBy(<key selector>)
    .window(TumblingEventTimeWindows.of(Time.seconds(5)))
    .<windowed transformation>(<window function>);

// tumbling processing-time windows
input
    .keyBy(<key selector>)
    .window(TumblingProcessingTimeWindows.of(Time.seconds(5)))
    .<windowed transformation>(<window function>);

// daily tumbling event-time windows offset by -8 hours.
input
    .keyBy(<key selector>)
    .window(TumblingEventTimeWindows.of(Time.days(1), Time.hours(-8)))
    .<windowed transformation>(<window function>);
````

Flink's windows are aligned with the epoch (rather than the first event), so if the job is deployed at 3:25, the first window will be about the interval from 3:00:00 to 3:59:59.999, and will be produced soon thereafter (based on the watermarking).
Note that you can pass an offset to the constructor, so if wanted the windows to end at 10 minutes past the hour instead, you could do this by specifying
````
.window(TumblingEventTimeWindows.of(Time.hours(1), Time.minutes(10)))
````
https://nightlies.apache.org/flink/flink-docs-master/docs/dev/datastream/operators/windows/. 
https://stackoverflow.com/questions/60634265/flink-tumble-window-trigger-time
