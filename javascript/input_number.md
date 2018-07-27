# input number

- input type number
````
<input type="number" min="1" max="5">
````
- 숫자만 입력
````
$(document).ready(function() {	
	$(".onlynum").keyup(function(){
		$(this).val( $(this).val().replace(/[^0-9]/g,"") 
	);});
});
````
- isNumeric
````
$.isNumeric( "-10" ) // true
$.isNumeric( "+10" ) // true
$.isNumeric( "0" ) // true
$.isNumeric( "0xFF" ) // true
$.isNumeric( "8e5" ) // true
$.isNumeric( "3.1415" ) // true
$.isNumeric( "0144" ) // true
$.isNumeric( ".423" ) // true
 
$.isNumeric( "" ) // false
$.isNumeric( "432,000" ) // false
$.isNumeric( "23,223.002" ) // false
$.isNumeric( "3,23,423" ) // false
$.isNumeric( "-0x42" ) // false
$.isNumeric( "7.2acdgs" ) // false
$.isNumeric( {} ) // false
$.isNumeric( NaN ) // false
$.isNumeric( null ) // false
$.isNumeric( true ) // false
$.isNumeric( false ) // false
$.isNumeric( Infinity ) // false
$.isNumeric( undefined ) // false
````