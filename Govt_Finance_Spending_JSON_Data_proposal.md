## OVERVIEW AND EXPLANATIONS
Overall we wanted to make as much of the UI elements and their consituent parts discoverable
through the API response. Therefore, the response begins by listing off available attributes 
for various UI elements such as the list of available data view, comparison types, years, etc.
The use of objects for this list allows us to have both a display name as well as an ID to use 
'behind the scenes' for what is actively selected in the view (e.g. current_comparison, 
current_adjustments, etc). The examples here are meant to represent a couple fairly fleshed out 
responses in order to show the total data stucture needed to build the Government Finance tables 
and interactivity.

## DATA TABLES
The data_tables array is made up of objects that represent each table to be displayed on the 
page, given the selected combination of available attributes. Listing them out this way allows 
more control over their type.

## DATA TABLE OBJECT
The data table object has basic info about its display name and type and includes a list of column 
objects, row objects, and a list of total row objects. Omitting a name string will make it so there 
is no header above the table (e.g. 'the Grants Paid' table in the designs). 

## COLUMN OBJECT
The columns array of objects represents the horizontal columns of the data and uses the 'data_id' to point 
to the data point key in the data within the rows and total_rows array objects. The 'name' field 
is the visual display name to appear in the table header. I debated whether or not to include the columns 
array within the top level JSON object, so as not to have to repeat it for every table object, but including 
it in the table object will allow for more finer control ultimately should all the table on the page need to 
not share the same columns. 

## ROW OBJECT
The row object is where the magic happens, at least as far as the expandable tables are concerned. Each of the 
objects in the 'rows' array represents a row in the main section of a table, which can also include a 'children' array, 
which represents nested rows. The use of the 'key' field is merely meant to represent a unique value for each row within the given 
table. The use of this 'key' value is actually particular to the React table component we are using, and including it here from the API will make it so we don't 
have to traverse down into all the rows and children on the front end simply to give the object a value for 'key'. Including 
it in the row object will basically make it 'just work' with our third-party component. Note though that although the 'key' is a 
slug of the row names in these examples, it could theoretically be anything unique, but it being named 'key' is what is important.

## TOTAL_ROWS OBJECT
The 'total_rows' array within a table object is simply used to delineate a different style for the final rows of a table, as 
well as give more granualar and seperate control over what the totals are for each table. The object structure is identifical 
to that of the objects in the 'rows' array. Simpy omitting this array will make it so there is no totals row at the bottom of the 
table (e.g. the Grants Paid table, and Employment Summary table on Employment).


## STILL TO BE DETERMINED
While this might be a good place to start, it is obvisouly the beginning of the conversation. There are a number of elements within the 
designs that will need API support which are not covered herein. Specifically, sparklines. The intention of the sparklines in the design 
are meant to represent actual data over time for each data point in the row item. In addition, the sparkline is meant to be a link out to a specific metric on this site 
for that data. How exactly we make available the data for each sparkline in each row is obviously not covered here and will need to be a topic for discussion later. 

Also, although I approached this data structure with the other tables and pages in Government Finances in mind (i.e. Balance Sheets, Revenue, and Employment), it 
remains to be seen whether or not this approach would work for those tables as well. My feeling is they will, but until we start looking at the real data, things may need 
to change in that regard.


Following are examples of API endpoints for some of the various combinations and a mock example of their respective response:

### EXAMPLE for Spending/Expeditures viewed by Mission, Comparing by Government Type, for year 2013

#### GET `/API/spending/by_mission?comparison=government_type&year=2013`

