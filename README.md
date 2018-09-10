# Drools CEP

Complex Event Processing is combine data from multiple source or used to process a large stream of information and can be used for real-time event monitoring. Events can be processed in two ways, which is either in the 'stream' or in the 'cloud' mode

# Event Cloud 

<ul>
<li>It’s default processing mode in BRMS.</li>
   <li>There is no notion of time. No requirements clock synchronization.</li>
<li>The engine looks at the events as an unordered cloud against which engine tries to match rule.</li>
<li>Application must explicitly delete events when they no longer necessary.</li>
<li>No notion of flow of time (Drools support notion of events, which are facts limited life time)</li>
</ul>

![Cloud Event](https://github.com/rameshpk/drools_cep/blob/master/image/Event.png)

# Stream Event

<ul>
<li>Events in each stream must be time-ordered. I.e., inside a given stream, events that happened first must be inserted first into the engine.</li>
<li>In this mode, engine uses a session clock to force synchronization between stream</li>
<li>Session clock is responsible for keeping the current timestamp, which help in </li>
   <ol> <li>Determining event age.</li>
    <li>Do temporal calculations.</li>
    <li>Synchronizes stream form multiple source.</li>
    <li>Schedule future task.</li></ol>
</ul>

![Stream Event](https://github.com/rameshpk/drools_cep/blob/master/image/Stream.png)

# Reference Clock (Session Clock) implementations

![Clock](https://github.com/rameshpk/drools_cep/blob/master/image/Clock.png)
