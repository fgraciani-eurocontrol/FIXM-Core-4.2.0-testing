# References to published aeronautical information

Describing the predicted movement of a flight commonly means indicating
which parts of the infrastructure (ATS routes, waypoints, radio
navigation aids etc.) are expected to be used by the flight. This is
enabled in FIXM by a set of “references” constructs. These references
are not flight-specific information; they pertain to the aeronautical
information domain.

Different formats exist for exchanging aeronautical information, whose
usages depend on specific implementation considerations and actual
context within the overall data chain. For instance:

- AIXM is developed in order to enable the provision in digital format
    of the aeronautical information that is in the scope of Aeronautical
    Information Services (AIS). AIXM 5.1 covers both the content of
    Aeronautical Information Publications (AIP) and of the NOTAM
    information.

- The ARINC 424 standard defines a format for navigation (and
    communication) information, including but not limited to, aerodrome,
    runway, navaid, airway and terminal approach procedure information
    that is exchanged between data suppliers and avionics vendors.

- The Aerodrome Mapping Exchange Schema (AMXM XML Schema) is an
    exchange format specification for AMDB as standardized by and
    dedicated to the EUROCAE/RTCA Aeronautical Databases WG44/SC217. It
    is an XML Schema implementation of the DO-291C/ED-119C AMXM UML
    model.

**FIXM does not import any of these formats or profiles thereof.** FIXM
defines its own structures for referring to aeronautical information
that are **self-contained** but mappable to their AIXM/AMXM… equivalents
thanks to the reuse of a common semantics. FIXM also provides an
**optional** mechanism enabling these self-contained references to be
supplemented with, but not replaced by, **hypertext** **references** to
AIXM features.

The following sections provide guidance for correctly encoding these
references in FIXM.

## Generic hypertext references

