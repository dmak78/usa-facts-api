# Government Finance Tables API Proposal

```javascript
{
    "id": 1,
    "name": "Government Spending By Mission",
    "lexicon_name": "Spending By Mission",
    "url": "financeTables/1",
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
              "text": "<p>This is example tooltip text for General Government</p>"
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

