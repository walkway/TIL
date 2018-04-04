# avro
- Serialization Using Parsers
````
Schema schema = new Schema.Parser().parse(new File("emp.avsc"));
````
````
GenericRecord e1 = new GenericData.Record(schema);
````
````
e1.put("name", "hello");
e1.put("id", 001);
````
````
DatumWriter<emp> empDatumWriter = new SpecificDatumWriter<emp>(emp.class);
````
````
DataFileWriter<emp> dataFileWriter = new DataFileWriter<emp>(empDatumWriter);
````
````
empFileWriter.create(e1.getSchema(), new File("emp.avro"));
````
````
empFileWriter.append(e1);
empFileWriter.append(e2);
empFileWriter.append(e3);
````
https://www.tutorialspoint.com/avro/serialization_using_parsers.htm