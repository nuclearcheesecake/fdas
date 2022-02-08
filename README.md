# Fire Damage Assessment System (Using Oracle PL/SQL and Microsoft Power BI)

<p align="center">
  <img width="470" src="https://github.com/nuclearcheesecake/fdas/blob/main/images/fire.jpg">
</p>

As our final university project for a module on advanced databases, my team and I created a database and generated reports for the tracking of fires in different municipalities. This system stored information on the location and spread ("sparking") of new fires, as well as data on properties in the district. This could be used to warn property owners, as well as by insurers to prepare for potential claims.

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

<p align="center">
  <img width="470" src="https://github.com/nuclearcheesecake/fdas/blob/main/images/assign1.png">
</p>

<a name="2"></a>
## 1.1 The brief

The following brief was received as an initialisation of this project:

```
Your task for this deliverable is to design a database which can be used to support tracking of 
the building damage caused by bushfires - a Fire Damage Assessment System (FDAS).

When a bushfire starts it is noted as a fire event. Each fire event is assigned a unique identifier 
such as F20200135 and given a name such as "West Bush Fire" (fire event names are not unique; the 
same name may be reused many times). For a given fire, the date on which the fire started, the 
coordinates of the estimated start location (latitude and longitude), and the number of hectares which 
were burnt, are recorded. Sometimes one fire event may spark further fire events, for example when embers 
travel many kilometres in the wind and start a new fire. The FDAS needs to record if a particular fire 
event was started by sparks from anotherfire event. 

Fire events occur within Local Government Areas (LGAs) - a given fire event may involve many LGAs.

A bushfire may impact a property and damage buildings located on that property. The assessment of such 
damage is the major task of the FDAS.

Properties are identified by a unique national property id and have their street address/location and 
property size in hectares recorded as part of the system. Such properties may be a farm property, a 
residential property (in a city or town) or a business property. Each property has a single owner who is 
identified by an owner id. The owner's name and contact phone number are also recorded. A particular 
owner may own several properties. For properties such as groups of apartments/villas or units owned by 
individuals which share lawns, roads etc (i.e. a sectional title), the owner of the property, for this 
scenario, will be taken as the body corporate. In this way, all properties will be regarded as having 
only a single owner.

Properties are located in a local government area or LGA. Each LGA is assigned a unique LGA code, for 
example, 45. The LGA's name, for example, "Wardinia", size (in hectares), Chief Executive Officer's 
name and bushfire contact phone number are recorded.

A property may have one or more buildings located on the property. Each building on the property is 
assigned a building number for that property. These building numbers are reused for each property - for 
example, property 1234567 may have a building number 1, but also property 2134578 may have a building 
number 1. Some properties may have no buildings located on them. For a building, the FDAS must record 
the date the building was approved for construction and the size of the building in square metres.	

For this system, we will assume that insurance takes place at a building level only. Each building may 
be insured by an insurance company, although not all buildings are covered in this manner. For a given 
building there will only be a single insurance cover. Note the FDAS is not concerned with the actual 
cover or premium etc, only that a particular insurer insures a given building. Each insurer is identified 
by an insurance code, the insurer's name and contact phone number are also recorded.

Insurance companies employ Insurance Assessors to visit each building which has been damaged by a fire 
event and evaluate the building damage cost. The total dollar cost of the damage to the building is recorded. 
Only one assessor is used to assess the damage of a given building due to a particular fire event. An 
assessor is identified by an assessor id. The system needs to record the assessor’s name, their contact 
number and the insurer they work for.
```

<a name="3"></a>
## 1.2 Proposed solution

My team and I then created the following ERD in response to this brief:

<p align="center">
  <img width="750" src="https://github.com/nuclearcheesecake/fdas/blob/main/images/erd.png">
</p>

<a name="4"></a>
# 2. Deliverable 2 - Full Database Model and Implementation

<p align="center">
  <img width="470" src="https://github.com/nuclearcheesecake/fdas/blob/main/images/assign2.png">
