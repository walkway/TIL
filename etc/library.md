# commonmark

- Java library for parsing and rendering Markdown text according to the CommonMark specification (and some extensions).
````
<dependency>
    <groupId>org.commonmark</groupId>
    <artifactId>commonmark</artifactId>
    <version>0.20.0</version>
</dependency>
````

- example
````
import org.commonmark.node.*;
import org.commonmark.parser.Parser;
import org.commonmark.renderer.html.HtmlRenderer;

Parser parser = Parser.builder().build();
Node document = parser.parse("This is *Sparta*");
HtmlRenderer renderer = HtmlRenderer.builder().build();
renderer.render(document);  // "<p>This is <em>Sparta</em></p>\n"
````
````
Parser parser = Parser.builder().build();
Node document = parser.parse("## This is *Sparta*");
TextContentRenderer renderer = TextContentRenderer.builder().build();
renderer.render(document);  // "This is Sparta"
````

https://github.com/commonmark/commonmark-java