```javascript
{
  "title": "Spending By Mission",
  "available_views": [
    {
      "name": "Spending By Mission",
      "id": "by_mission"
    },
    {
      "name": "Spending By Function",
      "id": "by_function"
    },
    {
      "name": "Alternate",
      "id": "by_alternate"
    }
  ],
  "current_view": "by_mission",
  "available_comparisons": [
    {
      "name": "Government Type",
      "id": "government_type"
    },
    {
      "name": "Years",
      "id": "years"
    }
  ],
  "current_comparison": "government_type",
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
  "current_adjustments": [],
  "available_governement_types": null,
  "current_government_type": null,
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
    }
  ],
  "current_year": "2013",
  "data_tables": [
    {
      "id": "deficit-surplus",
      "name": "Annual Deficit / Surplus ($ billions)",
      "type": "summary",
      "columns": [
        {
          "name": "Federal",
          "data_id": "federal",
        },
        {
          "name": "State & Local",
          "data_id": "state_local"
        },
        {
          "name": "Combined",
          "data_id": "combined"
        }
      ],
      "rows": [
        {
          "key": "total-revenue",
          "name": "Total Revenue",
          "data": {
            "federal": 2925.0,
            "state_local": 2344.1,
            "combined": 5533.1
          }
        },
        {
          "key": "total-spending",
          "name": "Total Spending",
          "data": {
            "federal": 3534.6,
            "state_local": 2938.2,
            "combined": 5999.7
          }
        }
      ],
      "total_rows": [
        {
          "key": "net-balance",
          "name": "Net Balance",
          "data": {
            "federal": 2925.0,
            "state_local": 2344.1,
            "combined": 5533.1
          }
        }
      ]
    },
    {
      "id": "by-mission",
      "name": "Spending By Mission ($ billions)",
      "type": "expandable",
      "columns": [
        {
          "name": "Federal",
          "data_id": "federal"
        },
        {
          "name": "State & Local",
          "data_id": "state_local"
        },
        {
          "name": "Combined",
          "data_id": "combined"
        }
      ],
      "rows": [
        {
          "key": "justice",
          "name": "Establish Justice and Ensure Domestic Tranquility",
          "data": {
            "federal": 325.2,
            "state_local": 325.2,
            "combined": 325.2,
          },
          "children": [
            {
              "key": "justice-physical-safety",
              "name": "Physical Safety",
              "data": {
                "federal": 12.6,
                "state_local": 12.6,
                "combined": 12.6,
              },
            },
            {
              "key": "justice-consumer-protection",
              "name": "Consumer Protection",
              "data": {
                "federal": 12.6,
                "state_local": 12.6,
                "combined": 12.6,
              },
              "children": [
                {
                  "key": "justice-consumer-protection-consumer-protection",
                  "name": "Consumer Protection",
                  "data": {
                    "federal": 1.6,
                    "state_local": 1.6,
                    "combined": 1.6,
                  },
                },
                {
                  "key": "justice-consumer-protection-employee-protection",
                  "name": "Employee Protection",
                  "data": {
                    "federal": 2.6,
                    "state_local": 2.6,
                    "combined": 2.6,
                  },
                },
              ]
            },
            {
              "key": "justice-environment-and-energy",
              "name": "Environment and Energy",
              "data": {
                "federal": 12.6,
                "state_local": 12.6,
                "combined": 12.6,
              },
            },
            {
              "key": "justice-civil-rights",
              "name": "Civil Rights",
              "data": {
                "federal": 12.6,
                "state_local": 12.6,
                "combined": 12.6,
              },
            },
            {
              "key": "justice-child-protection",
              "name": "Child Protection",
              "data": {
                "federal": 12.6,
                "state_local": 12.6,
                "combined": 12.6,
              },
            },
            {
              "key": "justice-immigration",
              "name": "Immigration",
              "data": {
                "federal": 12.6,
                "state_local": 12.6,
                "combined": 12.6,
              },
            }
          ]
        },
        {
          "key": "defense",
          "name": "Provide for the Common Defense",
          "data": {
            "federal": 212.6,
            "state_local": "na",
            "combined": 212.6,
          }
        },
        {
          "key": "welfare",
          "name": "Promote the General Welfare",
          "data": {
            "federal": 325.2,
            "state_local": 325.2,
            "combined": 325.2,
          }
        },
        {
          "key": "liberty",
          "name": "Secure the Blessings of Liberty to Ourselves and Our Posterity",
          "data": {
            "federal": 312.6,
            "state_local": 312.6,
            "combined": 312.6,
          }
        },
        {
          "key": "general",
          "name": "General Government",
          "data": {
            "federal": 12.6,
            "state_local": 12.6,
            "combined": 12.6,
          }
        }
      ],
      "total_rows": [
        {
          "id": "total-spending",
          "name": "Total Spending 2013",
          "data": {
            "federal": 546.2,
            "state_local": 584.4,
            "combined": -38.3,
          }
        }
      ]
    },
  ],
}
```

#### EXAMPLE for Spending/Expeditures viewed by Mission, Comparing by Years, for government type 'Combined'

GET `/API/spending/by_mission?comparison=years&government_type=combined`

