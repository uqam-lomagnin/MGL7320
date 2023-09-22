<img style="float: right;" src="../../images/component_engineering.svg" alt="EngineeringAISystems" width="250"/>

## MGL7320 - Ingénierie logicielle des systèmes d'IA
# 02 - Apprentissage Machine (ML)

## Prelude

- Quizz - [https://ahaslides.com/CU2S7](https://ahaslides.com/CU2S7)
- Questions / réponses concernant le cours de la semaine dernière

## Lectures du jour
- :bulb: [Apprentissage Machine (ML)](./02_machine_learning.pdf)

## (Various) Data Structures
<details>
<summary>À déplier</summary>

|  Storage | Model |  Similar to | Benefits
|---|---|---|---|
|  Relational Database | Relational | MySQL, Oracle DB, etc. | Complex structures, SQL (Structured Query Language)
|  Text File |  Unstructured |  Plain English, French, etc. | Natural Language
|  CSV Files |  Row-oriented |  Excel | Compact, Splittable
|  [Parquet](https://en.wikipedia.org/wiki/Apache_Parquet) |  Column-oriented |  [Cassandra](https://en.wikipedia.org/wiki/Apache_Cassandra) | Could be more efficient (R/W & Compression) than Row-oriented
|  [MongoDB](https://en.wikipedia.org/wiki/MongoDB) |  Semi-structured |  JSON, XML, YAML, [Avro](https://en.wikipedia.org/wiki/Apache_Avro) | Easy to read (self-describing), flexible

### Relational Database

![DB Tables](https://docs.microsoft.com/en-us/azure/architecture/data-guide/images/example-relational2.png)

- https://docs.microsoft.com/en-us/azure/architecture/data-guide/relational-data/

### Example of the same Data declined in various NoSQL Formats
(taken from [Column-oriented](https://en.wikipedia.org/wiki/Column-oriented_DBMS) Wiki Page)

| RowId	| EmpId	| Lastname	| Firstname	| Salary |
|---|---|---|---|---|
| 001	| 10	| Smith	| Joe	| 40000
| 002	| 12	| 	| Mary | 50000
| 003	| 11	| Johnson	| Cathy	| |
| 004	| 22	| Jones	| Bob	| 55000

#### Row-oriented
```cs
RowId:EmpId,Lastname,Firstname,Salary
001:10,Smith,Joe,40000;
002:12,,Mary,50000;
003:11,Johnson,Cathy;
004:22,Jones,Bob,55000;
```

#### Column-oriented
```cs
10:001,12:002,11:003,22:004;
Smith:001,Johnson:003,Jones:004;
Joe:001,Mary:002,Cathy:003,Bob:004;
40000:001,50000:002,55000:004;
```

#### Semi-structured
##### JSon
JSon is a subset of the JavaScript Object Notation syntax
- data stored in name/value pairs
- records separated by commas
- field names & strings are wrapped by double quotes

``` json
{
  "employees": [
    {
      "EmpId": 10,
      "Lastname": "Smith",
      "Firstname": "Joe",
      "Salary": "40000"
    },
    {
      "EmpId": 12,
      "Firstname": "Mary",
      "Salary": "50000"
    },
    {
      "EmpId": 11,
      "Lastname": "Johnson",
      "Firstname": "Cathy"
    },
    {
      "EmpId": 22,
      "Lastname": "Jones",
      "Firstname": "Bob",
      "Salary": "55000"
    }
  ]
}
```

##### YAML
is a superset of JSON
- `.yml` files [begin with '---', marking the start of the document] (optional)
- key value pairs are separated by colon
- lists begin with a hyphen

``` yaml
---
employees:
- EmpId: 10
  Lastname: Smith
  Firstname: Joe
  Salary: 40000
- EmpId: 12
  Firstname: Mary
  Salary: 50000
- EmpId: 11
  Lastname: Johnson
  Firstname: Cathy
- EmpId: 22
  Lastname: Jones
  Firstname: Bob
  Salary: 55000
```

##### XML
An older format (more verbose, harder to read) that is mostly used for SOAP (abbreviation for Simple Object Access Protocol) exchanges, legacy configuration files, as well as Web applications (XML is similar to HTML)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<root>
   <employees>
      <element>
         <EmpId>10</EmpId>
         <Firstname>Joe</Firstname>
         <Lastname>Smith</Lastname>
         <Salary>40000</Salary>
      </element>
      <element>
         <EmpId>12</EmpId>
         <Firstname>Mary</Firstname>
         <Salary>50000</Salary>
      </element>
      <element>
         <EmpId>11</EmpId>
         <Firstname>Cathy</Firstname>
         <Lastname>Johnson</Lastname>
      </element>
      <element>
         <EmpId>22</EmpId>
         <Firstname>Bob</Firstname>
         <Lastname>Jones</Lastname>
         <Salary>55000</Salary>
      </element>
   </employees>
</root>
```

:bulb: Tip, to convert CSV to JSON, JSON to YAML, etc. online tools are available.
</details>

## Discussion
> :nut_and_bolt: Prod vs Non Prod

## Travail personnel pour la semaine prochaine
- [ ] Prévoir 3 questions / réponses (pertinentes) concernant le cours d'aujourd'hui
  - Certains d'entre-vous devront les poser lors de la prochaine séance
