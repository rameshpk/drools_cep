# Sliding window

* Time sliding windows -  allows the user to write rules that will only match events occurring in the last X time units. The following code snippet shows the syntax of the operator
* The Sliding Window is only available when engine is running in STREAM MODE

# Configure Drools STREAM Mode

The below configurations will enable the STREAM event processing mode and PseudoClock

```xml
<kmodule xmlns="http://www.drools.org/xsd/kmodule" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <kbase name="cepConfigKbase" default="false" eventProcessingMode="stream" equalsBehavior="identity" packages="org.drools.devguide.cep">
    <ksession name="cepConfigKsessionPseudoClock" type="stateful" default="false" clockType="pseudo"/>
    <ksession name="cepConfigKsessionRealtimeClock" type="stateful" default="false" clockType="realtime"/>
  </kbase>
</kmodule>
```
The above file named 'kmodule.xml' is included in the META-INF of your project. Make sure you make it available in the classpath of your main class

# Event role declaration 

This code can reside inside Model class

* @Role : Declares that the type is to be handled as an event
* @Expire : Events may be automatically expired after some time in the working memory.

```java
import org.kie.api.definition.type.Expires;
import org.kie.api.definition.type.Role;

@Role(Role.Type.EVENT)
@Expires("30m")
public class HeartAttackEvent implements java.io.Serializable {

    static final long serialVersionUID = 1L;

    public HeartAttackEvent() {
        super();
    }
}
```
# CEP Time Sliding Rule 
If there is No heart beat in the last 5 seconds, Know the Symptoms of a Heart Attack.
```DRL
package org.drools.devguide.cep;

import org.drools.devguide.cep.HeartAttackEvent;
import org.drools.devguide.cep.HeartBeatEvent;

rule "No heart beat in the last 5 seconds!"

when
    not (
            HeartBeatEvent()
            over windows:time(5s)
        )
then 
    System.out.println("===Know the Symptoms of a Heart Attack====");
    insert (new HeartAttackEvent());
    drools.halt();
end 
```

# Deploying Event Package to Realtime Decision Server

![eploying Event Package to Realtime Decision Server](https://github.com/rameshpk/drools_cep/blob/master/image/Deploy.png)

# Firing rules using kie server

Fire Rule Post Method : http://<SERVER>:<PORT>/kie-server/services/rest/server/containers/instances/cep-window-over

Input XML  Request 
```xml
<batch-execution lookup="cepConfigKsessionPseudoClock">
  <insert out-identifier="beat" return-object="true" >
    <org.drools.devguide.cep.HeartBeatEvent/>
  </insert>
  <advance-session-time out-identifier="session-advancecurrenttime" amount="1" unit="SECONDS"/>
    <insert out-identifier="beat" return-object="true" >
    <org.drools.devguide.cep.HeartBeatEvent/>
  </insert>
  <advance-session-time out-identifier="session-advancecurrenttime" amount="1" unit="SECONDS"/>
  <insert out-identifier="beat" return-object="true" >
    <org.drools.devguide.cep.HeartBeatEvent/>
  </insert>
   <advance-session-time out-identifier="session-advancecurrenttime" amount="1" unit="SECONDS"/>
    <insert out-identifier="beat" return-object="true" >
    <org.drools.devguide.cep.HeartBeatEvent/>
  </insert>
   <advance-session-time out-identifier="session-advancecurrenttime" amount="6" unit="SECONDS"/>
  <fire-all-rules/>
</batch-execution>
```
We manually advance time 6 minute, without a heart beat
```xml
<advance-session-time out-identifier="session-advancecurrenttime" amount="6" unit="SECONDS"/>
```

Output

![Output](https://github.com/rameshpk/drools_cep/blob/master/image/Output.png)


