# Medicine extension

## Description

In the framework of the pandemic, it was possible to observe how the purchase of prescribed drugs for the treatment of COVID-19 patients increased worldwide, including in some countries a result of them was achieved. Considering the supply of these drugs, in some cases prices increased by up to 400 percent.

Verifying the variation in the prices of the same medicine over time, or between countries, is not an easy task and this difficulty favors speculation on prices by the pharmaceutical industry.

Currently, many of the data on the drugs purchased are published in plain text format, or it is only possible to verify them in the terms of reference, which makes their analysis difficult. Publishing data against a single standard will be able to make high-quality price references for identical products, helping to meet the vital need for cross-country comparability that is not always seen in public procurement monitoring.

## Background

With the objective of developing an extension of the OCDS standard for public drug procurement processes, a study was carried out on the way in which drug purchase data is currently published in different countries, the use that users make of it, and the standards of medicines, substances and drugs that exist.

The study included interviews with publishers and users who relate to drug purchases in different ways:

1. I1: Publisher and system maintenance
2. I2: Publisher, write ToR and analysts
3. I3: Publisher, journalist, system developer and analysts
4. I4: Analysts and Software development
5. I5: Analysts 
6. I6: Analysts and reports
7. I7: Publisher, write ToR
8. I8: Publisher, write ToR, analysts and system development
9. I9: Publisher and system development
10. I10: Analysts, advocacy and accountability

In addition, the following standards for the identification of drugs were analyzed:

| Standard        | Maintains       |  Purpose                        |
| -----------     | -----------    |  -----------                      |
| [ATC](https://www.whocc.no/atc_ddd_index/)   | [WHO](https://www.who.int/home)         | medicine classification|
| [Schema.org](https://schema.org/Drug)        | [SCHEMA.ORG COMMUNITY GROUP](https://www.w3.org/community/schemaorg/)         | medicine classification|
| [INN](https://www.who.int/teams/health-product-and-policy-standards/inn/)        | [WHO](https://www.who.int/home)         | medicine names|
| [MSH Products Price Guide](https://www.msh.org/resources/international-medical-products-price-guide)        | [Management Sciences for Health](https://www.msh.org/about-us)         | medicine list|
| [SNOMED-CT](https://bioportal.bioontology.org/ontologies/SNOMEDCT?p=classes&conceptid=410942007) | [National Center for Biomedical Ontology](https://ncbo.bioontology.org/about-ncbo) | medicine classification|
| [LME MSPY](https://www.mspbs.gov.py/dependencias/dggies/adjunto/db7bee-ListadodeMedicamentosEsenciales.pdf) | [Ministerio de Salud del Paraguay](https://www.mspbs.gov.py/index.php) | medicine list|
| [Catálogo CBM](http://www.csg.gob.mx/contenidos/priorizacion/cuadro-basico/med/catalogos.html)| [Consejo de Salubridad General de México](http://www.csg.gob.mx/index.html) | medicine list|
| [UNSPSC](https://ncbo.bioontology.org/about-ncbo) | [United Nations](https://www.un.org/en/) |classification of products and services|
| [Catálogo de Productos Farmacéuticos](http://observatorio.digemid.minsa.gob.pe/Precios/ProcesoL/Catalogo/CatalogoProductos.aspx)|[Ministerio de Salud de Perú](https://www.gob.pe/minsa/)| classification of products and services|

This analysis resulted in the list of attributes of the extension.

## Proposal:
### Codelists

* Dosage Form
    * Tablet
    * Liquid
    * Suspension
    * Injection
    * Cream
    * Ear Drops
    * Gas
    * Gel
    * Inhaler
    * Powder
    * Lotion
    * Spray
    * Suppository
    * Test


* Route of Administration
    * Implant
    * Inhalation
    * Instillation
    * Nasal
    * Oral
    * Parenteral
    * Rectal
    * Sublingual / Buccal / Oromucosal
    * Transdermal
    * Vaginal
    

* Dosage Unit
    * Gram
    * Miligram
    * Microgram
    * Milliliter
    * Millimole
    * Unit
    * Thousand Units
    * Million Units


* Container
    * Jar
    * Blister
    * Blister Vial
    * Dropper Bottle
    * Pre-Filled Syringe
    * Tube
    * Compressed
    * Cartridge

Add an object named "medicineAttributes" in "item":

### Schema

* Dosage {Object}
    * numericValue (integer)
    * unit (string) (codelist)

* DosageForm (string) (codeList)

* RouteOfAdministration (string) (codelist)

* Container {Object}
    * id (string, integer)
    * name (string)
    * capacity 
        * $ref : #/definitions/Dosage

* ActiveIngredient {Object}
    * id (string, integer)
    * name (string)
    * dosage 
        * $ref : #/definitions/Dosage

* Item {Object}
    * MedicineAttributes {Object}
        * activeIngredients (array)
            * $ref : #/definitions/ActiveIngredient
        * dosageForm
            * $ref: #/definitions/DosageForm
        * routeOfAdministration 
            * $ref: #/definitions/RouteOfAdministration
        * container 
            * $ref: #/definitions/Container

### Examples
These are examples of how this extension can be used to describe medicine items in procurement processes with a single o more Active Ingredients:

```
  "item" : {
    "medicineAttributes" : {   
      "dosageForm" :  "liquid",
      "routeOfAdministration" : "P",
      "container" : {
        "id" : 1
        "name" : "jar",
        "capacity: {
          "numericValue" : 1,
          "unit" : "ml"
        }
      },
      "activeIngredient" : [
        {
          "id" : 1,
          "name" : "Midazolam",
          "dosage" : {
            "numericValue" : 5,
            "unit" : "mg"
          }
        }
      ]        
    }
  }
```

```
  "item" : {
    "medicineAttributes" : {   
      "dosageForm" :  "tablet",
      "routeOfAdministration" : "O",
      "container" : {
        "id" : 2
        "name" : "compressed",
        "capacity: {
          "numericValue" : 12,
          "unit" : "U"
        }
      },
      "activeIngredient" : [
        {
          "id" : 2,
          "name" : "Lamivudina",
          "dosage" : {
            "numericValue" : 150,
            "unit" : "mg"
          }
        },
        {
          "id" : 3,
          "name" : "Zidovudina",
          "dosage" : {
            "numericValue" : 300,
            "unit" : "mg"
          }
        }
      ]        
    }
  }
```


## Defining texts:

| Code                  | Title                     |  Description                                                 |
| -----------           | -----------               |  -----------                                                 |
| Dosage                | Dosage                    | The value and unit of the dosage, concentration or capacity  |
| numericValue          | Numeric Value             | The numeric value of the dosage                              |
| unit                  | Unit                      | The unit of measurement of the dosage                        |                           
| DosageForm            | Dosage Form               | A dosage form in which the medicine is available            |
| RouteOfAdministration | Route of Administration   | A route by which the medicine can be given                  |
| Container             | Container                 | The container or presentation form of the medicine  |
| id                    | ID                        | An identifier for the container |
| name                  | Container Name            | The name of the container |
| capacity              | Container Capacity        | The capacity of the container |
| ActiveIngredient      | Active Ingredient         | Describe an active ingredient and dosage for the drug, generally chemical compounds and / or biological substances |
| id                    | ID                        | An identifier for the active ingredient |
| name                  | Active Ingredient Name    | The name of the active ingredient. It refers to the generic name as in the International Common Denomination (INN). This field is the same as the 'name' attribute in the ATC / DDD standard. Also known as: generic name, drug, substance name, active substance |
| MedicineAttributes    | Medicine Attributes       | Describe the attributes and properties of chemical or biological substances used as medical therapy, which has a physiological effect on an organism. Here the term drug is used interchangeably with the term medicine, although clinical knowledge makes a clear difference between them |


## Issues

Report issues for this extension 
in the [ocds-extensions repository](https://github.com/open-contracting/ocds-extensions/issues), 
putting the extension's name in the issue's title.