If an AIXM 5.1 feature exists that corresponds to the element being
referred to, an **optional** hypertext reference to that AIXM feature
may be provided. This reference shall be expressed in accordance with
chapter 3.4.1 (*Abstract references using UUID*) of the [AIXM feature
Identification and
Reference](http://www.aixm.aero/sites/aixm.aero/files/imce/AIXM51/aixm_feature_identification_and_reference-1.0.pdf)
document developed by the AIXM community.

This hypertext reference may be used - or ignored - by the receiving
system depending on its capabilities.

Example:

```xml
<fx:[FIXMelement] href="urn:uuid:81e47548-9f00-4970-b641-8ff8f99098a5">
```

Important note: FIXM does not import the W3C XML Linking Language
(xlink) v1.1 schemas in order to represent the hypertext references.
FIXM mimics the xlink Locator attribute named “href” but defines it
within the FIXM (Base) namespace[5].

## References to Waypoints

<table>
<thead>
<tr class="header">
<th><strong>FIXM Logical Model</strong></th>
<th><strong>FIXM XML schemas</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p><img src=".//media/image13.png" style="width:2.37391in;height:2.98811in" /></p>
<p><em>UML Class <strong>DesignatedPoint</strong> in package FIXM.Base.AeronauticalReference</em></p></td>
<td><p>

```xml
<xs:complexType name="DesignatedPointType">
   <xs:sequence>
      <xs:element name="designator" type="fb:DesignatedPointDesignatorType" minOccurs="1" maxOccurs="1">
      </xs:element>
      <xs:element name="extension" type="fb:DesignatedPointExtensionType" nillable="true" minOccurs="0" maxOccurs="2000">
      </xs:element>
      <xs:element name="position" type="fb:GeographicalPositionType" nillable="true" minOccurs="0" maxOccurs="1">
      </xs:element>
   </xs:sequence>
   <xs:attribute name="href" type="fb:HypertextReferenceType" use="optional">
   </xs:attribute>
</xs:complexType>
```

</p>
<p><em>Complex type <strong>DesignatedPointType</strong> in<br />
file AeronauticalReference.xsd</em></p></td>
</tr>
</tbody>
</table>

### OPTION 1 - Minimum reference

As a minimum, the coded designator of waypoint shall be provided, as
published in the AIPs.

### OPTION 2 - Unambiguous reference

*(OPTION 2 = OPTION 1 + supplementary position information)*

The coded designator of a waypoint is not always sufficient for
unambiguously referring to that element. The 5-letter coded designator
of a waypoint is supposed to be unique world-wide (according to ICAO
Annex 11) but is not in reality. There are at least 5%
duplicates/triplicates/even more…

FIXM adds an optional property 'position' which may be used as a
complement to the 'designator' information in order to remove any
ambiguity on the designator.

Important note: the combinations of fields \[designator + position\]
shall not be interpreted as a natural key uniquely identifying the
waypoint, in so far as producing and consuming systems/services may use
different aeronautical information sources with different degree of
precisions for lat/long, leading to small variations of the position
information. The supplementary fields shall be used for disambiguation
purposes only (i.e. plausibility checks based on position) in case of
duplicate/triplicate/…

### OPTION 3 - Minimum reference with supplementary AIXM pointer

*(OPTION 3 = OPTION 1 + supplementary hypertext reference)*

Option 3 corresponds to Option 1 with an additional hypertext reference
as described in chapter Generic hypertext references.

### OPTION 4 - Complete reference

*(OPTION 4 = OPTION 2 + OPTION 3)*

Option 4 corresponds to the combination of Option 2 and Option 3. See
explanations above.

?> Examples (NOT for OPERATIONAL USE)

The table below depicts examples of FIXM references to fictitious
Waypoint “TEMPO” that is ‘published’ in AIXM 5.1 as part of the
fictitious [Donlon
dataset](https://github.com/aixm/donlon/blob/master/Donlon.xml). The
data is entirely fictitious, located somewhere in the middle of the
Atlantic Ocean. The examples shall NEVER BE USED AS OPERATIONAL DATA.

<table>
<thead>
<tr class="header">
<th></th>
<th><strong>Examples of Waypoint references in FIXM</strong></th>
<th><strong>Notes</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p><strong>OPTION 1</strong></p>
<p>designator</p></td>
<td><p>

```xml
<fb:designatedPoint>
   <fb:designator>TEMPO</fb:designator>
</fb:designatedPoint>
```

</p></td>
<td>This is the minimum reference that SHALL be provided.</td>
</tr>
<tr class="even">
<td><p><strong>OPTION 2</strong></p>
<p>designator<br />
+position</p></td>
<td><p>

```xml
<fb:designatedPoint>
   <fb:designator>TEMPO</fb:designator>
   <fb:position srsName="urn:ogc:def:crs:EPSG::4326">
      <fb:pos>56.84 -29.860000000000003</fb:pos>
   </fb:position>
</fb:designatedPoint>
```

</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p><strong>OPTION 3</strong></p>
<p>designator<br />
+href</p></td>
<td><p>

```xml
<fb:designatedPoint href="urn:uuid:81e47548-9f00-4970-b641-8ff8f99098a5">
   <fb:designator>TEMPO</fb:designator>
</fb:designatedPoint>
```

</p></td>
<td>Hypertext reference to be provided in accordance with the <a href="http://www.aixm.aero/sites/aixm.aero/files/imce/AIXM51/aixm_feature_identification_and_reference-1.0.pdf"><em>AIXM feature Identification and Reference document</em></a>, chapter 3.4.1.</td>
</tr>
<tr class="even">
<td><p><strong>OPTION 4</strong></p>
<p>designator<br />
+position<br />
+href</p></td>
<td><p>

```xml
<fb:designatedPoint href="urn:uuid:81e47548-9f00-4970-b641-8ff8f99098a5">
   <fb:designator>TEMPO</fb:designator>
   <fb:position srsName="urn:ogc:def:crs:EPSG::4326">
      <fb:pos>56.84 -29.860000000000003</fb:pos>
   </fb:position>
</fb:designatedPoint> 
```

</p></td>
<td></td>
</tr>
</tbody>
</table>

### Note about the pattern constraint for waypoint designators

FIXM supports waypoint designators of 1 to 5 characters. This design is
intentional. In most cases, waypoints have a 5-letter designator, as
prescribed by ICAO. However, these designators might be shorter in some
particular cases. Examples of shorter waypoint designators could be
found in various sources:

- In chapter 7 of the ARINC 424 specification. This chapter provides
    rules for forming the designators of waypoints in the absence of
    published designators. These rules can lead to designators of less
    than 5 characters. The following extracts from ARINC 424-19 provide
    some examples, which are **highlighted**.

Examples:

- 3.0 NM from DOOTY should be expressed as **3NM**.
- 2.8 NM from CHASS should be expressed as **28NM**.
- 11.0 NM from BACUP should be expressed as **NM11**.
- 13.8 NM from KITTY should be expressed as **NM138**.

    Fix Ident:  **AB13**

    Fix Name:   AB180013 AB 180.3 degrees 12.8

- In the [US FAA’s publication of Instrument Flight Procedures
    encodings in ARINC 424-18
    format](https://www.faa.gov/air_traffic/flight_info/aeronav/digital_products/cifp/download/).
    For instance:

    - Localizer only approach on RWY06 at KFOD


SUSAP KFODK3FL06 AFOD 010FOD K3D 0V IF 18000 0 NS 854081209

 SUSAP KFODK3FL06 AFOD 020**FF06** K3PC0E TF + 02800 0 NS 854091209

 SUSAP KFODK3FL06 AFOD 030**FF06** K3PC0EE AR PI IFODK3 2430006119800100PI + 02800 0 NS 854101209

 SUSAP KFODK3FL06 AHIMSU 010HIMSUK3PC0E A IF FOD K3 15000140 D 18000 0
 NS 854111310

 SUSAP KFODK3FL06 AHIMSU 020SHRLAK3PC0EE BR AF FOD K3 216501401500 D +
 02800 0 NS 854121310

 SUSAP KFODK3FL06 ALESFI 010LESFIK3PC0E A IF FOD K3 25400140 D 18000 0
 NS 854131310

 SUSAP KFODK3FL06 ALESFI 020SHRLAK3PC0EE BL AF FOD K3 216501402540 D +
 02800 0 NS 854141310

 SUSAP KFODK3FL06 L 010SHRLAK3PC0E I IF IFODK3 24300163 PI + 02800
 18000 0 NS 854151310

 SUSAP KFODK3FL06 L 020**FF06** K3PC0E F CF IFODK3 2430006106300101PI +
 02800 FO K3PN0 NS 854161209

 SUSAP KFODK3FL06 L 021ATLOJK3PC0E S CF IFODK3 2430003406300028PI +
 01820 -324 0 NS 854171310

 SUSAP KFODK3FL06 L 030RW06 K3PG0GY M CF IFODK3 2430001306300021PI
 01134 -324 0 NS 854181209

 SUSAP KFODK3FL06 L 040 0 M CA 0630 + 02800 0 NS 854191209

 SUSAP KFODK3FL06 L 050FOD K3D 0VY L DF + 02800 0 NS 854201209

 SUSAP KFODK3FL06 L 060FOD K3D 0VE R HM 1204T010 + 02800 0 NS 854211209


-   GPS approach on RWY29 at KFOT

 SUSAP KFOTK2FP29 ADINSE 010DINSEK2EA0E A IF 18000 P PS 857251211

 SUSAP KFOTK2FP29 ADINSE 020JEWPEK2PC0E TF + 06000 P PS 857261310

 SUSAP KFOTK2FP29 ADINSE 030SPHERK2PC0EE TF + 04700 P PS 857271310

 SUSAP KFOTK2FP29 AKNEES 010KNEESK2EA0E IF 18000 P PS 857281211

 SUSAP KFOTK2FP29 AKNEES 020YAGERK2EA0E A TF + 06800 P PS 857291211

 SUSAP KFOTK2FP29 AKNEES 030SPHERK2PC0EE TF + 04700 P PS 857301310

 SUSAP KFOTK2FP29 APLYAT 010PLYATK2EA0E A IF 18000 P PS 857311211

 SUSAP KFOTK2FP29 APLYAT 020JEVGYK2PC0E TF + 06000 P PS 857321310

 SUSAP KFOTK2FP29 APLYAT 030SPHERK2PC0EE TF + 04700 P PS 857331310

 SUSAP KFOTK2FP29 P 010SPHERK2PC0E I IF + 04700 18000 P PS 857341310

 SUSAP KFOTK2FP29 P 011JEVSYK2PC0E S TF + 03700 P PS 857351310

 SUSAP KFOTK2FP29 P 020ELLYSK2PC0E F TF + 02700 IZPUH K2PCP PS
 857361310

 SUSAP KFOTK2FP29 P 021**SP29** K2PC0E A TF + 01840 -356 P PS 857371211

 SUSAP KFOTK2FP29 P 030IZPUHK2PC0EY M TF 00617 -356 P PS 857381310

 SUSAP KFOTK2FP29 P 040 0 M CA 2757 + 01500 P PS 857391211

 SUSAP KFOTK2FP29 P 050FOT K2D 0VY R DF + 03000 P PS 857401211

 SUSAP KFOTK2FP29 P 060FOT K2D 0VE R HM 1610T010 + 03000 P PS 857411510

-   In the European AIS Database (EAD). A query on the EAD helped
    identify the following:

    -   40 occurrences of designated points with a 1-letter designator

    -   190 occurrences of designated points with a 2-letters designator

    -   356 occurrences of designated points with a 3-letters designator

    -   3046 occurrences of designated points with a 4-letters
        designator

Most of the designated points with 2, 3 or 4-letter designators
actually correspond to designated points formed after the position of
a navaid or an aerodrome. However, some occurrences cannot be related
to these cases. The following table quotes a few random examples.

| **Designator**        | **Lat**      | **Long**      | **Type** |
|-----------------------|--------------|---------------|----------|
| *1-letter designator* |              |               |          |
| N                     | 503241N      | 0042718E      | ADHP     |
| E                     | 502941N      | 0044206E      | ADHP     |
| A                     | 475826.0000N | 0161445.0000E | OTHER    |
| B                     | 475202.0000N | 0161514.0000E | OTHER    |
| B                     | 475734.0000N | 0161614.0000E | OTHER    |
| *2-letter designator* |              |               |          |
| S1                    | 460217N      | 0142702E      | OTHER    |
| S2                    | 460552N      | 0142748E      | OTHER    |
| S3                    | 460845N      | 0142452E      | OTHER    |
| W1                    | 461846N      | 0141449E      | OTHER    |
| W2                    | 461744N      | 0142049E      | OTHER    |
| A1                    | 453243N      | 0181709E      | OTHER    |
| A2                    | 423842N      | 0175651E      | ADHP     |
| A3                    | 434049N      | 0155502E      | ADHP     |
| *3-letter designator* |              |               |          |
| PJ1                   | 544005N      | 0253057E      | OTHER    |
| PJ2                   | 554243N      | 0211434E      | OTHER    |
| PJ3                   | 561350N      | 0221534E      | OTHER    |
| TP1                   | 481018.5959N | 0113540.1135E | ADHP     |
| TP2                   | 481314.6479N | 0113003.5398E | ADHP     |
| 13A                   | 512900.0000N | 0185100.0000E | OTHER    |
| 17A                   | 520800.0000N | 0174500.0000E | OTHER    |
| 20A                   | 514400.0000N | 0160800.0000E | OTHER    |
| 21A                   | 521300.0000N | 0162800.0000E | OTHER    |
| 27A                   | 523500.0000N | 0160500.0000E | OTHER    |
| ME1                   | 462338N      | 0155433E      | OTHER    |
| ME2                   | 463440N      | 0155009E      | OTHER    |
| DX1                   | 481323.7979N | 0122604.5443E | ADHP     |
| DX1                   | 505951.6149N | 0141519.3372E | ADHP     |
| DX2                   | 505625.7259N | 0141418.8105E | ADHP     |
| *4-letter designator* |              |               |          |
| SJ91                  | 000224S      | 1044205E      | OTHER    |
| FF36                  | 375713.7253S | 1751802.2481E | ADHP     |
| SM01                  | 464002.55N   | 0170535.52E   | ADHP     |
| SM02                  | 464750.28N   | 0165926.87E   | ADHP     |
| LDD5                  | 514138.7284N | 0191615.9435E | ADHP     |
| PG23                  | 545132N      | 0230742E      | OTHER    |
| IF09                  | 431021.1N    | 0252819.1E    | ADHP     |
| MAPT                  | 492818.3679N | 0083229.2734E | ADHP     |
| SDF2                  | 495218.7277N | 0112216.9347E | ADHP     |
| FF16                  | 513503.7055N | 1124320.1363W | ADHP     |
| ECHO                  | 504818N      | 0051950E      | ADHP     |
| ECHO                  | 482129.75N   | 0703422.93W   | OTHER    |

## References to Navaid

<table>
<thead>
<tr class="header">
<th><strong>FIXM Logical Model</strong></th>
<th><strong>FIXM XML schemas</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><img src=".//media/image14.png" style="width:2.33913in;height:2.82482in" /><br />
<em>UML Class <strong>Navaid</strong> in package FIXM.Base. AeronauticalReference</em></td>
<td><p>

```xml
<xs:complexType name="NavaidType">
   <xs:sequence>
      <xs:element name="designator" type="fb:NavaidDesignatorType" minOccurs="1" maxOccurs="1">
      </xs:element>
      <xs:element name="extension" type="fb:NavaidExtensionType" nillable="true" minOccurs="0" maxOccurs="2000">
      </xs:element>
      <xs:element name="navaidServiceType" type="fb:NavaidServiceTypeType" nillable="true" minOccurs="0" maxOccurs="1">
      </xs:element>
      <xs:element name="position" type="fb:GeographicalPositionType" nillable="true" minOccurs="0" maxOccurs="1">
      </xs:element>
   </xs:sequence>
   <xs:attribute name="href" type="fb:HypertextReferenceType" use="optional">
   </xs:attribute>
</xs:complexType>
```

</p>
<p><em>Complex type <strong>NavaidType</strong> in<br />
file AeronauticalReference.xsd</em></p></td>
</tr>
</tbody>
</table>

### OPTION 1 - Minimum reference

As a minimum, the coded designator of a navaid shall be provided, as
published in the AIPs.

### OPTION 2 - Unambiguous reference

*(OPTION 2 = OPTION 1 + supplementary position & navaidServicetype
information)*

The coded designator of a navaid is not always sufficient for
unambiguously referring to that element. The en-route navaids (VOR, DME,
NDB) designator is supposed to be unique (according to ICAO Annex 11)
within 600 NM. This means that these designators are not unique
world-wide. For airport navaids, there is no limitation.

FIXM adds two optional properties 'position' and 'navaidServiceType'
which may be used as a complement to the 'designator' information in
order to remove any ambiguity on the designator.

Important note: the combination of fields \[designator + position +
navaid service type\] shall not be interpreted as a natural key uniquely
identifying the navaid, in so far as producing and consuming
systems/services may use different aeronautical information sources with
different degree of precisions for lat/long, leading to small variations
of the position information. The supplementary fields shall be used for
disambiguation purposes only (i.e. plausibility checks based on position
and navaid service type) in case of duplicate/triplicate/…

### OPTION 3 - Minimum reference with supplementary AIXM pointer

*(OPTION 3 = OPTION 1 + supplementary hypertext reference)*

Option 3 corresponds to Option 1 with an additional hypertext reference
as described in chapter Generic hypertext references.

### OPTION 4 - Complete reference

*(OPTION 4 = OPTION 2 + OPTION 3)*

Option 4 corresponds to the combination of Option 2 and Option 3. See
explanations above.

?> Examples (NOT for OPERATIONAL USE)

The table below depicts examples of FIXM references to fictitious VOR
DME “BOR” that is ‘published’ in AIXM 5.1 as part of the fictitious
[Donlon dataset](https://github.com/aixm/donlon/blob/master/Donlon.xml).
The data is entirely fictitious, located somewhere in the middle of the
Atlantic Ocean. The examples shall NEVER BE USED AS OPERATIONAL DATA.

<table>
<thead>
<tr class="header">
<th></th>
<th><strong>Examples of Navaid references in FIXM</strong></th>
<th><strong>Notes</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p><strong>OPTION 1</strong></p>
<p>designator</p></td>
<td><p>

```xml
<fb:navaid>
   <fb:designator>BOR</fb:designator>
</fb:navaid>
```

</p></td>
<td>This is the minimum reference that SHALL be provided.</td>
</tr>
<tr class="even">
<td><p><strong>OPTION 2</strong></p>
<p>designator<br />
+position<br />
+navaidServiceType</p></td>
<td>

```xml
<fb:navaid>
   <fb:designator>BOR</fb:designator>
   <fb:navaidServiceType>VOR_DME</fb:navaidServiceType>
   <fb:position srsName="urn:ogc:def:crs:EPSG::4326">
      <fb:pos>52.36833333333333 -32.375</fb:pos>
   </fb:position>
</fb:navaid>
```

</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p><strong>OPTION 3</strong></p>
<p>designator<br />
+href</p></td>
<td><p>

```xml
<fb:navaid href="urn:uuid:08a1bbd5-ea70-4fe3-836a-ea9686349495">
   <fb:designator>BOR</fb:designator>
</fb:navaid>
```

</p></td>
<td>Hypertext reference to be provided in accordance with the <a href="http://www.aixm.aero/sites/aixm.aero/files/imce/AIXM51/aixm_feature_identification_and_reference-1.0.pdf"><em>AIXM feature Identification and Reference document</em></a>, chapter 3.4.1.</td>
</tr>
<tr class="even">
<td><p><strong>OPTION 4</strong></p>
<p>designator<br />
+position<br />
+navaidServiceType<br />
+href</p></td>
<td><p>

```xml
<fb:navaid href="urn:uuid:08a1bbd5-ea70-4fe3-836a-ea9686349495">
   <fb:designator>BOR</fb:designator>
   <fb:navaidServiceType>VOR_DME</fb:navaidServiceType>
   <fb:position srsName="urn:ogc:def:crs:EPSG::4326">
      <fb:pos>52.36833333333333 -32.375</fb:pos>
   </fb:position>
</fb:navaid>
```

</p></td>
<td></td>
</tr>
</tbody>
</table>

## References to Aerodromes

<table>
<thead>
<tr class="header">
<th><strong>FIXM Logical Model</strong></th>
<th><strong>FIXM XML schemas</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p><img src=".//media/image15.png" style="width:2.75652in;height:2.14502in" /></p>
<p><em>UML Class <strong>AerodromeReference</strong> in package FIXM.Base.AeronauticalReference</em></p></td>
<td><p>

```xml
<xs:complexType name="AerodromeReferenceType">
   <xs:sequence>
      <xs:element name="extension" type="fb:AerodromeReferenceExtensionType" nillable="true" minOccurs="0" maxOccurs="2000">
      </xs:element>
      <xs:element name="iataDesignator" type="fb:IataAerodromeDesignatorType" nillable="true" minOccurs="0" maxOccurs="1">
      </xs:element>
      <xs:element name="locationIndicator" type="fb:LocationIndicatorType" nillable="true" minOccurs="0" maxOccurs="1">
      </xs:element>
      <xs:element name="name" type="fb:AerodromeNameType" nillable="true" minOccurs="0" maxOccurs="1">
      </xs:element>
      <xs:element name="referencePoint" type="fb:GeographicalPositionType" nillable="true" minOccurs="0" maxOccurs="1">
      </xs:element>
   </xs:sequence>
   <xs:attribute name="href" type="fb:HypertextReferenceType" use="optional">
   </xs:attribute>
</xs:complexType> 
```

</p>
<p><em>Complex types <strong>AerodromeReferenceType</strong> in file AeronauticalReference.xsd</em></p></td>
</tr>
</tbody>
</table>

### OPTION 1 - Minimum reference

The minimum aerodrome reference shall consist of the aerodrome location
indicator, if provided by ICAO Doc 7910 \[11\]. If the aerodrome has no
ICAO Doc 7910 location indicator, the minimum aerodrome reference shall
consist of the name of the aerodrome and its geographical location,
namely the aerodrome reference point.

### OPTION 2 - Minimum reference with supplementary AIXM pointer

*(OPTION 2 = OPTION 1 + supplementary hypertext reference)*

Option 2 corresponds to Option 1 with an additional hypertext reference
as described in chapter Generic hypertext references.

?> Examples (NOT for OPERATIONAL USE)

The table below depicts examples of FIXM references to fictitious
aerodrome “DONLON” that is ‘published’ in AIXM 5.1 as part of the
fictitious [Donlon
dataset](https://github.com/aixm/donlon/blob/master/Donlon.xml). The
data is entirely fictitious, located somewhere in the middle of the
Atlantic Ocean. The examples shall NEVER BE USED AS OPERATIONAL DATA.

<table>
<thead>
<tr class="header">
<th></th>
<th><strong>Examples of aerodrome references in FIXM</strong></th>
<th></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td></td>
<td><strong>CASE 1 – The Aerodrome has an ICAO Doc 7910 location indicator</strong></td>
<td><strong>CASE 2 – The Aerodrome has no ICAO Doc. 7910 location indicator</strong></td>
</tr>
<tr class="even">
<td><p><strong>OPTION 1</strong></p>
<p>minimum</p></td>
<td><p>

```xml
<fx:destinationAerodrome>
  <fb:locationIndicator>EADD</fb:locationIndicator>
</fx:destinationAerodrome>
```

</p></td>
<td><p>

```xml
<fx:destinationAerodrome>
   <fb:name>DONLON</fb:name>
   <fb:referencePoint srsName="urn:ogc:def:crs:EPSG::4326">
      <fb:pos>52.3716666666667 -31.9494444444444</fb:pos>
   </fb:referencePoint>
</fx:destinationAerodrome>
```

</p></td>
</tr>
<tr class="odd">
<td><p><strong>OPTION 2</strong></p>
<p>with href</p></td>
<td><p>

```xml
<fx:destinationAerodrome href="urn:uuid:1b54b2d6-a5ff-4e57-94c2-f4047a381c64">
   <fb:locationIndicator>EADD</fb:locationIndicator>
</fx:destinationAerodrome>
```

</p></td>
<td><p>

```xml
<fx:destinationAerodrome href="urn:uuid:1b54b2d6-a5ff-4e57-94c2-f4047a381c64">
   <fb:name>DONLON</fb:name>
   <fb:referencePoint srsName="urn:ogc:def:crs:EPSG::4326">
      <fb:pos>52.3716666666667 -31.9494444444444</fb:pos>
   </fb:referencePoint>
</fx:destinationAerodrome>
```

</p></td>
</tr>
</tbody>
</table>

Important note: FIXM enables the encoding of richer aerodrome
“reference” structures, such as

```xml
 <fx:destinationAerodrome href="urn:uuid:1b54b2d6-a5ff-4e57-94c2-f4047a381c64">
   <fb:iataDesignator>DLN</fb:iataDesignator>
   <fb:locationIndicator>EADD</fb:locationIndicator>	
   <fb:name>DONLON</fb:name>
   <fb:referencePoint srsName="urn:ogc:def:crs:EPSG::4326">
      <fb:pos>52.3716666666667 -31.9494444444444</fb:pos>
   </fb:referencePoint>
</fx:destinationAerodrome>
```
The provision of the name and reference point in addition to the
location indicator is technically possible but does not serve the
purpose of identifying the aerodrome. This implementation practice only
aims to provide consumers with richer information about the aerodrome
being referred to. Whether or not to use this supplementary information
is at the discretion of the consuming system / service.

## References to Runway Directions

<table>
<thead>
<tr class="header">
<th><strong>FIXM Logical Model</strong></th>
<th><strong>FIXM XML schemas</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p><img src=".//media/image16.png" style="width:2.48696in;height:2.27903in" /></p>
<p><em>UML Class <strong>RunwayDirectionDesignator</strong> in package FIXM.Base.AeronauticalReference</em></p></td>
<td><p>

```xml
<xs:simpleType name="RestrictedRunwayDirectionDesignatorType">
   <xs:restriction base="fb:CharacterStringType">
      <xs:pattern value="(0[1-9]|[12][0-9]|3[0-6])[LRC]{0,1}"/>
   </xs:restriction>
</xs:simpleType>
<xs:complexType name="RunwayDirectionDesignatorType">
   <xs:simpleContent>
      <xs:extension base="fb:RestrictedRunwayDirectionDesignatorType">
         <xs:attribute name="href" type="fb:HypertextReferenceType" use="optional">
         </xs:attribute>
      </xs:extension>
   </xs:simpleContent>
</xs:complexType>
```

</p>
<p><em>Simple type <strong>RunwayDirectionDesignatorType</strong> in<br />
file AeronauticalReference.xsd</em></p></td>
</tr>
</tbody>
</table>

### OPTION 1 - Minimum reference

The minimum Runway Direction reference shall consist of the Runway
Direction designator.

### OPTION 2 - Minimum reference with supplementary AIXM pointer

*(OPTION 2 = OPTION 1 + supplementary hypertext reference)*

Option 2 corresponds to Option 1 with an additional hypertext reference
as described in chapter Generic hypertext references.

?> Examples (NOT for OPERATIONAL USE)

The table below depicts examples of FIXM references to fictitious Runway
Direction “09L” that is ‘published’ in AIXM 5.1 as part of the
fictitious [Donlon
dataset](https://github.com/aixm/donlon/blob/master/Donlon.xml). The
data is entirely fictitious, located somewhere in the middle of the
Atlantic Ocean. The examples shall NEVER BE USED AS OPERATIONAL DATA.

<table>
<thead>
<tr class="header">
<th></th>
<th><strong>Examples of Runway Direction references in FIXM</strong></th>
<th><strong>Notes</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p><strong>OPTION 1</strong></p>
<p>designator</p></td>
<td>

```xml
<fx:runwayDirection>09L</fx:runwayDirection>
```

</td>
<td>This is the minimum reference that SHALL be provided.</td>
</tr>
<tr class="even">
<td><p><strong>OPTION 2</strong></p>
<p>designator<br />
+href</p></td>
<td>

```xml
<fx:runwayDirection href="urn:uuid:c8455a6b-9319-4bb7-b797-08e644342d64">09L</fx:runwayDirection>
```

</td>
<td>Hypertext reference to be provided in accordance with the <a href="http://www.aixm.aero/sites/aixm.aero/files/imce/AIXM51/aixm_feature_identification_and_reference-1.0.pdf"><em>AIXM feature Identification and Reference document</em></a>, chapter 3.4.1.</td>
</tr>
</tbody>
</table>

## References to Enroute ATS routes

<table>
<thead>
<tr class="header">
<th><strong>FIXM Logical Model</strong></th>
<th><strong>FIXM XML schemas</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p><img src=".//media/image17.png" style="width:2.77633in;height:2.42466in" /></p>
<p><em>UML Class <strong>RouteDesignator</strong> in package FIXM.Base.AeronauticalReference</em></p></td>
<td><p>&lt;xs:simpleType name="RestrictedRouteDesignatorType"&gt;</p>
<p>&lt;xs:restriction base="fb:CharacterStringType"&gt;</p>
<p>&lt;xs:pattern value="[A-Z][A-Z0-9]{1,7}"/&gt;</p>
<p>&lt;/xs:restriction&gt;</p>
<p>&lt;/xs:simpleType&gt;</p>
<p>&lt;xs:complexType name="RouteDesignatorType"&gt;</p>
<p>&lt;xs:simpleContent&gt;</p>
<p>&lt;xs:extension base="fb:RestrictedRouteDesignatorType"&gt;</p>
<p>&lt;xs:attribute name="href" type="fb:HypertextReferenceType" use="optional"&gt;</p>
<p>&lt;/xs:attribute&gt;</p>
<p>&lt;/xs:extension&gt;</p>
<p>&lt;/xs:simpleContent&gt;</p>
<p>&lt;/xs:complexType&gt;</p>
<p><em>Simple type <strong>RouteDesignatorType</strong> in<br />
file AeronauticalReference.xsd</em></p></td>
</tr>
</tbody>
</table>

OPTION 1 - Minimum reference

The minimum Enroute ATS Route reference shall consist of the Enroute ATS
Route designator as published in the AIP. Enroute ATS Route designators
are not unique. However, the systematic pairing of an ATS route
designator with a route point (i.e. a significant point belonging to
that ATS route) in FIXM is considered sufficient for enabling
unambiguous identification of the ATS Route being referred to.

OPTION 2 - Minimum reference with supplementary AIXM pointer

*(OPTION 2 = OPTION 1 + supplementary hypertext reference)*

Option 2 corresponds to Option 1 with an additional hypertext reference
as described in chapter Generic hypertext references.

Examples (NOT for OPERATIONAL USE)

The table below depicts examples of FIXM references to fictitious
Enroute ATS Route “UA4” that is ‘published’ in AIXM 5.1 as part of the
fictitious [Donlon
dataset](https://github.com/aixm/donlon/blob/master/Donlon.xml). The
data is entirely fictitious, located somewhere in the middle of the
Atlantic Ocean. The examples shall NEVER BE USED AS OPERATIONAL DATA.

<table>
<thead>
<tr class="header">
<th></th>
<th><strong>Examples of EnRoute ATS Route references in FIXM</strong></th>
<th><strong>Notes</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p><strong>OPTION 1</strong></p>
<p>designator</p></td>
<td><img src=".//media/image2.png" style="width:0.26042in;height:0.26042in" /> &lt;fx:routeDesignator&gt;UA4&lt;/fx:routeDesignator&gt;</td>
<td>This is the minimum reference that SHALL be provided.</td>
</tr>
<tr class="even">
<td><p><strong>OPTION 2</strong></p>
<p>designator<br />
+href</p></td>
<td><img src=".//media/image2.png" style="width:0.26042in;height:0.26042in" />&lt;fx:routeDesignator href="urn:uuid:a14a8751-5428-46bc-a2d1-32ef84d37b5c"&gt;UA4&lt;/fx:routeDesignator&gt;</td>
<td>Hypertext reference to be provided in accordance with the <a href="http://www.aixm.aero/sites/aixm.aero/files/imce/AIXM51/aixm_feature_identification_and_reference-1.0.pdf"><em>AIXM feature Identification and Reference document</em></a>, chapter 3.4.1.</td>
</tr>
</tbody>
</table>

#### References to SIDs and STARs

<table>
<thead>
<tr class="header">
<th><strong>FIXM Logical Model</strong></th>
<th><strong>FIXM XML schemas</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p><img src=".//media/image18.png" style="width:2.71379in;height:1.875in" /></p>
<p><em>UML Classes <strong>SidStarReference</strong> package FIXM.Base.AeronauticalReference</em></p></td>
<td><p>&lt;xs:complexType name="SidStarReferenceType"&gt;</p>
<p>&lt;xs:sequence&gt;</p>
<p>&lt;xs:element name="abbreviatedDesignator" type="fb:SidStarAbbreviatedDesignatorType" nillable="true" minOccurs="0" maxOccurs="1"&gt;</p>
<p>&lt;/xs:element&gt;</p>
<p>&lt;xs:element name="designator" type="fb:SidStarDesignatorType" nillable="true" minOccurs="0" maxOccurs="1"&gt;</p>
<p>&lt;/xs:element&gt;</p>
<p>&lt;xs:element name="extension" type="fb:SidStarReferenceExtensionType" nillable="true" minOccurs="0" maxOccurs="2000"&gt;</p>
<p>&lt;/xs:element&gt;</p>
<p>&lt;/xs:sequence&gt;</p>
<p>&lt;xs:attribute name="href" type="fb:HypertextReferenceType" use="optional"&gt;</p>
<p>&lt;/xs:attribute&gt;</p>
<p>&lt;/xs:complexType&gt;</p>
<p>&lt;xs:simpleType name="SidStarAbbreviatedDesignatorType"&gt;</p>
<p>&lt;xs:restriction base="fb:CharacterStringType"&gt;</p>
<p>&lt;xs:minLength value="1"/&gt;</p>
<p>&lt;xs:maxLength value="6"/&gt;</p>
<p>&lt;xs:pattern value="([A-Z]|[0-9])+([ \+\-/]*([A-Z]|[0-9])+)*"/&gt;</p>
<p>&lt;/xs:restriction&gt;</p>
<p>&lt;/xs:simpleType&gt;</p>
<p>&lt;xs:simpleType name="SidStarDesignatorType"&gt;</p>
<p>&lt;xs:restriction base="fb:CharacterStringType"&gt;</p>
<p>&lt;xs:minLength value="1"/&gt;</p>
<p>&lt;xs:maxLength value="7"/&gt;</p>
<p>&lt;xs:pattern value="([A-Z]|[0-9])+([ \+\-/]*([A-Z]|[0-9])+)*"/&gt;</p>
<p>&lt;/xs:restriction&gt;</p>
<p>&lt;/xs:simpleType&gt;</p>
<p><em>Simple types <strong>SidStarReferenceType</strong> in file<br />
AeronauticalReference.xsd</em></p></td>
</tr>
</tbody>
</table>

OPTION 1 - Minimum reference

The minimum SID or STAR reference shall consist of the SID or STAR
designator as published in the AIP.

OPTION 2 - Minimum reference with supplementary AIXM pointer

*(OPTION 2 = OPTION 1 + supplementary hypertext reference)*

Option 2 corresponds to Option 1 with an additional hypertext reference
as described in chapter Generic hypertext references.

Examples (NOT for OPERATIONAL USE)

The table below depicts examples of FIXM references to SID “AMOLO 5B”
that is published in the French AIPs.

<table>
<thead>
<tr class="header">
<th></th>
<th><strong>Examples of SID references in FIXM</strong></th>
<th><strong>Notes</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p><strong>OPTION 1</strong></p>
<p>designator</p></td>
<td><p><img src=".//media/image2.png" style="width:0.26042in;height:0.26042in" /> &lt;fx:standardInstrumentDeparture&gt;</p>
<p>&lt;fb:designator&gt;AMOLO5B&lt;/fb:designator&gt;</p>
<p>&lt;/fx:standardInstrumentDeparture&gt;</p></td>
<td>This is the minimum reference that SHALL be provided.</td>
</tr>
<tr class="even">
<td><p><strong>OPTION 2</strong></p>
<p>designator<br />
+href</p></td>
<td><p><img src=".//media/image2.png" style="width:0.26042in;height:0.26042in" />&lt;fx:standardInstrumentDeparture href="urn:uuid:..."&gt;</p>
<p>&lt;fb:designator&gt;AMOLO5B&lt;/fb:designator&gt;</p>
<p>&lt;/fx:standardInstrumentDeparture&gt;</p></td>
<td>Hypertext reference to be provided in accordance with the <a href="http://www.aixm.aero/sites/aixm.aero/files/imce/AIXM51/aixm_feature_identification_and_reference-1.0.pdf"><em>AIXM feature Identification and Reference document</em></a>, chapter 3.4.1.</td>
</tr>
</tbody>
</table>

FIXM also supports the supplementary provision of the abbreviated
designator of the SID or the STAR which is commonly used in FMS
databases and in some ground automation systems. The ‘abbreviated
designator’, if provided, should be the designator obtained after
applying the rules for shortening names specified by the ARINC 424
specification, chapter 7.4. Example:

> &lt;fx:standardInstrumentDeparture&gt;
>
> &lt;fb:abbreviatedDesignator&gt;AMOL5B&lt;/fb:abbreviatedDesignator&gt;
>
> &lt;fb:designator&gt;AMOLO5B&lt;/fb:designator&gt;
>
> &lt;/fx:standardInstrumentDeparture&gt;

#### References to Airspace

<table>
<thead>
<tr class="header">
<th><strong>FIXM Logical Model</strong></th>
<th><strong>FIXM XML schemas</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p><img src=".//media/image19.png" style="width:2.4375in;height:1.88479in" /></p>
<p><em>UML Class <strong>AirspaceDesignator</strong> in package FIXM.Base.AeronauticalReference</em></p></td>
<td><p>&lt;xs:simpleType name="RestrictedAirspaceDesignatorType"&gt;</p>
<p>&lt;xs:restriction base="fb:CharacterStringType"&gt;</p>
<p>&lt;xs:minLength value="1"/&gt;</p>
<p>&lt;xs:maxLength value="10"/&gt;</p>
<p>&lt;xs:pattern value="([A-Z]|[0-9]|[, !&amp;quot;&amp;amp;#$%'\(\)\*\+\-\./:;&amp;lt;=&amp;gt;\?@\[\\\]\^_\|\{\}])*"/&gt;</p>
<p>&lt;/xs:restriction&gt;</p>
<p>&lt;/xs:simpleType&gt;</p>
<p>&lt;xs:complexType name="AirspaceDesignatorType"&gt;</p>
<p>&lt;xs:simpleContent&gt;</p>
<p>&lt;xs:extension base="fb:RestrictedAirspaceDesignatorType"&gt;</p>
<p>&lt;xs:attribute name="href" type="fb:HypertextReferenceType" use="optional"&gt;</p>
<p>&lt;/xs:attribute&gt;</p>
<p>&lt;/xs:extension&gt;</p>
<p>&lt;/xs:simpleContent&gt;</p>
<p>&lt;/xs:complexType&gt;</p>
<p><em>Simple type <strong>AirspaceDesignatorType</strong> in<br />
file AeronauticalReference.xsd</em></p></td>
</tr>
</tbody>
</table>

OPTION 1 - Minimum reference

The minimum airspace reference shall consist of the airspace location
indicator, if provided by ICAO Doc 7910 \[11\]. If the airspace has no
ICAO Doc 7910 location indicator, the minimum airspace reference shall
consist of the coded designator of the airspace as published in the AIP.

OPTION 2 - Minimum reference with supplementary AIXM pointer

*(OPTION 2 = OPTION 1 + supplementary hypertext reference)*

Option 2 corresponds to Option 1 with an additional hypertext reference
as described in chapter Generic hypertext references.

Examples (NOT for OPERATIONAL USE)

<table>
<thead>
<tr class="header">
<th></th>
<th><strong>Examples of Airspace references in FIXM</strong></th>
<th></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td></td>
<td><strong>CASE 1 – The Airspace has an ICAO Doc 7910 location indicator</strong></td>
<td><strong>CASE 2 – The Airspace has no ICAO Doc. 7910 location indicator</strong></td>
</tr>
<tr class="even">
<td><p><strong>OPTION 1</strong></p>
<p>designator</p></td>
<td><p><img src=".//media/image2.png" style="width:0.26042in;height:0.26042in" /></p>
<p>&lt;fx:region&gt;KZLC&lt;/fx:region&gt;</p></td>
<td><p><img src=".//media/image2.png" style="width:0.26042in;height:0.26042in" /></p>
<p>&lt;fx:region&gt;AMSWELL&lt;/fx:region&gt;</p></td>
</tr>
<tr class="odd">
<td><p><strong>OPTION 2</strong></p>
<p>designator<br />
+href</p></td>
<td><img src=".//media/image2.png" style="width:0.26042in;height:0.26042in" />&lt;fx:region href="urn:uuid:..."&gt;KZLC&lt;/fx:region&gt;</td>
<td><p>&lt;fx:region href="urn:uuid:..."&gt;AMSWELL&lt;/fx:region&gt;</p>
<p><img src=".//media/image2.png" style="width:0.26042in;height:0.26042in" /></p>
<p><em>(from DONLON dataset)</em></p></td>
</tr>
</tbody>
</table>

#### References to (ATC) Units

<table>
<thead>
<tr class="header">
<th><strong>FIXM Logical Model</strong></th>
<th><strong>FIXM XML schemas</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p><img src=".//media/image20.png" style="width:2.97919in;height:2.14151in" /></p>
<p><em>UML Class <strong>ATCUnitReference</strong> in package FIXM.Base.AeronauticalReference</em></p></td>
<td><p>&lt;xs:complexType name="AtcUnitReferenceType"&gt;</p>
<p>&lt;xs:sequence&gt;</p>
<p>&lt;xs:element name="atcUnitNameOrAlternate" type="fb:TextNameType" nillable="true" minOccurs="0" maxOccurs="1"&gt;</p>
<p>&lt;/xs:element&gt;</p>
<p>&lt;xs:element name="controlSectorDesignator" type="fb:AirspaceDesignatorType" nillable="true" minOccurs="0" maxOccurs="1"&gt;</p>
<p>&lt;/xs:element&gt;</p>
<p>&lt;xs:element name="extension" type="fb:AtcUnitReferenceExtensionType" nillable="true" minOccurs="0" maxOccurs="2000"&gt;</p>
<p>&lt;/xs:element&gt;</p>
<p>&lt;xs:element name="locationIndicator" type="fb:LocationIndicatorType" nillable="true" minOccurs="0" maxOccurs="1"&gt;</p>
<p>&lt;/xs:element&gt;</p>
<p>&lt;xs:element name="position" type="fb:GeographicalPositionType" nillable="true" minOccurs="0" maxOccurs="1"&gt;</p>
<p>&lt;/xs:element&gt;</p>
<p>&lt;/xs:sequence&gt;</p>
<p>&lt;xs:attribute name="href" type="fb:HypertextReferenceType" use="optional"&gt;</p>
<p>&lt;/xs:attribute&gt;</p>
<p>&lt;/xs:complexType&gt;</p>
<p><em>Complex type <strong>ATCUnitReferenceType</strong> in file AeronauticalReference.xsd</em></p></td>
</tr>
</tbody>
</table>

OPTION 1 - Minimum reference

The minimum ATC unit reference shall consist of the location indicator
of the unit, if provided by ICAO Doc 7910 \[11\]. If the unit has no
ICAO Doc 7910 location indicator, the minimum airspace reference shall
consist of the name of the unit or any alternate name, as published in
the AIP.

OPTION 2 - Minimum reference with supplementary AIXM pointer

*(OPTION 2 = OPTION 1 + supplementary hypertext reference)*

Option 2 corresponds to Option 1 with an additional hypertext reference
as described in chapter Generic hypertext references.

Examples (NOT for OPERATIONAL USE)

<table>
<thead>
<tr class="header">
<th></th>
<th><strong>Examples of Unit references in FIXM</strong></th>
<th></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td></td>
<td><strong>CASE 1 – The Unit has an ICAO Doc 7910 location indicator</strong></td>
<td><strong>CASE 2 – The Unit has no ICAO Doc. 7910 location indicator</strong></td>
</tr>
<tr class="even">
<td><p><strong>OPTION 1</strong></p>
<p>designator</p></td>
<td><p>&lt;fx:originator&gt;</p>
<p>&lt;fb:locationIndicator&gt;EBBU&lt;/fb:locationIndicator&gt;</p>
<p><img src=".//media/image2.png" style="width:0.26042in;height:0.26042in" />&lt;/fx:originator&gt;</p></td>
<td><p>&lt;fx:originator&gt;</p>
<p><img src=".//media/image2.png" style="width:0.26042in;height:0.26042in" /> &lt;fb:atcUnitNameOrAlternate&gt;MILITARY DONLON TWR&lt;/fb:atcUnitNameOrAlternate&gt;</p>
<p>&lt;/fx:originator&gt;</p></td>
</tr>
<tr class="odd">
<td><p><strong>OPTION 2</strong></p>
<p>designator<br />
+href</p></td>
<td><p>&lt;fx:originator href="urn:uuid:..."&gt;</p>
<p><img src=".//media/image2.png" style="width:0.26042in;height:0.26042in" /> &lt;fb:locationIndicator&gt;EBBU&lt;/fb:locationIndicator&gt;</p>
<p>&lt;/fx:originator&gt;</p></td>
<td><p>&lt;fx:originator href="urn:uuid:..."&gt;</p>
<p><img src=".//media/image2.png" style="width:0.26042in;height:0.26042in" /> &lt;fb:atcUnitNameOrAlternate&gt;MILITARY DONLON TWR&lt;/fb:atcUnitNameOrAlternate&gt;</p>
<p>&lt;/fx:originator&gt;</p></td>
</tr>
</tbody>
</table>