</p>

<a name="5"></a>
## 2.1 The brief

For the next phase, we receive an update to the scenario:

```
This task continues the work you have started in deliverable 1 by refining/extending the model you 
developed and implementing for the FDAS System

Assignment 2 brief must be read in conjunction with the assignment 1 brief - i.e. your final model 
must encompass both sets of requirements. 

You may modify your assignment 1 conceptual model in any manner you desire as you work through 
assignment 2, provided your final model meets both sets of requirements. 

In developing your final logical data model, any composite attributes present on your conceptual 
model must be expanded into their component simple attributes, unless otherwise directed. If the 
supplementary material presented in this document does not guide you in deciding the components 
you may make any reasonable decision on their component simple attributes.

---000---

Case Study: 

Further research has revealed that the FDAS must keep full historical details of all building 
fire damage. The following points need to be considered in your further development:

A building may be damaged due to a fire event:
●	If the building is partially damaged the building will be repaired. If further permits are 
required to facilitate this repair the FDAS system will not record their details, the FDAS will
only maintain the original approval under which the building was built.
●	If the building is damaged beyond repair the owner may choose to rebuild. If they do rebuild 
the new building will be assigned a new building number for that property. The date the new 
building was approved for construction and the size of the new building in square metres will 
be recorded within the FDAS.
 
Fire assessors, which are identified by an assessor_id, may change employment from one insurer 
to another (they cannot assess for multiple insurers at the same time). To keep track of the 
historical employment of a particular assessor the date at which they started assessing for a 
particular company is recorded. When they leave employment with a particular insurer the date 
they stopped assessing for that company is also recorded.

Since the owner name for a property may be an individual’s name, a business name or the name of 
the body corporate, the FDAS requires that the owner name not be treated as a composite attribute. 
FDAS is also happy to keep the Chief Executive Officer's name (administrator) for a Municipality 
and all contact phone numbers as simple attributes. 

The property types which FDAS currently record are: "farm property, a residential property (in a 
city or town) or a business property". Discussions have indicated that this range of property types 
needs to be expanded and be such that it can be added to/modified as needed on a regular basis.

When a new fire is detected, it is added to the FDAS immediately. Initially, attributes which record 
the impact of the fire, such as area burnt, are set to 0 and will be updated as the fire proceeds and 
data is gathered. After a fire passes through a particular area, first level responders (fire personnel 
or police officers) move through the area and record details of buildings which have been damaged and 
whether the building has been totally destroyed or not. These details are then added into the FDAS. 
Note that FDAS are not interested in recording details about the first level responders, only the 
property damage they note. 

At a later stage, when the site is safe, insurance companies will schedule assessor visits to those 
buildings which were insured at the time of the fire impact. Assessor visits will be added to the 
FDAS after the visit has been completed. The assessor will provide the damage cost for an insured 
building when they have completed all necessary visits.

Buildings which are not insured will have their damage cost estimated by the municipality on the 
basis of the recorded municipality evaluation value. 
```

<a name="6"></a>
## 2.2 Proposed solution

We first compile a new list of assumptions:

### Assumptions from Assignment 1
* No Property is so big that it lies in more than one LGA.
* An LGA needs at least one property on it, or the idea of a government
there is absurd.
* A fire event can only be sparked by one other such event.
* An insurer may insure multiple buildings, but at least one, or there is no
point in including the company in the fdas.
* A building can only be damaged by one fire event at a time. Since a
building can be damaged more than once at different times by different
fires, its damage assessment is stored in a different table.

### Assumptions from ‘Fire_Damage’ entity in Question 1
* Total damage could be an indication of whether the building was partially or
totally destroyed, but ultimately each building has a distinct value before
the damage, and the partial damage of one building could equal the total
damage of another. Thus, we assume that there is no transitive
relationship between ‘Total_damage’ and ‘Totally’ in the Fire_damage
entity.
* We can only assess the total damage and the degree to which the building
was destroyed (‘Totally’ attribute) for a given building after a specific fire.
Thus these two attributes require three primary keys to uniquely identify
each entry.
* The initial/default value for the fire_area attribute is set to 0.

