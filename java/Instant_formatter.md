# Instant formatter
````
DateTimeFormatter formatter = DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss")
        					 				   .withZone(ZoneId.systemDefault());
````
````
DateTimeFormatter formatter = DateTimeFormatter.ofLocalizedDateTime(FormatStyle.SHORT)
                     						   .withZone(ZoneId.systemDefault());
````
````
Instant instant = Instant.now();
String time = formatter.format(instant);
````
