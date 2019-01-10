# thymeleaf date

- current date
````
${#dates.format(#dates.createNow(), 'dd MMM yyyy HH:mm')}
````
````
${#calendars.format(#calendars.createNow(), 'dd MMM yyyy HH:mm')}
````