### Assumptions from ‘Assessment’ entity in Question 1
* ‘Assessment’ is shorthand for ‘Assessment of Fire Damage’, and thus
combines the ‘Assessment’ and ‘Fire_Damage’ entities from Question 1’s
two different sample documents. We combine these two entities since they
share the same primary keys, and also since one cannot record the totaldamage and whether the building was fully destroyed (the two attributes
from ‘Fire_Damage’) without making a professional assessment.
* A damage assessment needs to be identified not only by the specific
building which is assessed, but also the specific fire event. This allows the
same building to be assessed multiple times for different fires. The building
in turn is identified by its property and building ID. Thus the assessment for
a specific building for a specific fire will have a composite primary key
consisting of three attributes.
* A visit is determined by where the building is (thus property and building
IDs as explained above) but also when the assessor arrives. A separate
entity allows for multiple visits.
* If a person wants to look up which assessor visited the building for a visit,
the building and property IDs from the Visit entity can be used to find the
assessor in the Assessment entity.

### Assumptions from ‘LGA’ entity in logical model
* It is assumed that an LGA and municipality are equivalent.
Assumptions from ‘Property’ entity in logical model
* Properties are assumed to either be “Agricultural”, “Business” or “Residential”.
However, according to the new specifications, these may have to be expanded on.
We have made use of an attribute domain for the prop_type attribute so that if any
expansions must be made, the new values can be added to the attribute domain.
This ensures data integrity as well as the ability to add additional types of properties
in future.

### Assumptions from ‘Insurer’ entity in logical model
* It is assumed that not every building is insured against fire damage.
* Because the municipality is responsible for assessing the damage done to an
uninsured building, we also assume that not all assessors are employed by an
insurance company. Thus, the foreign key insure_code can be left as null.

### Assumptions from 'Assessor' in logical model
* A new assessor entry (with a new assessor ID) is added to the Assessor
entity whenever the assessor changes insurer as occupation. 

### Assumptions from 'Building', 'Assessment' and 'Visit' in logical
model
* A new building entry (with a new building number) is added to the Building
entity whenever a building is rebuilt. The old building’s building_destroyed
attribute is set to True, to indicate that the old building is no longer in use
and a new one has taken/is taking its place.
* The extent of the damage is handled within the Assessment entity in the
TOTALLY attribute. This attribute has an attribute domain, which contains
the values “Totalled” and “Partial”.
* Surrogate key was used for the Visit entity’s primary key.

<a name="7"></a>
## 2.3 Database creation and sample data

We created a database for this project that resulted in the following logical model:

<p align="center">
  <img width="600" src="https://github.com/nuclearcheesecake/fdas/blob/main/images/logic.png">
</p>

The following code was used to create the database:

