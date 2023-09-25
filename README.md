# Livestock Event Information Schema
In the cattle life cycle many events happened, some events are related to young cattle like birth, and some are related to Dams or Sires like mating. We collected information about these events from LPA NVD and CSU farmer’s representatives. Some of the events are composite of other events, for example, calving an event by itself is a stand-alone event but it consists of other sub-events such as weight, and once calving happens the weight must happen as well. Another example is the weaning event contains 2 sub-events weight and health treatment, but weight can occur by itself in different timestamps the same thing can happen to health treatment events can occur in different timestamps than the other events. The nature of the event in the real-life application specifies the description (properties) of that event.
The LEI will be extended from the ICAR and ISC schema as there are some fields need to be added such the sessionID and numbers of animals in that session. Moreover, most of the existing ISC and ICAR event need to know from where this event is coming so we will add iscOrganisationType.json in each single event as owner of this property.

# LEI structure

![image](https://github.com/mahirgamal/LEI-schema/assets/86919381/dc10e33b-a65e-4b49-b669-61d54b2d90de)

# Schema Mandatory Information (ICAR and ISC limitations)

| Information                                      | ICAR   | ISC    | Note                                                           |
|--------------------------------------------------|--------|--------|----------------------------------------------------------------|
| `datetime`: "When?"                             | ✅     | ✅     | Events capture date and time.                                |
| `source`: "Where?"                              | ❌     | ❌     | In ICAR/ISC, there are not many details about the source of the event, for example, GPS location details for the device that triggered the event, device brand, or the company name. |
| `owner`: "Who?"                                 | ❌     | ❌     | The ICAR/ISC has no specific field for identifying the animal owner. Although it does have fields for organisations and people, these are not used to indicate the owner of an animal. |
| `message`: "What?"                              |        |        |                                                                |
|   - `item`: "Which?"                            | ✅     | ✅     | Animal details include identification number or tag number, date of birth, breed information, and gender. |
|   - `event`: "Why?"                            | ✅     | ✅     | The details about the event include the animal's health and vaccination history, feeding regimen and nutrition information, weight or body condition score, information about any treatments (such as antibiotics or hormones) that the animal has received, and production data (such as milk yield or meat quality). |


# Event JSON File (eventCore.json)
The "eventCore.json" file is a crucial component in the organisation of data within the proposed data schema. It serves as the central hub that connects all properties and events sub-schemas together, allowing for a cohesive and consistent structure for data validation. This file is the actual JSON schema, meaning it defines the structure and format of the data being processed. It outlines the various fields and their respective data types, as well as any constraints or validation rules that must be met.

## Central Hub
"eventCore.json" serves as a central hub that connects various properties and events sub-schemas. It eliminates the need to validate data against each individual event sub-schema, making data validation more efficient.

![image](https://github.com/mahirgamal/LEI-schema/assets/86919381/a4a05312-e8e5-4d0a-bda2-64222e488d6e)

## Structure of "eventCore.json"

- **$schema and $id:** These fields specify the version of the JSON schema being used and the location of the schema file, respectively.
- **type:** It is set to "object," indicating that this schema defines an object with various properties.
- **description:** Provides a brief explanation of what the schema represents.
- **additionalProperties:** Set to "false," which means that no additional properties beyond those defined in the "properties" field can be added to the object.
- **required:** Lists the properties that must be present in the event object, including "source," "owner," "eventDateTime," and "message." These properties are foundational.

## Properties in "eventCore.json"
- **eventDateTime**
- **"source"** property references an external JSON file "eventSource.json" for hardware or software details.
- **"owner"** property references "eventOwnerType.json" for information about the producer or farmer.
- **"message"** property is a complex object with specific properties, including "eventName," "item," "event," and optional "session."

### Event Date Time
- **eventDateTime:** is the time stamp that the event has been published to the subscriber to consume it.

### Source
Which is shown from where this event is coming, is it coming from the sensor, NLIS system, or anything else that can send an event and the location. The mandatory property is “from”.

"source": {
    "id": "123",
    "ip_address": "128.0.0.0",
    "manufacturer": {
      "id": "1234"
    }
  }
![image](https://github.com/mahirgamal/LEI-schema/assets/86919381/06082e10-2009-427c-905c-e91988674452)

### Owner Property
The "Owner" property encompasses several sub-properties that collectively define the details of the farm or producer. These sub-properties include the Property Identification Code (PIC) for the farm, owner name, email, address, and phone number. Essentially, it provides information about the entity responsible for the event data.

### Message

- **eventName Property:** This property is crucial as it defines what the event is about. Depending on the value of the "eventName" property, specific actions or properties will be selected from the "oneOf" section. In essence, "eventName" serves as a descriptor or label for the nature of the event.
- **Session Property:** In certain scenarios, multiple animals may experience the same type of event within a specific period. For instance, the "weight" event might occur for 50 animals on the same date. The "session" property is used to group such events together, indicating that these events happened within the same session or timeframe. It helps in organizing and categorizing events that occur simultaneously for multiple entities.
- **oneOf Property:** This property is central to the event data structure. It contains multiple sub-properties, and one of them will be selected based on the value of the "event" property. The "oneOf" section defines different possibilities or variations of event data. It ensures that the event data is properly categorized and structured based on the nature of the event.
- **"item" Property:** This property is part of the schema and plays a role in defining the type of item associated with an event. It serves to categorize events into different types, such as animals, crops, or machinery.
    - "itemType.json" File: This external JSON file, named "itemType.json," is referenced by the "item" property. It is responsible for defining and enumerating the various types of items that events can be associated with. For instance, it may list "animal," "crop," and "machinery" as possible item types.
    - "icarAnimalCoreResource.json" File: Within the schema, there is an object structure named "animal," which represents the structure and properties associated with animals. Instead of defining this structure within the schema itself, it references an external JSON file named "icarAnimalCoreResource.json" using the "$ref" keyword. This external file likely contains the detailed schema for animal-related data, including properties like identification numbers, birthdates, breed information, gender, etc.

![image](https://github.com/mahirgamal/LEI-schema/assets/86919381/6c40ba67-65b0-4fd1-b94c-290ddcd810e7)

#### Complex "message" Property
- It contains "if-then-else" statements to check the value of the "itemType" field.
- Depending on the "itemType," the "eventName" can only be selected from specific JSON files.
- The structure and properties of the "animal" object are referenced from an external file "icarAnimalCoreResource.json" using "$ref."
#### "oneOf" Keyword
The "message" property employs the "oneOf" keyword to define various options for the "eventName" property based on different conditions. For example, if "eventName" is "Weight," it specifies that "event" must be an object, and the required definition is found in an external JSON file "leiWeightEvent.json."


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
