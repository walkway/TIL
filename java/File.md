# File
````
File directory =new File("path");
File[] files = directory.listFiles();
for(File file : fileList) {
  if(file.isFile()) {
    System.out.println("file name: " + file.getName());
  }
}
````