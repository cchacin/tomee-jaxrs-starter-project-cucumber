[![Build Status](https://travis-ci.org/cchacin/tomee-jaxrs-starter-project-cucumber.svg)](https://travis-ci.org/cchacin/tomee-jaxrs-starter-project-cucumber)

# Apache TomEE JAX-RS Starter Project + Cucumber

## Add the dependencies:
```xml
<dependency>
  <groupId>com.github.cchacin</groupId>
  <artifactId>cucumber-common-steps</artifactId>
  <version>0.2.11</version>
  <scope>test</scope>
</dependency>
<dependency>
  <groupId>com.github.cukespace</groupId>
  <artifactId>cukespace-core</artifactId>
  <version>1.6.0</version>
  <scope>test</scope>
</dependency>
```


## Run tests with CukeSpace
```java
@Glues({RestSteps.class}) // cucumber common steps to test rest endpoints
@Features({"features/colorservice.feature"}) // feature file(s) to run
@RunWith(ArquillianCucumber.class) // run with arquillian + cucumber
public class ColorServiceTest {

    @Deployment
    public static WebArchive createDeployment() {
        return ShrinkWrap
        .create(WebArchive.class, "test-app.war") // added application path
        .addClasses(ColorService.class, Color.class);
    }
}
```

## Set arquillian port to 8080 in ```src/test/resources/arquillian.xml```

```diff
 <arquillian>
   <container qualifier="tomee" default="true">
     <configuration>
-      <property name="httpPort">-1</property>
+      <property name="httpPort">8080</property>
       <property name="stopPort">-1</property>
       <property name="ajpPort">-1</property>
     </configuration>
   </container>
 </arquillian>
```

## Create the feature file to execute it: ```src/test/resources/features/colorservice.feature```
```gherkin
Feature:
  Scenario: POST and GET
    When I make a POST call to "test-app/color/green" endpoint with post body:
    """
    """
    Then response status code should be 204
    And response should be empty

  Scenario: GET
    When I make a GET call to "test-app/color" endpoint
    Then response content type should be "application/octet-stream"
    And response status code should be 200
    And response should be json:
    """
    green
    """

  Scenario: GET
    When I make a GET call to "test-app/color/object" endpoint
    Then response content type should be "application/json"
    And response status code should be 200
    And response should be json:
    """
    {
      "color": {
        "name": "orange",
        "r": 231,
        "g": 113,
        "b": 0
      }
    }
    """
```