# Sliding window

* Time sliding windows -  allows the user to write rules that will only match events occurring in the last X time units. The following code snippet shows the syntax of the operator
* The Sliding Window is only available when engine is running in STREAM MODE

# Configure Drools STREAM Mode

The below configurations will enable the STREAM event processing mode

```xml
<kmodule xmlns="http://www.drools.org/xsd/kmodule" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <kbase name="cepConfigKbase" default="false" eventProcessingMode="stream" equalsBehavior="identity" packages="org.drools.devguide.cep">
    <ksession name="cepConfigKsessionPseudoClock" type="stateful" default="false" clockType="pseudo"/>
    <ksession name="cepConfigKsessionRealtimeClock" type="stateful" default="false" clockType="realtime"/>
  </kbase>
</kmodule>
```
The above file named 'kmodule.xml' is included in the META-INF of your project. Make sure you make it available in the classpath of your main class