```sql
DROP TABLE assessment CASCADE CONSTRAINTS;

DROP TABLE assessor CASCADE CONSTRAINTS;

DROP TABLE building CASCADE CONSTRAINTS;

DROP TABLE fire_event CASCADE CONSTRAINTS;

DROP TABLE insurer CASCADE CONSTRAINTS;

DROP TABLE lga CASCADE CONSTRAINTS;

DROP TABLE prop_owner CASCADE CONSTRAINTS;

DROP TABLE property CASCADE CONSTRAINTS;

DROP TABLE visits CASCADE CONSTRAINTS;

spool "FDAS_SCHEMA_OUTPUT.txt";

CREATE TABLE assessment (
    ass_id                      VARCHAR2(4) NOT NULL,
    damage_cost                 NUMBER(2) NOT NULL,
    ass_date                    DATE NOT NULL,
    assessor_assessor_id        VARCHAR2(3) NOT NULL,
    building_building_id        VARCHAR2(2) NOT NULL,
    totally                     VARCHAR2(20) NOT NULL,
    building_property_prop_id   VARCHAR2(7) NOT NULL,
    fire_event_fire_id          VARCHAR2(9) NOT NULL
);

ALTER TABLE assessment
    ADD CHECK ( totally IN (
        'Partial',
        'Totalled'
    ) );

COMMENT ON COLUMN assessment.ass_id IS
    'Unique identifier.';

COMMENT ON COLUMN assessment.damage_cost IS
    'Estimated damage cost after assessment.';

COMMENT ON COLUMN assessment.ass_date IS
    'Date on which assessment was finalised.';

COMMENT ON COLUMN assessment.totally IS
    'Extent of damage. Values are chosen from an attribute domain. Can either take on the value "Totalled" or "Partial".';

ALTER TABLE assessment ADD CONSTRAINT assessment_pk PRIMARY KEY ( ass_id );

CREATE TABLE assessor (
    assessor_id           VARCHAR2(3) NOT NULL,
    assessor_name         VARCHAR2(50) NOT NULL,
    asr_contact           VARCHAR2(10) NOT NULL,
    insurer_insure_code   VARCHAR2(4),
    ass_end               DATE,
    ass_start             DATE NOT NULL,
    assessor_surn         CHAR(30 CHAR) NOT NULL
);

COMMENT ON COLUMN assessor.assessor_id IS
    'Unique identifier.';

COMMENT ON COLUMN assessor.assessor_name IS
    'Name of the assessor.';

COMMENT ON COLUMN assessor.asr_contact IS
    'Contact number of assessor.';

COMMENT ON COLUMN assessor.ass_end IS
    'Date of employment termination.';

COMMENT ON COLUMN assessor.ass_start IS
    'Date of employment beginning.';

COMMENT ON COLUMN assessor.assessor_surn IS
    'The surname of the assessor.';

ALTER TABLE assessor ADD CONSTRAINT assessor_pk PRIMARY KEY ( assessor_id );

CREATE TABLE building (
    building_id           VARCHAR2(2) NOT NULL,
    build_approvaldate    DATE NOT NULL,
    build_size            NUMBER(2) NOT NULL,
    build_val             NUMBER(2) NOT NULL,
    build_destroyed       CHAR(1) NOT NULL,
    insurer_insure_code   VARCHAR2(4),
    property_prop_id      VARCHAR2(7) NOT NULL,
    insurance             CHAR(1) NOT NULL,
    building_class        CHAR(3 CHAR) NOT NULL
);

COMMENT ON COLUMN building.building_id IS
    'Unique Identifier for each building.';

COMMENT ON COLUMN building.build_approvaldate IS
    'Date on which the building was approved.';

COMMENT ON COLUMN building.build_size IS
    'Size of the building in square meters.';

COMMENT ON COLUMN building.build_val IS
    'Value of the building as per the municipality.';

COMMENT ON COLUMN building.build_destroyed IS
    'Indicates whether the building has been destroyed (historical data) or if it is still in use.';

COMMENT ON COLUMN building.insurance IS
    'Indicates whether the building is insured against fire damage or not.';

COMMENT ON COLUMN building.building_class IS
    'The class of the building.';

ALTER TABLE building ADD CONSTRAINT building_pk PRIMARY KEY ( building_id,
                                                              property_prop_id );

CREATE TABLE fire_event (
    fire_id              VARCHAR2(9) NOT NULL,
    fire_name            VARCHAR2(50) NOT NULL,
    date_started         DATE NOT NULL,
    fire_lat             NUMBER(10) NOT NULL,
    fire_long            NUMBER(10) NOT NULL,
    fire_area            NUMBER(2) DEFAULT 0 NOT NULL,
    fire_event_fire_id   VARCHAR2(9) NOT NULL,
    lga_lga_code         VARCHAR2(2) NOT NULL,
    lives_lost           INTEGER NOT NULL,
    total_damage         NUMBER(38) NOT NULL
);

ALTER TABLE fire_event ADD CHECK ( fire_area = 0 );

COMMENT ON COLUMN fire_event.fire_id IS
    'Primary key for each fire.';

COMMENT ON COLUMN fire_event.fire_name IS
    'Name of fire.';

COMMENT ON COLUMN fire_event.date_started IS
    'Date the fire started.';

COMMENT ON COLUMN fire_event.fire_lat IS
    'Latitudinal co-ordinates of the fire.';

COMMENT ON COLUMN fire_event.fire_long IS
    'Longitudinal co-ordinate of the fire.';

COMMENT ON COLUMN fire_event.fire_area IS
    'Area burnt by the fire in square meters. Set to 0 by default.';

COMMENT ON COLUMN fire_event.fire_event_fire_id IS
    'Primary key of the child fire started by the initial fire.';

COMMENT ON COLUMN fire_event.lives_lost IS
    'The total amount of lives the fire cost.';

COMMENT ON COLUMN fire_event.total_damage IS
    'The total damage caused by the specific fire event.';

CREATE UNIQUE INDEX fire_event__idx ON
    fire_event (
        fire_event_fire_id
    ASC );

ALTER TABLE fire_event ADD CONSTRAINT fire_event_pk PRIMARY KEY ( fire_id );

CREATE TABLE insurer (
    insure_code      VARCHAR2(4) NOT NULL,
    insure_name      VARCHAR2(50) NOT NULL,
    insure_contact   VARCHAR2(10) NOT NULL
);

COMMENT ON COLUMN insurer.insure_code IS
    'Unique identifier.';

COMMENT ON COLUMN insurer.insure_name IS
    'Name of insurance company.';

COMMENT ON COLUMN insurer.insure_contact IS
    'Contact number of insurance company.';

ALTER TABLE insurer ADD CONSTRAINT insurer_pk PRIMARY KEY ( insure_code );

CREATE TABLE lga (
    lga_code      VARCHAR2(2) NOT NULL,
    lga_name      VARCHAR2(50) NOT NULL,
    lga_size      NUMBER(2),
    lga_ceo       VARCHAR2(50) NOT NULL,
    lga_contact   VARCHAR2(10) NOT NULL
);

COMMENT ON COLUMN lga.lga_code IS
    'Code of LGA.';

COMMENT ON COLUMN lga.lga_name IS
    'Name of LGA.';

COMMENT ON COLUMN lga.lga_size IS
    'Size of the LGA in square meters.';

COMMENT ON COLUMN lga.lga_ceo IS
    'Name of the CEO of the LGA.';

COMMENT ON COLUMN lga.lga_contact IS
    'Phone number of the CEO of the LGA.';

ALTER TABLE lga ADD CONSTRAINT lga_pk PRIMARY KEY ( lga_code );

CREATE TABLE prop_owner (
    own_id        VARCHAR2(2) NOT NULL,
    own_name      VARCHAR2(50) NOT NULL,
    own_contact   VARCHAR2(10) NOT NULL
);

COMMENT ON COLUMN prop_owner.own_id IS
    'Unique identifier.';

COMMENT ON COLUMN prop_owner.own_name IS
    'Name of owner.';

COMMENT ON COLUMN prop_owner.own_contact IS
    'Contact number of owner.';

ALTER TABLE prop_owner ADD CONSTRAINT prop_owner_pk PRIMARY KEY ( own_id );

CREATE TABLE property (
    prop_id             VARCHAR2(7) NOT NULL,
    adr_street          VARCHAR2(200) NOT NULL,
    prop_size           NUMBER(2),
    prop_type           VARCHAR2(20) NOT NULL,
    lga_lga_code        VARCHAR2(2) NOT NULL,
    prop_owner_own_id   VARCHAR2(2) NOT NULL,
    adr_town            VARCHAR2(200 CHAR) NOT NULL,
    adr_code            VARCHAR2(200 CHAR) NOT NULL
);

ALTER TABLE property
    ADD CHECK ( prop_type IN (
        'Agricultural',
        'Business',
        'Residential'
    ) );

COMMENT ON COLUMN property.prop_id IS
    'Unique identifier.';

COMMENT ON COLUMN property.adr_street IS
    'Address of the property.';

COMMENT ON COLUMN property.prop_size IS
    'Size of the property in square meters.';

COMMENT ON COLUMN property.prop_type IS
    'Type of property. Attribute domain was used to ensure that this attribute can only take on the values of "Agricultural","Business" or "Residential". Potential expansion of this list was discussed, and so if it is necessary to add more options for this attribute, the options will simply be added to the attribute domain.'
    ;

COMMENT ON COLUMN property.lga_lga_code IS
    'Equivalent to the ''mun_code'' in the normalisation, since we assumed that Municipality and LGA are interchangeable.';

COMMENT ON COLUMN property.adr_town IS
    'The specific town that the property lies in.';

COMMENT ON COLUMN property.adr_code IS
    'The code used in the address.';

ALTER TABLE property ADD CONSTRAINT property_pk PRIMARY KEY ( prop_id );

CREATE TABLE visits (
    visit_arrive        DATE NOT NULL,
    visit_id            VARCHAR2(3) NOT NULL,
    visit_depart        DATE NOT NULL,
    assessment_ass_id   VARCHAR2(4) NOT NULL,
    visits_id           NUMBER NOT NULL
);

COMMENT ON COLUMN visits.visit_arrive IS
    'The date and time of arrival per visit.';

COMMENT ON COLUMN visits.visit_id IS
    'Surrogate unique Identifier.';

COMMENT ON COLUMN visits.visit_depart IS
    'Date and time of visit departure.';

ALTER TABLE visits ADD CONSTRAINT visits_pk PRIMARY KEY ( visits_id );

ALTER TABLE visits ADD CONSTRAINT visits_visit_id_un UNIQUE ( visit_id );

ALTER TABLE assessment
    ADD CONSTRAINT assessment_assessor_fk FOREIGN KEY ( assessor_assessor_id )
        REFERENCES assessor ( assessor_id );

ALTER TABLE assessment
    ADD CONSTRAINT assessment_building_fk FOREIGN KEY ( building_building_id,
                                                        building_property_prop_id )
        REFERENCES building ( building_id,
                              property_prop_id );

ALTER TABLE assessment
    ADD CONSTRAINT assessment_fire_event_fk FOREIGN KEY ( fire_event_fire_id )
        REFERENCES fire_event ( fire_id );

ALTER TABLE assessor
    ADD CONSTRAINT assessor_insurer_fk FOREIGN KEY ( insurer_insure_code )
        REFERENCES insurer ( insure_code );

ALTER TABLE building
    ADD CONSTRAINT building_insurer_fk FOREIGN KEY ( insurer_insure_code )
        REFERENCES insurer ( insure_code );

ALTER TABLE building
    ADD CONSTRAINT building_property_fk FOREIGN KEY ( property_prop_id )
        REFERENCES property ( prop_id );

ALTER TABLE fire_event
    ADD CONSTRAINT fire_event_fire_event_fk FOREIGN KEY ( fire_event_fire_id )
        REFERENCES fire_event ( fire_id );

ALTER TABLE fire_event
    ADD CONSTRAINT fire_event_lga_fk FOREIGN KEY ( lga_lga_code )
        REFERENCES lga ( lga_code );

ALTER TABLE property
    ADD CONSTRAINT property_lga_fk FOREIGN KEY ( lga_lga_code )
        REFERENCES lga ( lga_code );

ALTER TABLE property
    ADD CONSTRAINT property_prop_owner_fk FOREIGN KEY ( prop_owner_own_id )
        REFERENCES prop_owner ( own_id );

ALTER TABLE visits
    ADD CONSTRAINT visits_assessment_fk FOREIGN KEY ( assessment_ass_id )
        REFERENCES assessment ( ass_id );

CREATE SEQUENCE visits_visits_id_seq START WITH 1 NOCACHE ORDER;

CREATE OR REPLACE TRIGGER visits_visits_id_trg BEFORE
    INSERT ON visits
    FOR EACH ROW
    WHEN ( new.visits_id IS NULL )
BEGIN
    :new.visits_id := visits_visits_id_seq.nextval;
END;
/
```

