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

### type
- Primitive Types
````
null: no value
boolean: a binary value
int: 32-bit signed integer
long: 64-bit signed integer
float: single precision (32-bit) IEEE 754 floating-point number
double: double precision (64-bit) IEEE 754 floating-point number
bytes: sequence of 8-bit unsigned bytes
string: unicode character sequence
````
- Record
````
{
  "type": "record",
  "name": "LongList",
  "aliases": ["LinkedLongs"],                      // old name for this
  "fields" : [
    {"name": "value", "type": "long"},             // each element has a long
    {"name": "next", "type": ["null", "LongList"]} // optional next element
  ]
}
````

- Enum
````
{ "type": "enum",
  "name": "Suit",
  "symbols" : ["SPADES", "HEARTS", "DIAMONDS", "CLUBS"]
}
````

- Array
````
{"type": "array", "items": "string"}
````

- Map
````
{"type": "map", "values": "long"}
````

- Fixed
````
{"type": "fixed", "size": 16, "name": "md5"}
````

https://avro.apache.org/docs/1.8.1/spec.html