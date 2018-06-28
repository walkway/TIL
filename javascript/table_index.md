# table index

````
$(document).ready( function() {
    $('table tr').click( function() {
        console.log($(this).index());
    }); 
});
````