And then we inserted sample data, which displayed as:

<p align="center">
  <img width="600" src="https://github.com/nuclearcheesecake/fdas/blob/main/images/sample.png">
</p>

<a name="8"></a>
# 3. Deliverable 3 - BI Design and Implementation

<p align="center">
  <img width="470" src="https://github.com/nuclearcheesecake/fdas/blob/main/images/assign3.png">
</p>

<a name="9"></a>
## 3.1 The brief

The final step:

```
You have been engaged up to now by a company to design a database which can be used to support 
tracking of the building damage caused by bushfires – the Fire Damage Assessment System (FDAS). 
You followed the Database Implementation phases in Assignment 2 where you designed and implemented 
the logical data model of the requirements as stated by the company. Assignment 2 concluded the 
transaction processing database design and implementation phase. 

The next phase and further development for assignment 3 will solely take place on the strategic 
level of the company’s decision processing system as well as the business intelligence application 
and supporting data warehouse. 

Following the case study, you will identify the reporting requirements of the user. You will produce 
a User Interface (UI) and the supporting multi-dimensional model in the form of Thomsen diagram. You 
will then produce a multidimensional model specifying a data warehouse database schema in the form of 
a star-schema. You will also need to consider the ETL process and implement of the system on a 
strategic level.
```

<a name="10"></a>
## 3.2 Thomsen diagrams