```javascript
{
  "title": "Spending By Function",
  "available_views": [
    {
      "name": "Spending By Mission",
      "id": "by_mission"
    },
    {
      "name": "Spending By Function",
      "id": "by_function"
    },
    {
      "name": "Alternate",
      "id": "by_alternate"
    }
  ],
  "current_view": "by_mission",
  "available_comparisons": [
    {
      "name": "Government Type",
      "id": "government_type"
    },
    {
      "name": "Years",
      "id": "years"
    }
  ],
  "current_comparison": "year",
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
  "current_adjustments": [],
  "available_governement_types": [
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
      "id": "comnbined"
    }
  ],
  "current_government_type": "combined",
  "available_years": null,
  "current_year": null,
  "data_tables": [
    {
      "id": "deficit-surplus", // just some unique ID
      "name": "Annual Deficit / Surplus ($ billions)",
      "type": "summary",
      "columns": [
        {
          "name": "1980",
          "data_id": "1980"
        },
        {
          "name": "1990",
          "data_id": "1990"
        },
        {
          "name": "2000",
          "data_id": "2000"
        },
        {
          "name": "2010",
          "data_id": "2010"
        },
        {
          "name": "2011",
          "data_id": "2011"
        },
        {
          "name": "2012",
          "data_id": "2012"
        },
        {
          "name": "2013",
          "data_id": "2013"
        },
      ],
      "rows": [
        {
          "key": "total-revenue",
          "name": "Total Revenue",
          "data": {
            "1980": 2333.1,
            "1990": 5533.1,
            "2000": 5533.1,
            "2010": 5533.1,
            "2011": 5533.1,
            "2012": 5533.1,
            "2013": 5533.1
          }
        },
        {
          "key": "total-spending",
          "name": "Total Spending",
          "data": {
            "1980": 5999.7,
            "1990": 5999.7,
            "2000": 5999.7,
            "2010": 5999.7,
            "2011": 5999.7,
            "2012": 5999.7,
            "2013": 5999.7
          }
        }
      ],
      "total_rows": [
        {
          "key": "net-balance",
          "name": "Net Balance",
          "data": {
            "1980": -466.7,
            "1990": -466.7,
            "2000": -466.7,
            "2010": -466.7,
            "2011": -466.7,
            "2012": -466.7,
            "2013": -466.7
          }
        }
      ]
    },
    {
      "id": "by-mission",
      "name": "Spending By Mission ($ billions)",
      "type": "expandable",
      "columns": [
        {
          "name": "1980",
          "data_id": "1980"
        },
        {
          "name": "1990",
          "data_id": "1990"
        },
        {
          "name": "2000",
          "data_id": "2000"
        },
        {
          "name": "2010",
          "data_id": "2010"
        },
        {
          "name": "2011",
          "data_id": "2011"
        },
        {
          "name": "2012",
          "data_id": "2012"
        },
        {
          "name": "2013",
          "data_id": "2013"
        },
      ],
      "rows": [
        {
          "key": "justice",
          "name": "Establish Justice and Ensure Domestic Tranquility",
          "data": {
            "1980": 325.2,
            "1990": 325.2,
            "2000": 325.2,
            "2010": 325.2,
            "2011": 325.2,
            "2012": 325.2,
            "2013": 325.2
          },
          "children": [
            {
              "key": "justice-physical-safety",
              "name": "Physical Safety",
              "data": {
                "1980": 12.6,
                "1990": 12.6,
                "2000": 12.6,
                "2010": 12.6,
                "2011": 12.6,
                "2012": 12.6,
                "2013": 12.6
              },
            },
            {
              "key": "justice-consumer-protection",
              "name": "Consumer Protection",
              "data": {
                "1980": 12.6,
                "1990": 12.6,
                "2000": 12.6,
                "2010": 12.6,
                "2011": 12.6,
                "2012": 12.6,
                "2013": 12.6
              },
              "children": [
                {
                  "key": "justice-consumer-protection-consumer-protection",
                  "name": "Consumer Protection",
                  "data": {
                    "1980": 1.6,
                    "1990": 1.6,
                    "2000": 1.6,
                    "2010": 1.6,
                    "2011": 1.6,
                    "2012": 1.6,
                    "2013": 1.6
                  },
                },
                {
                  "key": "justice-consumer-protection-employee-protection",
                  "name": "Employee Protection",
                  "data": {
                    "1980": 2.6,
                    "1990": 2.6,
                    "2000": 2.6,
                    "2010": 2.6,
                    "2011": 2.6,
                    "2012": 2.6,
                    "2013": 2.6
                  },
                },
              ]
            },
            {
              "key": "justice-environment-and-energy",
              "name": "Environment and Energy",
              "data": {
                "1980": 12.6,
                "1990": 12.6,
                "2000": 12.6,
                "2010": 12.6,
                "2011": 12.6,
                "2012": 12.6,
                "2013": 12.6
              },
            },
            {
              "key": "justice-civil-rights",
              "name": "Civil Rights",
              "data": {
                "1980": 12.6,
                "1990": 12.6,
                "2000": 12.6,
                "2010": 12.6,
                "2011": 12.6,
                "2012": 12.6,
                "2013": 12.6
              },
            },
            {
              "key": "justice-child-protection",
              "name": "Child Protection",
              "data": {
                "1980": 12.6,
                "1990": 12.6,
                "2000": 12.6,
                "2010": 12.6,
                "2011": 12.6,
                "2012": 12.6,
                "2013": 12.6
              },
            },
            {
              "key": "justice-immigration",
              "name": "Immigration",
              "data": {
                "1980": 12.6,
                "1990": 12.6,
                "2000": 12.6,
                "2010": 12.6,
                "2011": 12.6,
                "2012": 12.6,
                "2013": 12.6
              },
            }
          ]
        },
        {
          "key": "defense",
          "name": "Provide for the Common Defense",
          "data": {
            "1980": 312.6,
            "1990": 312.6,
            "2000": 312.6,
            "2010": 312.6,
            "2011": 312.6,
            "2012": 312.6,
            "2013": 312.6,
          }
        },
        {
          "key": "welfare",
          "name": "Promote the General Welfare",
          "data": {
            "1980": 325.2,
            "1990": 325.2,
            "2000": 325.2,
            "2010": 325.2,
            "2011": 325.2,
            "2012": 325.2,
            "2013": 325.2
          }
        },
        {
          "key": "liberty",
          "name": "Secure the Blessings of Liberty to Ourselves and Our Posterity",
          "data": {
            "1980": 312.6,
            "1990": 312.6,
            "2000": 312.6,
            "2010": 312.6,
            "2011": 312.6,
            "2012": 312.6,
            "2013": 312.6,
          }
        },
        {
          "key": "general",
          "name": "General Government",
          "data": {
            "1980": 12.6,
            "1990": 12.6,
            "2000": 12.6,
            "2010": 12.6,
            "2011": 12.6,
            "2012": 12.6,
            "2013": 12.6
          }
        }
      ],
      "total_rows": [
        {
          "key": "total-spending",
          "name": "Total Combined Spending",
          "data": {
            "1980": -38.3,
            "1990": -38.3,
            "2000": -38.3,
            "2010": -38.3,
            "2011": -38.3,
            "2012": -38.3,
            "2013": -38.3,
          }
        }
      ]
    },
  ],
}
```

