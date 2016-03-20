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
        </dependency>
  </dependencies>

            <!--  does this skip work 20/03/2016 15:05:02 fsp
                  see https://github.com/hcoles/pitest/issues/157
                  // configuration not recognised
                  // but "mvn install -DskipTests=true" is NOT executing the scripts 
            <configuration> 
              <skip>${skipTests}</skip> 
            </configuration>            
             -->

The following dependency should also be added:
        <dependency>
            <groupId>org.assertj</groupId>
            <artifactId>assertj-core</artifactId>
            <version>3.3.0</version>
            <scope>test</scope>
        </dependency>        



Add to <build> <plugins> the following
<build>
        <plugins>
            <!-- fsp 20/03/2016 12:38:49, this can not be added by commands in Roo -->
            <!-- start for adding the unit mutation test  -->
            <plugin>
                <groupId>org.pitest</groupId>
                <artifactId>pitest-maven</artifactId>
                <version>1.1.9</version>
                <configuration>
                    <!-- targetClasses if not specified will take by default <project|groupId> -->
                    <targetClasses>
<!--                         <param>training.mvnpitest01*</param>
 -->                    </targetClasses>
                    
                    <!-- targetTests if not specified will take by default <project|groupId> -->
                    <targetTests>
<!--                         <param>training.mvnpitest01*</param> 
 -->
                    </targetTests>
                    <!-- add verbose true (by default false) in this way to get more information if something fails with pitest -->
                    <verbose>false</verbose>
                    <!-- does this work for skipping the tests? 20/03/2016 15:39:27  Not really needed-->
                    <!-- <skip>${skipTests}</skip> -->

                    <!-- this timeoutConstant is the final one we had to set, in order to get it executed without problems 20/03/2016 17:34:25 fsp -->
                    <timeoutConstant>30000</timeoutConstant>

                    <mutators>
                        <mutator>CONSTRUCTOR_CALLS</mutator>
                        <mutator>NON_VOID_METHOD_CALLS</mutator>
                        <mutator>CONDITIONALS_BOUNDARY</mutator>
                        <mutator>INCREMENTS</mutator>
                        <mutator>INVERT_NEGS</mutator>
                        <mutator>MATH</mutator>
                        <mutator>NEGATE_CONDITIONALS</mutator>
                        <mutator>RETURN_VALS</mutator>
                        <mutator>VOID_METHOD_CALLS</mutator>
                        <mutator>CONSTRUCTOR_CALLS</mutator>
                        <mutator>INLINE_CONSTS</mutator>
                        <mutator>NON_VOID_METHOD_CALLS</mutator>
                        <mutator>REMOVE_CONDITIONALS</mutator>
                    </mutators>
                </configuration>
            </plugin>
            <!-- end of adding the unit mutation test  -->


The minimal Maven command to launch a pitest is:
'''mvn org.pitest:pitest-maven:mutationCoverage'''