Each fire has the following dimensions:
* Location (where it took place, thus its LGA, latitude and longitude, and its name,
since the name corresponds to the what3words standard of location identification).
* Damage (extent of the damage, thus the total lives lost, fire damage done, area of
the fire, and whether the fire sparked other fires).
* Time (when the fire started, thus the day, month and year).

There exists a hierarchy in both the time and locational data:

<p align="center">
  <img width="420" src="https://github.com/nuclearcheesecake/fdas/blob/main/images/hiera.png">
</p>

The assessment also has different dimensions:

* Performer (who does the assessment, thus the assessor ID, which then links to a
specific insurer)
* Target (what is being assessed, thus the property ID and building ID, but also the fire
ID, since that helps uniquely identify the specific assessment)
* Value (the extent of the damage being assessed, thus whether the building was
totally destroyed and the damage cost)
* Time (when the assessment was finalised)

Thus two logical units that need to be modelled are that of the fire event and the
assessment, as they represent two processes that generate data in the database.

<p align="center">
  <img width="350" src="https://github.com/nuclearcheesecake/fdas/blob/main/images/fireevent.png">
</p>


<p align="center">
  <img width="470" src="https://github.com/nuclearcheesecake/fdas/blob/main/images/assess.png">
</p>

Seeing the database according to these schemas is helpful in creating inferences from the
data. Further dimensions can be drawn up for each entity, eg. for properties, we can add a
dimension of location, or where the building is located, and a dimension for its owner. Now
we can, for example, model the location and thus spread of a fire using the dimensions
above, but also coincide that with the locational dimension of a property. This allows us to
alert property owners with nearby properties of the danger and extent of fires by using the
damage dimension of the fire.

