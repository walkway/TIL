# select option value 

A data-* attribute on an <option> tag attaches additional data to the option item.
To create a custom attribute, replace * with a lowercase string, such as data-id, data-status, or data-location.
  
````
<select name="opt1" id="opt1">
    <option value="main1" data-sub="sub1">main text 1</option>
    <option value="main2" data-sub="sub2">main text 2</option>
</select>

<script>
    $(function () {
        $("#opt1").on("change", function () {
            var value = $(this).val();
            var subValue = $(this).find("option:selected").data("sub");
        });
    });
</script>
````
