# Exception Handling

````
@ResponseStatus(value=HttpStatus.NOT_FOUND, reason="No such Order")  // 404
public class OrderNotFoundException extends RuntimeException {
 // ...
}
````
````
@RequestMapping(value="/orders/{id}", method=GET)
public String showOrder(@PathVariable("id") long id, Model model) {
	Order order = orderRepository.findOrderById(id);

	if (order == null) throw new OrderNotFoundException(id);

	model.addAttribute(order);
	return "orderDetail";
}
````
https://spring.io/blog/2013/11/01/exception-handling-in-spring-mvc