The damage and value attributes of the fire and assessment entity respectively can also
work in tandem, as an insurer can use the total damage caused by a fire when assessing the
amount of claim to be paid out for specific buildings.

By drilling down and rolling up with the location hierarchy of the fire, different reports for
different audiences can also be obtained. When we look at the location of a fire at an
LGA-level, the CEO of that LGA could draw inferences from what is happening in his/her
municipality. But for the purposes of this report, we are more interested in drilling down to the
specific locations, seeing the spread of the fires and coinciding these locations with insured
properties. The insurers/assessors can use this data, to see where properties are damaged
that need assessment, and track their claims per region.

<a name="11"></a>
## 3.3 Sample reports and project dashboard

<p align="center">
  <img width="470" src="https://github.com/nuclearcheesecake/fdas/blob/main/images/screen.png">
</p>

To see the full sample report and dashboard, please click [HERE](https://github.com/nuclearcheesecake/fdas/blob/main/Sample%20Report%20and%20Dashboard.pdf). 

### Explanation of report components

<p align="center">
  <img width="525" src="https://github.com/nuclearcheesecake/fdas/blob/main/images/report1.png">
</p>

<p align="center">
  <img width="525" src="https://github.com/nuclearcheesecake/fdas/blob/main/images/report2.png">
</p>

<p align="center">
  <img width="525" src="https://github.com/nuclearcheesecake/fdas/blob/main/images/report3.png">
</p>

<p align="center">
  <img width="525" src="https://github.com/nuclearcheesecake/fdas/blob/main/images/report4.png">
</p>

<p align="center">
  <img width="525" src="https://github.com/nuclearcheesecake/fdas/blob/main/images/report5.png">
</p>

### Explanation of dashboard components

<p align="center">
  <img width="525" src="https://github.com/nuclearcheesecake/fdas/blob/main/images/dash1.png">
</p>

<p align="center">
  <img width="525" src="https://github.com/nuclearcheesecake/fdas/blob/main/images/dash2.png">
</p>

<p align="center">
  <img width="525" src="https://github.com/nuclearcheesecake/fdas/blob/main/images/dash3.png">
</p>

<p align="center">
  <img width="525" src="https://github.com/nuclearcheesecake/fdas/blob/main/images/dash4.png">
</p>

<p align="center">
  <img width="525" src="https://github.com/nuclearcheesecake/fdas/blob/main/images/dash5.png">
</p>

### Report UI Specifications

The following UI guidelines (obtained from REF) were used to guide the design of the
reports:

1. Visibility of system status
Each page contains a page number out of the total number of pages, and thus the user gets a view of how far the reports they are reading are progressing, and they won’t feel lost in the system.

2. Match between system and the real world
This heuristic is applied by the use of real-world maps. For example, the map which indicates each LGA in the ‘Municipal damage report’. This creates a better idea of where each affected LGA is in the user’s mind, instead of just being data in a table.

3. User control and freedom
At the top of each page, a reset button is present. This is for if users scroll around in maps or put the spotlight on specific elements and don’t know how to return to the default view.

4. Consistency and standards
Each report uses the same header in the same format, and includes the buttons (eg. return) in the left-hand corner. Furthermore, the fonts and styles of equivalent text is the same on each page (eg. titles of graphs and tables).

5. Error preventions
Each page contains an informational button at the top, which the user can hover over to see information about the page and its navigation, in order to prevent errors.

6. Recognition rather than recall
The format of each report is the same and has useful titles, and thus the user knows what to expect of the document, and doesn’t have to search for what they want and try and recall where they last saw it. There is also no real interaction necessary to use the system, and thus the user will not have to recall more than being able to scroll.

7. Flexibility and efficiency of use
Since these reports are static, the only requirement is scrolling between pages, and is thus efficient since it has a high pay-off (analysed information) for minimal interaction. It is also flexible, since as new fires are entered in the future, the reports adapt and changeautomatically as new data is connected. Thus the reports are flexible and will always be timely. The reports can also drill down, for example, to a specific fire.

8. Aesthetic and minimalist design
During the design of the “look” of the reports, a lot of attention was given to minimalism. There is no clutter, but rather a neat title and organised report divided by a single line, with only a single logo for visual appeal in the right-hand corner. Titles of graphs and tables were trimmed to only contain necessary information. Every piece of information on each page serves a purpose, and nothing is redundant.

9. Help users recognise, diagnose, and recover from errors
The probability of the user making an error while scrolling is quite small, but the reset button once again addresses this if the user scrolled too far away when using a map.

10. Help and documentation
Tooltips display over most graphs and tables, providing extra information on the relevancy and content of each element. For example, hovering over a fire shows you its information.
