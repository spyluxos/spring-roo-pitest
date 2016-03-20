# spring-roo-pitest
how to congigure the maven pom.xml file in Spring Roo projects

# warning
The information mentioned here is not yet confirmed; but the following steps lead to a correct (but maybe not sufficient or necessary) configuration.

# what needs to be configured
## pom.xml file (Maven)

in Spring Roo, you can with the following statements change the Maven pom.xml file:
'''dependency add --groupId org.pitest --artifactId pitest-maven --version 1.1.9'''
but this will not be sufficient.

it will add
  <dependencies>
        <dependency>
            <groupId>org.pitest</groupId>
            <artifactId>pitest-maven</artifactId>
            <version>1.1.9</version>
            <!--  does this skip work 20/03/2016 15:05:02 fsp
                  see https://github.com/hcoles/pitest/issues/157
                  // configuration not recognised
                  // but "mvn install -DskipTests=true" is NOT executing the scripts 
            <configuration> 
              <skip>${skipTests}</skip> 
            </configuration>            
             -->
        </dependency>
  </dependencies>


The minimal Maven command to launch a pitest is:
'''mvn org.pitest:pitest-maven:mutationCoverage'''







