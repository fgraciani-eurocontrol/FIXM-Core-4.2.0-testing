# Geographical positions

FIXM captures the concept of Geographical Position as defined by ICAO
Annex 15.

*Position (geographical). Set of coordinates (latitude and longitude)
referenced to the mathematical reference ellipsoid which define the
position of a point on the surface of the Earth.*

This model element maps to the ISO 19107 “Point” construct, defined as a
single location given by a direct position.

<table>
<thead>
<tr class="header">
<th><strong>FIXM Logical Model</strong></th>
<th><strong>FIXM XML schemas</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p><img src=".//media/general-guidance-geographical-positions-01.png" style="width:2.88042in;height:1.1381in" /></p>
<p><em>UML Class <strong>GeographicalPosition</strong> in package FIXM.Base.</em>AeronauticalReference</p></td>
<td><p>

```xml
<xs:complexType name="GeographicalPositionType">
    <xs:sequence>
        <xs:element name="extension" type="fb:GeographicalPositionExtensionType" nillable="true" minOccurs="0" maxOccurs="2000">
        </xs:element>
        <xs:element name="pos" type="fb:LatLongPosType" minOccurs="1" maxOccurs="1">
        </xs:element>
    </xs:sequence>
    <xs:attribute name="srsName" type="xs:string" use="required" fixed="urn:ogc:def:crs:EPSG::4326">
    </xs:attribute>
</xs:complexType>
```

<em>Complex type <strong>GeographicalPositionType</strong> in file<br />
AeronauticalReference.xsd</em></p></td>
</tr>
</tbody>
</table>

A geographic location consists of a co-ordinate reference system and
geographic co-ordinates.

## Co-ordinate reference system

ICAO Annex 11 chapter 2.29.1 states that World Geodetic System — 1984
(WGS-84) shall be used as the horizontal (geodetic) reference system for
air navigation. The Coordinate Reference System reference is critical
for the correct encoding and processing of FIXM positions*. This is
because a CRS not only indicates the geodetic datum and ellipsoid for
which point coordinates are expressed but also the order of the
coordinate axes in which coordinate values are provided, e.g. latitude
before longitude – which is an important convention for the aviation
domain.* \[copied from [OGC
12-028r1](https://portal.opengeospatial.org/files/?artifact_id=62061)\]
The EPSG:4326 CRS is the recommended choice for AIXM 5.1 data sets that
use the WGS-84 reference datum.

FIXM implements a fixed co-ordinate reference system: `urn:ogc:def:crs:EPSG::4326`.

## Geographic co-ordinates

The EPSG:4326 CRS has latitude as the primary axis, which indicates that
**the order of the values in the fb:pos element is** **first latitude**,
**second longitude**. This ordering convention is the one applied to the
aviation domain.

The co-ordinates are represented in FIXM by a two-valued sequence[4],
the first being the latitude and the second being the longitude, each of
which is a floating point number (the decimal value in degrees). The
direction is determined by the sign of the value, as specified in the
next table.

| Sign     | Latitude | Longitude |
|----------|----------|-----------|
| Positive | N        | E         |
| Negative | S        | W         |

Note the latitude and longitude values are encoded as double in FIXM.
Imposition of range restriction (-90≤latitude≤90, -180≤longitude≤180)
does not appear in the model since different elements of the sequence of
values have different constraints.

## Examples

?> Example

```xml
<fb:position srsName="urn:ogc:def:crs:EPSG::4326">
    <fb:pos>59.0 -30.0</fb:pos>
</fb:position>
```

?> Example

```xml
<fx:point4D srsName="urn:ogc:def:crs:EPSG::4326">
    <fb:pos>50.03330555555556 8.570455555555556</fb:pos>
[...]
```

On EXAMPLE 1 above, number `59.0` represents the latitude and number
`-30.0` represents the longitude.

## Miscellaneous

The W3C XML schema 1.0 specification defines three special values for
float/double: positive infinity, negative infinity and not-a-number. In
this context, a `pos` element expressed as `<fb:pos>INF -INF</fb:pos>` or `<fb:pos>NaN NaN</fb:pos> ` would be syntactically correct; it would validate against the core FIXM XML
schemas. However, it would not represent any plausible location. The use
of these special values is therefore not accepted when exchanging
geographical positions in FIXM.

## Why FIXM does not use the Geography Markup Language (GML)

FIXM does not adopt the GML standard for the representation of
geospatial data. The reasons for not adopting GML are the following:

-   Wherever a GML dependency is introduced, there would be a need for
    users to use the GML schemas and therefore to understand GML, which
    increases implementation costs.

-   The Flight Planning community is not traditionally familiar with
    geospatial concepts. The introduction of GML would become an
    important drawback for FIXM adoption in support of FF-ICE/R1.

-   Introducing a dependency on GML would make FIXM more difficult to
    implement particularly in certain environments. For instance, .NET
    technologies have been identified as *incompatible* with the GML
    usage.