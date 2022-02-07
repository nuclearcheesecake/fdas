# Fire Damage Assessment System (Using Oracle PL/SQL and Microsoft Power BI)

<p align="center">
  <img width="470" src="https://github.com/nuclearcheesecake/fdas/blob/main/images/fire.jpg">
</p>

## Table of Contents

1. [Deliverable 1 - The Conceptual Model](#1)
  * 1.1 [The brief](#2)
  * 1.2 [Proposed solution](#3)
2. [Deliverable 2 - Full Database Model and Implementation](#4)
  * 2.1 [The brief](#5)
  * 2.2 [Proposed solution](#6)
  * 2.3 [Database creation and sample data](#7)
3. [Deliverable 1 - BI Design and Implementation](#8)
  * 3.1 [The brief](#9)
  * 3.2 [Thomsen diagram](#10)
  * 3.3 [Sample reports and project dashboard](#11)

<a name="1"></a>
# 1. Deliverable 1 - The Conceptual Model

<a name="2"></a>
## 1.1 The brief

The following brief was received as an initialisation of this project:

```
Your task for this deliverable is to design a database which can be used to support tracking of the building damage caused by 
bushfires - a Fire Damage Assessment System (FDAS).

When a bushfire starts it is noted as a fire event. Each fire event is assigned a unique identifier such as F20200135 and given 
a name such as "West Bush Fire" (fire event names are not unique; the same name may be reused many times). For a given fire, the 
date on which the fire started, the coordinates of the estimated start location (latitude and longitude), and the number of hectares 
which were burnt, are recorded. Sometimes one fire event may spark further fire events, for example when embers travel many 
kilometres in the wind and start a new fire. The FDAS needs to record if a particular fire event was started by sparks from another
fire event. 

Fire events occur within Local Government Areas (LGAs) - a given fire event may involve many LGAs.

A bushfire may impact a property and damage buildings located on that property. The assessment of such damage is the major 
task of the FDAS.

Properties are identified by a unique national property id and have their street address/location and property size in hectares 
recorded as part of the system. Such properties may be a farm property, a residential property (in a city or town) or a business 
property. Each property has a single owner who is identified by an owner id. The owner's name and contact phone number are 
also recorded. A particular owner may own several properties. For properties such as groups of apartments/villas or units owned 
by individuals which share lawns, roads etc (i.e. a sectional title), the owner of the property, for this scenario, will be taken 
as the body corporate. In this way, all properties will be regarded as having only a single owner.

Properties are located in a local government area or LGA. Each LGA is assigned a unique LGA code, for example, 45. The LGA's name, 
for example, "Wardinia", size (in hectares), Chief Executive Officer's name and bushfire contact phone number are recorded.

A property may have one or more buildings located on the property. Each building on the property is assigned a building number for 
that property. These building numbers are reused for each property - for example, property 1234567 may have a building number 1, 
but also property 2134578 may have a building number 1. Some properties may have no buildings located on them. For a building, 
the FDAS must record the date the building was approved for construction and the size of the building in square metres.	

For this system, we will assume that insurance takes place at a building level only. Each building may be insured by an 
insurance company, although not all buildings are covered in this manner. For a given building there will only be a single 
insurance cover. Note the FDAS is not concerned with the actual cover or premium etc, only that a particular insurer insures 
a given building. Each insurer is identified by an insurance code, the insurer's name and contact phone number are also recorded.

Insurance companies employ Insurance Assessors to visit each building which has been damaged by a fire event and evaluate the 
building damage cost. The total dollar cost of the damage to the building is recorded. Only one assessor is used to assess the 
damage of a given building due to a particular fire event. An assessor is identified by an assessor id. The system needs to 
record the assessor’s name, their contact number and the insurer they work for.
```

<a name="3"></a>
## 1.2 Proposed solution


<a name="4"></a>
# 2. Deliverable 2 - Full Database Model and Implementation

<a name="5"></a>
## 2.1 The brief

An update to the scenario:

```
This task continues the work you have started in deliverable 1 by refining/extending the model you developed and implementing for 
the FDAS System

Assignment 2 brief must be read in conjunction with the assignment 1 brief - i.e. your final model must encompass both sets of 
requirements. 

You may modify your assignment 1 conceptual model in any manner you desire as you work through assignment 2, provided your final 
model meets both sets of requirements. 

In developing your final logical data model, any composite attributes present on your conceptual model must be expanded into their 
component simple attributes, unless otherwise directed. If the supplementary material presented in this document does not guide 
you in deciding the components you may make any reasonable decision on their component simple attributes.

---000---

Case Study: 

Further research has revealed that the FDAS must keep full historical details of all building fire damage. The following points 
need to be considered in your further development:

A building may be damaged due to a fire event:
●	If the building is partially damaged the building will be repaired. If further permits are required to facilitate this repair 
the FDAS system will not record their details, the FDAS will only maintain the original approval under which the building was built.
●	If the building is damaged beyond repair the owner may choose to rebuild. If they do rebuild the new building will be assigned 
a new building number for that property. The date the new building was approved for construction and the size of the new building 
in square metres will be recorded within the FDAS.
 
Fire assessors, which are identified by an assessor_id, may change employment from one insurer to another (they cannot assess for 
multiple insurers at the same time). To keep track of the historical employment of a particular assessor the date at which they 
started assessing for a particular company is recorded. When they leave employment with a particular insurer the date they stopped 
assessing for that company is also recorded.

Since the owner name for a property may be an individual’s name, a business name or the name of the body corporate, the FDAS 
requires that the owner name not be treated as a composite attribute. FDAS is also happy to keep the Chief Executive Officer's 
name (administrator) for a Municipality and all contact phone numbers as simple attributes. 

The property types which FDAS currently record are: "farm property, a residential property (in a city or town) or a business 
property". Discussions have indicated that this range of property types needs to be expanded and be such that it can be added 
to/modified as needed on a regular basis.

When a new fire is detected, it is added to the FDAS immediately. Initially, attributes which record the impact of the fire, 
such as area burnt, are set to 0 and will be updated as the fire proceeds and data is gathered. After a fire passes through 
a particular area, first level responders (fire personnel or police officers) move through the area and record details of 
buildings which have been damaged and whether the building has been totally destroyed or not. These details are then added 
into the FDAS. Note that FDAS are not interested in recording details about the first level responders, only the property 
damage they note. 

At a later stage, when the site is safe, insurance companies will schedule assessor visits to those buildings which were 
insured at the time of the fire impact. Assessor visits will be added to the FDAS after the visit has been completed. The 
assessor will provide the damage cost for an insured building when they have completed all necessary visits.

Buildings which are not insured will have their damage cost estimated by the municipality on the basis of the recorded 
municipality evaluation value. 
```

<a name="6"></a>
## 2.2 Proposed solution

<a name="7"></a>
## 2.3 Database creation and sample data

<a name="8"></a>
# 3. Deliverable 3 - BI Design and Implementation

<a name="9"></a>
## 3.1 The brief

The final step:

```
You have been engaged up to now by a company to design a database which can be used to support tracking of the building 
damage caused by bushfires – the Fire Damage Assessment System (FDAS). You followed the Database Implementation phases in 
Assignment 2 where you designed and implemented the logical data model of the requirements as stated by the company. Assignment 
2 concluded the transaction processing database design and implementation phase. 

The next phase and further development for assignment 3 will solely take place on the strategic level of the company’s 
decision processing system as well as the business intelligence application and supporting data warehouse. 

Following the case study, you will identify the reporting requirements of the user. You will produce a User Interface (UI) 
and the supporting multi-dimensional model in the form of Thomsen diagram. You will then produce a multidimensional model 
specifying a data warehouse database schema in the form of a star-schema. You will also need to consider the ETL process and 
implement of the system on a strategic level.
```

<a name="10"></a>
## 3.2 Thomsen diagram

<a name="11"></a>
## 3.3 Sample reports and project dashboard
