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
## Window Assigners 
### Tumbling Windows
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

## Window Functions
### AggregateFunction
````

/**
 * The accumulator is used to keep a running sum and a count. The {@code getResult} method
 * computes the average.
 */
private static class AverageAggregate
    implements AggregateFunction<Tuple2<String, Long>, Tuple2<Long, Long>, Double> {
  @Override
  public Tuple2<Long, Long> createAccumulator() {
    return new Tuple2<>(0L, 0L);
  }

  @Override
  public Tuple2<Long, Long> add(Tuple2<String, Long> value, Tuple2<Long, Long> accumulator) {
    return new Tuple2<>(accumulator.f0 + value.f1, accumulator.f1 + 1L);
  }

  @Override
  public Double getResult(Tuple2<Long, Long> accumulator) {
    return ((double) accumulator.f0) / accumulator.f1;
  }

  @Override
  public Tuple2<Long, Long> merge(Tuple2<Long, Long> a, Tuple2<Long, Long> b) {
    return new Tuple2<>(a.f0 + b.f0, a.f1 + b.f1);
  }
}

DataStream<Tuple2<String, Long>> input = ...;

input
    .keyBy(<key selector>)
    .window(<window assigner>)
    .aggregate(new AverageAggregate());
````
````
createAccumulator
add
getResult
merge
````
#### merge
The merge method is called when two windows are merged. This applies to session windows, which are merged whenever two sessions are collapsed into one by the arrival of an event that bridges the gap between the sessions.

https://nightlies.apache.org/flink/flink-docs-master/docs/dev/datastream/operators/windows/. 
https://stackoverflow.com/questions/60634265/flink-tumble-window-trigger-time. 
https://stackoverflow.com/questions/65848095/implications-of-merge-method-in-aggregatefunction
