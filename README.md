# Livestock Event Information Schema
In the cattle life cycle many events happened, some events are related to young cattle like birth, and some are related to Dams or Sires like mating. We collected information about these events from LPA NVD and CSU farmer’s representatives. Some of the events are composite of other events, for example, calving an event by itself is a stand-alone event but it consists of other sub-events such as weight, and once calving happens the weight must happen as well. Another example is the weaning event contains 2 sub-events weight and health treatment, but weight can occur by itself in different timestamps the same thing can happen to health treatment events can occur in different timestamps than the other events. The nature of the event in the real-life application specifies the description (properties) of that event.
The LEI will be extended from the ICAR and ISC schema as there are some fields need to be added such the sessionID and numbers of animals in that session. Moreover, most of the existing ISC and ICAR event need to know from where this event is coming so we will add iscOrganisationType.json in each single event as owner of this property.

# LEI structure

![image](https://github.com/mahirgamal/LEI-schema/assets/86919381/e3595ecc-e04f-437c-b695-6eb75a710b79)


# Source
Which is shown from where this event is coming, is it coming from the sensor, NLIS system, or anything else that can send an event and the location. The mandatory property is “from”.

"source": {
"from": “optiweigh”,
        "ip_address": “196.10.10.11”,
      	"source_id": 142,
        "source_name":”CSU”,
        "gpsLat": -35.0458,
        "gpsLong": 147.3069
}

# Message

●	Message: it contains the actual event details. 
    eventName property value to define what is the event about and according to this value, we must choose one of the properties in the “oneOf “section.
    Producer property contains many other properties to define the Property Identification Code (PIC) for that farm, owner name, email, address, and phone number.
    Session property, sometimes in a period one type of event can occur to many animals for example weight event, it can happen for 50 animals on the same date which means that in the specific session the weight event occurs to 50 animals.
    oneOf is the core of the event, it has many properties and one of them will be chosen according to the value of the event property. It has 2 mandatory properties: the date of the event that happened and the RFID related to the animal who did the event or the event done on it.

![image](https://user-images.githubusercontent.com/86919381/165871184-0791c12b-5ed6-43da-8130-09abb7e86873.png)

# Event Date Time
●	eventDateTime: is the time stamp that the event has been published to the subscriber to consume it.

# Event JSON File (eventCore.json)
The "eventCore.json" file is a crucial component in the organisation of data within the proposed data schema. It serves as the central hub that connects all properties and events sub-schemas together, allowing for a cohesive and consistent structure for data validation. This file is the actual JSON schema, meaning it defines the structure and format of the data being processed. It outlines the various fields and their respective data types, as well as any constraints or validation rules that must be met.
