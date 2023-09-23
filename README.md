# Livestock Event Information Schema
In the cattle life cycle many events happened, some events are related to young cattle like birth, and some are related to Dams or Sires like mating. We collected information about these events from LPA NVD and CSU farmer’s representatives. Some of the events are composite of other events, for example, calving an event by itself is a stand-alone event but it consists of other sub-events such as weight, and once calving happens the weight must happen as well. Another example is the weaning event contains 2 sub-events weight and health treatment, but weight can occur by itself in different timestamps the same thing can happen to health treatment events can occur in different timestamps than the other events. The nature of the event in the real-life application specifies the description (properties) of that event.
The LEI will be extended from the ICAR and ISC schema as there are some fields need to be added such the sessionID and numbers of animals in that session. Moreover, most of the existing ISC and ICAR event need to know from where this event is coming so we will add iscOrganisationType.json in each single event as owner of this property.

# LEI structure

![image](https://github.com/mahirgamal/LEI-schema/assets/86919381/e3595ecc-e04f-437c-b695-6eb75a710b79)


![image](https://github.com/mahirgamal/LEI-schema/assets/86919381/a921a9d5-89ff-4513-be56-a251e5c5271c)




# Source
Which is shown from where this event is coming, is it coming from the sensor, NLIS system, or anything else that can send an event and the location. The mandatory property is “from”.

"source": {
    "id": "123",
    "ip_address": "128.0.0.0",
    "manufacturer": {
      "id": "1234"
    }
  }
![image](https://github.com/mahirgamal/LEI-schema/assets/86919381/06082e10-2009-427c-905c-e91988674452)


# Message

●	Message: it contains the actual event details. 
    eventName property value to define what is the event about and according to this value, we must choose one of the properties in the “oneOf “section.
    Producer property contains many other properties to define the Property Identification Code (PIC) for that farm, owner name, email, address, and phone number.
    Session property, sometimes in a period one type of event can occur to many animals for example weight event, it can happen for 50 animals on the same date which means that in the specific session the weight event occurs to 50 animals.
    oneOf is the core of the event, it has many properties and one of them will be chosen according to the value of the event property. It has 2 mandatory properties: the date of the event that happened and the RFID related to the animal who did the event or the event done on it.

![image](https://github.com/mahirgamal/LEI-schema/assets/86919381/6c40ba67-65b0-4fd1-b94c-290ddcd810e7)

# Event Date Time
●	eventDateTime: is the time stamp that the event has been published to the subscriber to consume it.

# Event JSON File (eventCore.json)
The "eventCore.json" file is a crucial component in the organisation of data within the proposed data schema. It serves as the central hub that connects all properties and events sub-schemas together, allowing for a cohesive and consistent structure for data validation. This file is the actual JSON schema, meaning it defines the structure and format of the data being processed. It outlines the various fields and their respective data types, as well as any constraints or validation rules that must be met.

## Central Hub
"eventCore.json" serves as a central hub that connects various properties and events sub-schemas. It eliminates the need to validate data against each individual event sub-schema, making data validation more efficient.

## Structure of "eventCore.json":

- **$schema and $id:** These fields specify the version of the JSON schema being used and the location of the schema file, respectively.

- **type:** It is set to "object," indicating that this schema defines an object with various properties.

- **description:** Provides a brief explanation of what the schema represents.

- **additionalProperties:** Set to "false," which means that no additional properties beyond those defined in the "properties" field can be added to the object.

- **required:** Lists the properties that must be present in the event object, including "source," "owner," "eventDateTime," and "message." These properties are foundational.

## Properties in "eventCore.json":

- **"source"** property references an external JSON file "eventSource.json" for hardware or software details.
- **"owner"** property references "eventOwnerType.json" for information about the producer or farmer.
- **"message"** property is a complex object with specific properties, including "eventName," "item," "event," and optional "session."

![image](https://github.com/mahirgamal/LEI-schema/assets/86919381/a4a05312-e8e5-4d0a-bda2-64222e488d6e)




# Troubleshooting
If you encounter any issues or have questions, feel free to [contact us](#Contact).

# Team

Meet the dedicated team behind LEI:

- [**Mahir Habib**](https://researchoutput.csu.edu.au/en/persons/mahir-habib) - (PhD Student)
- [**Ashad Kabir**](https://researchoutput.csu.edu.au/en/persons/akabircsueduau) - Associate Professor in Computer Science (Project Lead and Principal Supervisor)
- [**Lihong Zheng**](https://researchoutput.csu.edu.au/en/persons/lzhengcsueduau) - Associate Professor in Computer Science (Co-supervisor)

# Contributing
If you find any issues or have suggestions for improvements, please feel free to open an issue or submit a pull request.

# Acknowledgments
We acknowledge that this work is supported by the Trakka project and TerraCipher. We appreciate the support and resources provided by the Trakka project team. Also, we thank Dave Swain and Will Swain from TerraCipher for their guidance and assistance throughout **LEI**.


# License
This project is licensed under Apache License 2.0 - see the [LICENSE][lic] file for details.

# Contact
If you have any questions, suggestions or need assistance, please don't hesitate to contact us at mhabib@csu.edu.au, akabir@csu.edu.au.

[//]: #
  [lic]: <https://github.com/mahirgamal/LEI-schema/blob/main/LICENSE.txt>
