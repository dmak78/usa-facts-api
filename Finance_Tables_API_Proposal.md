# Government Finance Tables API Proposal V2
See [Full Example](#full-example---spending-by-mission) below

## Overview
This version of the Finance Tables API Proposal is very similar to the previous version with the main exception being that the data returned for each row include all government types (e.g. Federal, State & Local, and Combined) as well as all the yearly data for each of those government types. 

*Example of a single row without any children:*
```javascript
{
    "id": 1000,
    "key": "total-revenue",
    "name": "Annual Deficit / Surplus - Total Revenue",
    "lexicon_name": "Total Revenue",
    "url": "metrics/1000",
    "data": {
      "federal": {
        "1980": 2999.1,
        "1990": 2999.1,
        "2000": 2999.1,
        "2010": 2999.1,
        "2011": 2999.1,
        "2012": 2999.1,
        "2013": 2999.1,
        "2014": 2999.1,
        "2015": 2999.1,
      },
      "state_local": {
        "1980": 2333.1,
        "1990": 2333.1,
        "2000": 2333.1,
        "2010": 2333.1,
        "2011": 2333.1,
        "2012": 2333.1,
        "2013": 2333.1,
        "2014": "n/a",
        "2015": "n/a"
      },
      "combined": {
        "1980": 2333.1,
        "1990": 5533.1,
        "2000": 5533.1,
        "2010": 5533.1,
        "2011": 5533.1,
        "2012": 5533.1,
        "2013": 5533.1,
        "2014": "n/a",
        "2015": "n/a"
      }
    },
    "children": null
    }
```

This make it so the table component on the front-end has all the data it needs to render each view of the data given the UI selections. Also, it cuts down on needing to hit the API for every discrete change in the UI. This will also provide the ncessary data to render the sparklines on each row.


## Endpoints and Finance Page Configuration
Each table data set will have its own endpoint, for example, "Spending by Mission" could be `API/financeTables/1`. Therefore, one could also theorteically get a list of ALL of the available finance tables that could appear on any of the finance pages by going to `API/financeTables/`. The way each page knows which available datasets it has (e.g. Spending has 'Spending By Mission', 'Spending By Function', 'Alternate P&L') is via a page configration object for each Finance Page that maps the available datasets to each page and provides the title and URL for each of the datasets for that Finance Page. This object can either be hardcoded or should probably also come from the API, which I believe may help with search indexing.

**Example of Spending Page configuration object for the Spending Page, possibly aquired at `API/finances/spending`:**
```javascript
{
  "name": "Government Spending",
  "lexicon_name": "Spending",
  "current_table": 1,
  "available_tables": [
    {
      "id": 1,
      "name": "Spending By Mission",
      "lexicon_name": "Spending By Mission",
      "url": "financeTables/1"
    },
    {
      "id": 2,
      "name": "Spending By Function",
      "lexicon_name": "Spending By Function",
      "url": "financeTables/2"
    },
    {
      "id": 3,
      "name": "Spending By Alternate P&L",
      "lexicon_name": "Alternate P&L",
      "url": "financeTables/2"
    }
  ]
}
```
With this, the front-end can configure the selections in the **'Choose Data'** dropdown and procure the necessary data table set via the `url` when it is selected in the dropdown. Note in the above example, the default table is set to **Spending By Mission** for the Spending Page via `current_table`.
 
## Options
The data necessary to fill out the UI for **'Compare By'**, **'Select Year'**, and **'Select Government Type'** has been moved into an object titled `options`. These options will also now be used to determine which columns appear above the table and indicate which data point to grab for each column and row. For example, when `by_government_type` is selected as the `current_comparison`, `available_government_types` will be used as the columns, and `current_year` will be used to select which year's data will be displayed.

## Data Tables
Just like the previous proposal, each larger table object procured at the `API/financeTables/{id}` endpoint, will have an array of individual 'table blocks' found in the `data_tables` array. Each of the `data_tables` correspond to the various types of tables one would see on a given Finance Page. For example the below example of a full data table corresponds to this view: (https://artefactgroup.invisionapp.com/share/AQ9BWL1T4#/screens/210425493)

Again, each data_table will have a type that will render the table block accordingly. Right now we have 3 types: `summary`, `expandable`, and `other`. See the link to Invision above for reference: `summary` is the top grey colored table, `expandable` is the blue colored expandable table, and `other` is a plain table with no header (see "Grants Paid by Federal Government..." in the above Invision link).

**NOTE** that, although not represented in this example, it might be helpful for search indexing to be able to grab the `data_tables` of  a table set individually via a `url` as well, for example the "Annual Deficit / Surplus Total Spending" table block below *could* be directly fetched at `API/financeTables/1/tables/1`. 


## Rows

### Rows ARE Metrics
Each row will need to have a `url` that points to its corresponding metric page.

### Row Object and Children Row Objects
Just like the previous proposal, each object within `rows` and `total_rows` are the same type of object. One indicates the 'body' (`rows`) of the table block, and the other the 'footer' (`total_rows`).

Each row object can have an array of `children`, which are themselves row objects like their parents and can be nested infinitely. This is used to indicate how rows can expand to show rows underneath them. In this example if a row does not have children then it is set to null, but note that this is not necessary and could just omit the `children` array entirely if there are no children rows.

### Meta info
There may be more added to this API in this regard but for now if a row has a tooltip (see 'General Government' row in Invision design), then a `meta` object will be included in a row object.

```javascript
...
"children": null,
"meta": {
  "text": "This is example tooltip text for General Government"
}
...
```

## Other Notes About the Data
### Data Numbers and NA Strings
Although the numbers in the JSON example below are all floats or ints, they could, and probably just should, be strings being that where data is unavaiable or not applicable, we would want to render 'n/a' or a empty string depending on the context.

### Government-Run Business Page
see (https://artefactgroup.invisionapp.com/share/AQ9BWL1T4#/screens/212819839)
The Government-Run Business Page and Table will be a slight exception from the norm in that it will have only ONE available table and therefore will not render the **Choose Data** dropdown. Also, the `available_government_types` will be Federal, State & Local, and All. Here also, the rows themselves are applicable to either Federal or State & Local and will perhaps need to be hidden and shown depending on the selection in the Government Type dropdown. I am currently working on the best way to achieve this without needed to alter the structure I have proposed here.

### Adjustments
Similar to the previous proposal, adjustments such as Per-Capita and Inflation, will be handled with query strings such as `?adjustments=per-capita`, `?adjustments=inflation`, and `?adjustments=per-capita+inflation`.


## Full Example - Spending By Mission 

**Example of full data fetched at `API/financeTables/1`:**

```javascript
{
  "id": 1,
  "name": "Government Spending By Mission",
  "lexicon_name": "Spending By Mission",
  "url": "financeTables/1",
  "current_comparison": "by_government_type",
  "current_year": "2013",
  "current_government_type": null,
  "current_adjustments": [],
  "options": {
    "available_comparisons": [
      {
        "name": "Government Type",
        "id": "by_government_type"
      },
      {
        "name": "Years",
        "id": "by_years"
      }
    ],
    "available_adjustments": [
      {
        "name": "Per Capita",
        "id": "per-capita"
      },
      {
        "name": "Adjust for Inflation",
        "id": "inflation"
      }
    ],
    "available_government_types": [
      {
        "name": "Federal",
        "id": "federal"
      },
      {
        "name": "State & Local",
        "id": "state_local"
      },
      {
        "name": "Combined",
        "id": "combined"
      }
    ],
    "available_years": [
      {
        "name": "1980",
        "id": "1980"
      },
      {
        "name": "1990",
        "id": "1990"
      },
      {
        "name": "2000",
        "id": "2000"
      },
      {
        "name": "2010",
        "id": "2010"
      },
      {
        "name": "2011",
        "id": "2011"
      },
      {
        "name": "2012",
        "id": "2012"
      },
      {
        "name": "2013",
        "id": "2013"
      },
      {
        "name": "2014",
        "id": "2014"
      },
      {
        "name": "2015",
        "id": "2015"
      }
    ],
  }
  "data_tables": [
    {
      "id": "deficit-surplus",
      "name": "Annual Deficit / Surplus",
      "lexicon_name": "Annual Deficit / Surplus ($ billions)",
      "type": "summary",
      "rows": [
        {
          "id": 1000,
          "key": "total-revenue",
          "name": "Annual Deficit / Surplus - Total Revenue",
          "lexicon_name": "Total Revenue",
          "url": "metrics/1000",
          "data": {
            "federal": {
              "1980": 2999.1,
              "1990": 2999.1,
              "2000": 2999.1,
              "2010": 2999.1,
              "2011": 2999.1,
              "2012": 2999.1,
              "2013": 2999.1,
              "2014": 2999.1,
              "2015": 2999.1,
            },
            "state_local": {
              "1980": 2333.1,
              "1990": 2333.1,
              "2000": 2333.1,
              "2010": 2333.1,
              "2011": 2333.1,
              "2012": 2333.1,
              "2013": 2333.1,
              "2014": "n/a",
              "2015": "n/a"
            },
            "combined": {
              "1980": 2333.1,
              "1990": 5533.1,
              "2000": 5533.1,
              "2010": 5533.1,
              "2011": 5533.1,
              "2012": 5533.1,
              "2013": 5533.1,
              "2014": "n/a",
              "2015": "n/a"
            }
          },
          "children": null
        },
        {
          "id": 1001,
          "key": "total-spending",
          "name": "Annual Deficit / Surplus Total Spending",
          "lexicon_name": "Total Spending",
          "url": "metrics/1001",
          "data": {
            "federal": {
              "1980": 3534.6,
              "1990": 3534.6,
              "2000": 3534.6,
              "2010": 3534.6,
              "2011": 3534.6,
              "2012": 3534.6,
              "2013": 3534.6,
              "2014": 3534.6,
              "2015": 3534.6,
            },
            "state_local": {
              "1980": 2938.6,
              "1990": 2938.6,
              "2000": 2938.6,
              "2010": 2938.6,
              "2011": 2938.6,
              "2012": 2938.6,
              "2013": 2938.6,
              "2014": 'n/a',
              "2015": 'n/a',
            },
            "combined": {
              "1980": 5999.6,
              "1990": 5999.6,
              "2000": 5999.6,
              "2010": 5999.6,
              "2011": 5999.6,
              "2012": 5999.6,
              "2013": 5999.6,
              "2014": 'n/a',
              "2015": 'n/a',
            }
          }
        }
      ],
      "total_rows": [
        {
          "id": 1002,
          "url": "metrics/1002",
          "key": "net-balance",
          "name": "Annual Deficit / Surplus Net Balance",
          "lexicon_name": "Net Balance",
          "data": {
            "federal": {
              "1980": 3534.6,
              "1990": 3534.6,
              "2000": 3534.6,
              "2010": 3534.6,
              "2011": 3534.6,
              "2012": 3534.6,
              "2013": 3534.6,
              "2014": 3534.6,
              "2015": 3534.6,
            },
            "state_local": {
              "1980": 2938.6,
              "1990": 2938.6,
              "2000": 2938.6,
              "2010": 2938.6,
              "2011": 2938.6,
              "2012": 2938.6,
              "2013": 2938.6,
              "2014": 'n/a',
              "2015": 'n/a',
            },
            "combined": {
              "1980": 5999.6,
              "1990": 5999.6,
              "2000": 5999.6,
              "2010": 5999.6,
              "2011": 5999.6,
              "2012": 5999.6,
              "2013": 5999.6,
              "2014": 'n/a',
              "2015": 'n/a',
            }
          }
        }
      ]
    },
    {
      "id": "by-mission-1",
      "name": "Spending By Mission",
      "lexicon_name": "Spending By Mission ($ billions)",
      "type": "expandable",
      "rows": [
        {
          "id": 2000,
          "key": "justice",
          "name": "Establish Justice and Ensure Domestic Tranquility",
          "lexicon_name": "Establish Justice and Ensure Domestic Tranquility",
          "url": "metrics/2000",
          "data": {
            "federal": {
              "1980": 234.1,
              "1990": 6345.1,
              "2000": 345.1,
              "2010": 345.1,
              "2011": 234.1,
              "2012": 765.1,
              "2013": 675.1,
              "2014": 3453.1,
              "2015": 2342.1,
            },
            "state_local": {
              "1980": 456.1,
              "1990": 867.1,
              "2000": 234.1,
              "2010": 456.1,
              "2011": 675.1,
              "2012": 345.1,
              "2013": 567.1,
              "2014": 4623.1,
              "2015": 4684.1,
            },
            "combined": {
              "1980": 246.1,
              "1990": 2467.1,
              "2000": 574.1,
              "2010": 289.1,
              "2011": 3352.1,
              "2012": 569.1,
              "2013": 563.1,
              "2014": 457.1,
              "2015": 457.1,
            },
          },
          "children": [
            {
              "id": 2001,  
              "key": "justice-physical-safety",
              "name": "Physical Safety",
              "lexicon_name": "Physical Safety",
              "url": "metrics/2001",
              "data": {
                "federal": {
                  "1980": 12.1,
                  "1990": 12.1,
                  "2000": 12.1,
                  "2010": 12.1,
                  "2011": 12.1,
                  "2012": 12.1,
                  "2013": 12.1,
                  "2014": 12.1,
                  "2015": 12.1,
                },
                "state_local": {
                  "1980": 12.1,
                  "1990": 12.1,
                  "2000": 12.1,
                  "2010": 12.1,
                  "2011": 12.1,
                  "2012": 12.1,
                  "2013": 12.1,
                  "2014": 12.1,
                  "2015": 12.1,
                },
                "combined": {
                  "1980": 12.1,
                  "1990": 12.1,
                  "2000": 12.1,
                  "2010": 12.1,
                  "2011": 12.1,
                  "2012": 12.1,
                  "2013": 12.1,
                  "2014": 12.1,
                  "2015": 12.1,
                },
              },
              "children": null
            },
            {
              "id": 2002,
              "key": "justice-consumer-protection",
              "name": "Consumer Protection",
              "lexicon_name": "Consumer Protection",
              "url": "metrics/2002",
              "data": {
                "federal": {
                  "1980": 46,
                  "1990": 32,
                  "2000": 234,
                  "2010": 43,
                  "2011": 23,
                  "2012": 36,
                  "2013": 234,
                  "2014": 57,
                  "2015": 86,
                },
                "state_local": {
                  "1980": 34,
                  "1990": 16,
                  "2000": 855,
                  "2010": 345,
                  "2011": 76,
                  "2012": 234,
                  "2013": 236,
                  "2014": 65,
                  "2015": 345,
                },
                "combined": {
                  "1980": 56,
                  "1990": 456,
                  "2000": 234,
                  "2010": 234,
                  "2011": 7654,
                  "2012": 345,
                  "2013": 234,
                  "2014": 346,
                  "2015": 345,
                },
              },
              "children": [
                {
                  "id": 3000,
                  "key": "justice-consumer-protection-consumer-protection",
                  "name": "Consumer Protection",
                  "lexicon_name": "Consumer Protection",
                  "url": "metrics/3000",
                  "data": {
                    "federal": {
                      "1980": 56,
                      "1990": 456,
                      "2000": 234,
                      "2010": 234,
                      "2011": 7654,
                      "2012": 345,
                      "2013": 234,
                      "2014": 346,
                      "2015": 345,
                    },
                    "state_local": {
                      "1980": 56,
                      "1990": 456,
                      "2000": 234,
                      "2010": 234,
                      "2011": 7654,
                      "2012": 345,
                      "2013": 234,
                      "2014": 346,
                      "2015": 345,
                    },
                    "combined": {
                      "1980": 56,
                      "1990": 456,
                      "2000": 234,
                      "2010": 234,
                      "2011": 7654,
                      "2012": 345,
                      "2013": 234,
                      "2014": 346,
                      "2015": 345,
                    },
                  },
                  "children": null
                },
                {
                  "id": 3001,
                  "key": "justice-consumer-protection-employee-protection",
                  "name": "Employee Protection",
                  "lexicon_name": "Employee Protection",
                  "url": "metrics/3001",
                  "data": {
                    "federal": {
                      "1980": 56,
                      "1990": 456,
                      "2000": 234,
                      "2010": 234,
                      "2011": 7654,
                      "2012": 345,
                      "2013": 234,
                      "2014": 346,
                      "2015": 345,
                    },
                    "state_local": {
                      "1980": 56,
                      "1990": 456,
                      "2000": 234,
                      "2010": 234,
                      "2011": 7654,
                      "2012": 345,
                      "2013": 234,
                      "2014": 346,
                      "2015": 345,
                    },
                    "combined": {
                      "1980": 56,
                      "1990": 456,
                      "2000": 234,
                      "2010": 234,
                      "2011": 7654,
                      "2012": 345,
                      "2013": 234,
                      "2014": 346,
                      "2015": 345,
                    },
                  },
                  "children": null
                },
              ]
            },
            {
              "id": 2003,
              "key": "justice-environment-and-energy",
              "name": "Environment and Energy",
              "lexicon_name": "Environment and Energy",
              "url": "metrics/2003",
              "data": {
                "federal": {
                  "1980": 56,
                  "1990": 456,
                  "2000": 234,
                  "2010": 234,
                  "2011": 7654,
                  "2012": 345,
                  "2013": 234,
                  "2014": 346,
                  "2015": 345,
                },
                "state_local": {
                  "1980": 56,
                  "1990": 456,
                  "2000": 234,
                  "2010": 234,
                  "2011": 7654,
                  "2012": 345,
                  "2013": 234,
                  "2014": 346,
                  "2015": 345,
                },
                "combined": {
                  "1980": 56,
                  "1990": 456,
                  "2000": 234,
                  "2010": 234,
                  "2011": 7654,
                  "2012": 345,
                  "2013": 234,
                  "2014": 346,
                  "2015": 345,
                },
              },
              "children": null
            },
            {
              "id": 2004,
              "key": "justice-civil-rights",
              "name": "Civil Rights",
              "lexicon_name": "Civil Rights",
              "url": "metrics/2004",
              "data": {
                "federal": {
                  "1980": 56,
                  "1990": 456,
                  "2000": 234,
                  "2010": 234,
                  "2011": 7654,
                  "2012": 345,
                  "2013": 234,
                  "2014": 346,
                  "2015": 345,
                },
                "state_local": {
                  "1980": 56,
                  "1990": 456,
                  "2000": 234,
                  "2010": 234,
                  "2011": 7654,
                  "2012": 345,
                  "2013": 234,
                  "2014": 346,
                  "2015": 345,
                },
                "combined": {
                  "1980": 56,
                  "1990": 456,
                  "2000": 234,
                  "2010": 234,
                  "2011": 7654,
                  "2012": 345,
                  "2013": 234,
                  "2014": 346,
                  "2015": 345,
                },
              },
              "children": null
            },
            {
              "id": 2005,
              "key": "justice-child-protection",
              "name": "Child Protection",
              "lexicon_name": "Child Protection",
              "url": "metrics/2005",
              "data": {
                "federal": {
                  "1980": 56,
                  "1990": 456,
                  "2000": 234,
                  "2010": 234,
                  "2011": 7654,
                  "2012": 345,
                  "2013": 234,
                  "2014": 346,
                  "2015": 345,
                },
                "state_local": {
                  "1980": 56,
                  "1990": 456,
                  "2000": 234,
                  "2010": 234,
                  "2011": 7654,
                  "2012": 345,
                  "2013": 234,
                  "2014": 346,
                  "2015": 345,
                },
                "combined": {
                  "1980": 56,
                  "1990": 456,
                  "2000": 234,
                  "2010": 234,
                  "2011": 7654,
                  "2012": 345,
                  "2013": 234,
                  "2014": 346,
                  "2015": 345,
                },
              },
              "children": null
            },
            {
              "id": 2006,
              "key": "justice-immigration",
              "name": "Immigration",
              "lexicon_name": "Immigration",
              "url": "mwetrics/2006",
              "data": {
                "federal": {
                  "1980": 56,
                  "1990": 456,
                  "2000": 234,
                  "2010": 234,
                  "2011": 7654,
                  "2012": 345,
                  "2013": 234,
                  "2014": 346,
                  "2015": 345,
                },
                "state_local": {
                  "1980": 56,
                  "1990": 456,
                  "2000": 234,
                  "2010": 234,
                  "2011": 7654,
                  "2012": 345,
                  "2013": 234,
                  "2014": 346,
                  "2015": 345,
                },
                "combined": {
                  "1980": 56,
                  "1990": 456,
                  "2000": 234,
                  "2010": 234,
                  "2011": 7654,
                  "2012": 345,
                  "2013": 234,
                  "2014": 346,
                  "2015": 345,
                },
              },
              "children": null
            }
          ]
        },
        {
          "id": 2007,
          "key": "defense",
          "name": "Provide for the Common Defense",
          "lexicon_name": "Provide for the Common Defense",
          "url": "metrics/2007",
          "data": {
            "federal": {
              "1980": 56,
              "1990": 456,
              "2000": 234,
              "2010": 234,
              "2011": 7654,
              "2012": 345,
              "2013": 234,
              "2014": 346,
              "2015": 345,
            },
            "state_local": {
              "1980": 'n/a',
              "1990": 'n/a',
              "2000": 'n/a',
              "2010": 'n/a',
              "2011": 'n/a',
              "2012": 'n/a',
              "2013": 'n/a',
              "2014": 'n/a',
              "2015": 'n/a',
            },
            "combined": {
              "1980": 'n/a',
              "1990": 'n/a',
              "2000": 'n/a',
              "2010": 'n/a',
              "2011": 'n/a',
              "2012": 'n/a',
              "2013": 'n/a',
              "2014": 'n/a',
              "2015": 'n/a',
            },
          },
          "children": null
        },
        {
          "id": 2009,
          "key": "welfare",
          "name": "Promote the General Welfare",
          "lexicon_name": "Promote the General Welfare",
          "url": "metrics/2009",
          "data": {
            "federal": {
              "1980": 56,
              "1990": 456,
              "2000": 234,
              "2010": 234,
              "2011": 7654,
              "2012": 345,
              "2013": 234,
              "2014": 346,
              "2015": 345,
            },
            "state_local": {
              "1980": 56,
              "1990": 456,
              "2000": 234,
              "2010": 234,
              "2011": 7654,
              "2012": 345,
              "2013": 234,
              "2014": 346,
              "2015": 345,
            },
            "combined": {
              "1980": 56,
              "1990": 456,
              "2000": 234,
              "2010": 234,
              "2011": 7654,
              "2012": 345,
              "2013": 234,
              "2014": 346,
              "2015": 345,
            },
          },
          "children": null
        },
        {
          "id": 2010,
          "key": "liberty",
          "name": "Secure the Blessings of Liberty to Ourselves and Our Posterity",
          "lexicon_name": "Secure the Blessings of Liberty to Ourselves and Our Posterity",
          "url": "metrics/2010",
          "data": {
            "federal": {
              "1980": 56,
              "1990": 456,
              "2000": 234,
              "2010": 234,
              "2011": 7654,
              "2012": 345,
              "2013": 234,
              "2014": 346,
              "2015": 345,
            },
            "state_local": {
              "1980": 56,
              "1990": 456,
              "2000": 234,
              "2010": 234,
              "2011": 7654,
              "2012": 345,
              "2013": 234,
              "2014": 346,
              "2015": 345,
            },
            "combined": {
              "1980": 56,
              "1990": 456,
              "2000": 234,
              "2010": 234,
              "2011": 7654,
              "2012": 345,
              "2013": 234,
              "2014": 346,
              "2015": 345,
            },
          },
          "children": null
        },
        {
          "id": 2011,
          "key": "general",
          "name": "General Government",
          "lexicon_name": "General Government",
          "url": "metrics/2011",
          "data": {
            "federal": {
              "1980": 56,
              "1990": 456,
              "2000": 234,
              "2010": 234,
              "2011": 7654,
              "2012": 345,
              "2013": 234,
              "2014": 346,
              "2015": 345,
            },
            "state_local": {
              "1980": 56,
              "1990": 456,
              "2000": 234,
              "2010": 234,
              "2011": 7654,
              "2012": 345,
              "2013": 234,
              "2014": 346,
              "2015": 345,
            },
            "combined": {
              "1980": 56,
              "1990": 456,
              "2000": 234,
              "2010": 234,
              "2011": 7654,
              "2012": 345,
              "2013": 234,
              "2014": 346,
              "2015": 345,
            },
          },
          "children": null,
          "meta": {
            "text": "This is example tooltip text for General Government"
          }
        }
      ],
      "total_rows": [
        {
          "id": 2012,
          "key": "total-spending",
          "name": "Total Spending by Mission for 2013",
          "lexicon_name": "Total Spending 2013",
          "url": "metrics/2012",
          "data": {
            "federal": {
              "1980": 546.2,
              "1990": 234.2,
              "2000": 546.2,
              "2010": 876.2,
              "2011": 987.2,
              "2012": 1232.2,
              "2013": 1651.2,
              "2014": 2642.2,
              "2015": 3466.2,
            },
            "state_local": {
              "1980": 124.4,
              "1990": 643.4,
              "2000": 2345.4,
              "2010": 234.4,
              "2011": 765.4,
              "2012": 543.4,
              "2013": 9456.4,
              "2014": 245.4,
              "2015": 247.4,
            },
            "combined": {
              "1980": -138.3,
              "1990": -328.3,
              "2000": -348.3,
              "2010": -388.3,
              "2011": -348.3,
              "2012": -378.3,
              "2013": -738.3,
              "2014": -438.3,
              "2015": -638.3,
            },
          },
          "children": null
        }
      ]
    },
    {
      "id": "grants-paid-table",
      "name": "Grants Paid",
      "lexicon_name": "",
      "type": "other",
      "rows": [
        {
          "id": 2013,
          "key": "grants-paid-by-federal-government",
          "name": "Grants Paid by Federal Government, Received by S&L, and Discrepency",
          "lexicon_name": "Grants Paid by Federal Government, Received by S&L, and Discrepency",
          "url": "metrics/2013",
          "data": {
            "federal": {
              "1980": 235.2,
              "1990": 235.2,
              "2000": 235.2,
              "2010": 234.2,
              "2011": 643.2,
              "2012": 234.2,
              "2013": 8567.2,
              "2014": 546.2,
              "2015": 345.2,
            },
            "state_local": {
              "1980": 234,
              "1990": 423,
              "2000": 234,
              "2010": 432,
              "2011": 34,
              "2012": 243,
              "2013": 423,
              "2014": 423,
              "2015": 234,
            },
            "combined": {
              "1980": -3.3,
              "1990": -338.3,
              "2000": -464.3,
              "2010": -34.3,
              "2011": -234.3,
              "2012": -75.3,
              "2013": -345.3,
              "2014": -676.3,
              "2015": -345.3,
            },
          },
          "children": null
        }
      ],
      "total_rows": null
    }
  ]
}
```
