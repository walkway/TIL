# Spring boot, http post

- @RestController = @Controller + @ResponseBody

````
@RequestMapping(value="/cars", method = RequestMethod.POST, produces = "application/json")
public ResponseEntity<List<Car>> list(@RequestBody List<Car> _cars) {

    Iterator<Car> iter = _cars.iterator();

    while (iter.hasNext()) {
        Car curCar = iter.next();
        curCar.setColor(curCar.getColor().toUpperCase());
        curCar.setName(curCar.getName().toUpperCase());
    }

    return new ResponseEntity<List<Car>>(_cars, HttpStatus.OK);
}
````

- body, json
````
[
	{ "name": "bus", "color": "black" }, 
	{ "name": "taxi", "color": "green" }
]
````
http://jinhokwon.tistory.com/8