#### EXAMPLE for Spending/Expeditures viewed by Function, Comparing by Government Type, for year 2013, with adjustments for inflation and per capita engaged

GET `/API/spending/by_function?comparison=government_type&year=2013?adjustments=per-capita+inflation`

```javascript
{
  "title": "Spending By Mission",
  "available_views": [
    {
      "name": "Spending By Mission",
      "id": "by_mission"
    },
    {
      "name": "Spending By Function",
      "id": "by_function"
    },
    {
      "name": "Alternate",
      "id": "by_alternate"
    }
  ],
  "current_view": "by_function",
  "available_comparisons": [
    {
      "name": "Government Type",
      "id": "government_type"
    },
    {
      "name": "Years",
      "id": "years"
    }
  ],
  "current_comparison": "government_type",
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
  "current_adjustments": ["per-capita", "inflation"],
  "available_governement_types": null,
  "current_government_type": null,
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
    }
  ],
  "current_year": "2013",
  "data_tables": [
    {
      "id": "deficit-surplus",
      "name": "Annual Deficit / Surplus ($ billions)",
      "type": "summary",
      "columns": [
        ...
      ],
      "rows": [
        ...
      ],
      "total_rows": [
        ...
      ]
    },
    {
      "id": "by-function",
      "name": "Spending By Function ($ billions)",
      "type": "expandable",
      "columns": [
        ...
      ],
      "rows": [
        ...
      ],
      "total_rows": [
        ...
      ]
    },
  ],
}
```
