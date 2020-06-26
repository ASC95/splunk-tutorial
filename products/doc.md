Hello Team, 

Here are my thoughts on specifically how the Splunk app we are developing is going to address the unexpected topology changes use case. I welcome all feedback  

3.0 DaLta Needed for the Use Case 

 

    Supervisory Control and Data Acquisition (SCADA): at present, we are using synthetic SCADA data generated via MATPOWER and Python that models a 9-bus system as shown above. I don’t think we need to consider DNP3 or other SCADA communication protocols for the purposes of this use case. This synthetic SCADA data includes the following dimensions: 

    Datetime: when the measurement was taken (e.g. measurements are taken at 15-minute intervals) 

    Line ID: a unique identifier for a line that is composed of the two bus IDs that connect the line 

    Power: the power on the line as measured in MVA 

    Origin Voltage: the voltage at the first bus 

    Destination Voltage: the voltage at the second bus 

    Outage Management System (OMS): at present, we are using synthetic OMS data generated concurrently with the synthetic SCADA data. The synthetic OMS data includes the following dimensions: 

    Event Datetime: when the event occurred 

    Restoration Datetime: when the event was resolved 

    Line ID: the line that was affected by the event 

    Type: the type of event (e.g. line outage, loss of load, etc.) 

    Bus Affected: the bus that was affected by the event 

    Amount: the amount of power that was affected by the event in MW 

    Sequence of Events (SoE): at present, I am extrapolating from the SoE_Example file provide in the SharePoint site to create a larger SoE dataset that will complement the existing SCADA and OMS data. The synthetic SoE data includes the following dimensions: 

    Date: when the event occurred 

    Time: when the event occurred 

    Element: the device that was affected 

    State: the state of the device at the given date and time 

4.2 Draft User Interface 

    <I would like to implement Jeff’s suggestions before I start pasting things> 

    SCADA 

    OMS 

    SoE 

4.3 Implementation 

    <Describe a scenario. Ideally, the order of events in the scenario will align with the order that the data sources and their respective interfaces are presented in section 4.2> 

4.4 Data Model 

    ??? 