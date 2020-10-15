# Relative points

<table>
<thead>
<tr class="header">
<th><strong>FIXM Logical Model</strong></th>
<th><strong>FIXM XML schemas</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p><img src=".//media/image21.png" style="width:2.73585in;height:3.49573in" /></p>
<p><em>UML Class <strong>RelativePoint</strong> in package FIXM.Base.AeronauticalReference</em></p></td>
<td><p>

```xml
<xs:complexType name="RelativePointType">
   <xs:sequence>
      <xs:element name="bearing" type="fb:BearingType" minOccurs="1" maxOccurs="1">
      </xs:element>
      <xs:element name="distance" type="fb:DistanceType" minOccurs="1" maxOccurs="1">
      </xs:element>
      <xs:element name="extension" type="fb:RelativePointExtensionType" nillable="true" minOccurs="0" maxOccurs="2000">
      </xs:element>
      <xs:element name="position" type="fb:GeographicalPositionType" nillable="true" minOccurs="0" maxOccurs="1">
      </xs:element>
      <xs:element name="referencePoint" type="fb:NavaidType" minOccurs="1" maxOccurs="1">
      </xs:element>
   </xs:sequence>
</xs:complexType>
```

</p>
<p><em>Complex type <strong>RelativePointType</strong> in<br />
file AeronauticalReference.xsd</em></p></td>
</tr>
</tbody>
</table>

A relative point is a bearing and distance from a reference navaid.
Encoding a relative point in FIXM requires the ‘bearing’, ‘distance’ and
‘referencePoint’ properties to be provided. All of these properties
shall be provided.

FIXM enables a relative point to be supplemented with an optional
‘position’ value for storing the actual position of the relative point
if already known. The exchange of this information may prove useful in
order to save consuming systems / services from (re)computing the
position of the relative point. Whether or not to use this supplementary
information is at the discretion of the consuming system / service.

?> Examples (NOT for OPERATIONAL USE)

The table below depicts examples of FIXM encodings of relative points
that are derived from the fictitious [Donlon
dataset](https://github.com/aixm/donlon/blob/master/Donlon.xml). The
data is entirely fictitious, located somewhere in the middle of the
Atlantic Ocean. The examples shall NEVER BE USED AS OPERATIONAL DATA.

<table>
<thead>
<tr class="header">
<th></th>
<th><strong>Examples of relative point in FIXM</strong></th>
<th><strong>Notes</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><strong>Without position information</strong></td>
<td><p>

```xml
&lt;fb:relativePoint&gt;</p>
<p>&lt;fb:bearing uom="DEG" zeroBearingType="MAGNETIC_NORTH"&gt;82.0&lt;/fb:bearing&gt;</p>
<p>&lt;fb:distance uom="NM"&gt;2.0&lt;/fb:distance&gt;</p>
<p>&lt;fb:referencePoint&gt;</p>
<p>&lt;fb:designator&gt;BOR&lt;/fb:designator&gt; (*)</p>
<p>&lt;/fb:referencePoint&gt;</p>
<p>&lt;/fb:relativePoint&gt;
```
</p></td>
<td>(*) See chapter References to Navaid above. All four options can be used for encoding this reference. OPTION 1 is used in this example.</td>
</tr>
<tr class="even">
<td><strong>With position information</strong></td>
<td><p>

```xml
&lt;fb:relativePoint&gt;</p>
<p>&lt;fb:bearing uom="DEG" zeroBearingType="TRUE_NORTH"&gt;180&lt;/fb:bearing&gt;</p>
<p>&lt;fb:distance uom="NM"&gt;60.0&lt;/fb:distance&gt;</p>
<p>&lt;fb:position srsName="urn:ogc:def:crs:EPSG::4326"&gt;</p>
<p>&lt;fb:pos&gt;51.36833333333333 -32.375&lt;/fb:pos&gt;</p>
<p>&lt;/fb:position&gt;</p>
<p>&lt;fb:referencePoint&gt;</p>
<p>&lt;fb:designator&gt;BOR&lt;/fb:designator&gt; (*)</p>
<p>&lt;/fb:referencePoint&gt;</p>
<p>&lt;/fb:relativePoint&gt;
```

</p></td>
<td></td>
</tr>
</tbody>
</table>