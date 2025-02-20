# Documentation what I learned about engaging with the BLS API for Occupational Employment Statistics (OES)

## For any other person wanting to engange with the The BLS Public Data API but has come across this: “The BLS Public Data API requires users to know the series ID to request data. We do not currently have a catalogue of series IDs, but all BLS series IDs follow a similar format.” This is what I have scrapped together



## Table of Contents
1. Basic Request Structure
2. Series ID Format
3. Geographic Levels
4. State Codes
5. Occupation Codes
6. Data Types
7. Example Requests
8. Response Format

## 1. Basic Request Structure
```json
{
  "seriesid": ["OEXN000000000000000000"],
  "startyear": "2022",
  "endyear": "2023",
  "registrationkey": "YOUR_KEY"
}
```

## 2. Series ID Format
Example: `OESN06000000000291141`

### Position Breakdown:
1. **Program Code** (Positions 1-2)
   - `OE` = Occupational Employment Statistics
   - Source: https://www.bls.gov/developers/home.htm

2. **Geographic Level** (Position 3)
   - `U` = United States (National)
   - `S` = State
   - `M` = Metropolitan area
   - `N` = Non-metropolitan area

3. **Seasonal Adjustment** (Position 4)
   - `N` = Not seasonally adjusted
   - `S` = Seasonally adjusted (if available)

4. **Location Code** (Positions 5-6, if applicable)
   - Two-digit state FIPS code
   - Used when Geographic Level is 'S'

5. **Industry Code** (Positions vary)
   - Nine digits
   - Use `000000000` for all industries

6. **Occupation Code** (Last 6 digits before data type)
   - Six-digit SOC code
   - Example: `291141` for Registered Nurses

7. **Data Type** (Last 2 digits)
   - `01` = Employment count
   - `03` = Hourly mean wage
   - `13` = Annual mean wage

## 3. Geographic Levels
- **National (U)**: Entire United States
- **State (S)**: Individual state data
- **Metropolitan (M)**: Specific metropolitan areas
- **Non-metropolitan (N)**: Rural areas

## 4. State Codes (FIPS)
Source: https://www.bls.gov/respondents/mwr/electronic-data-interchange/appendix-d-usps-state-abbreviations-and-fips-codes.htm

```
01 = Alabama (AL)        02 = Alaska (AK)         04 = Arizona (AZ)
05 = Arkansas (AR)       06 = California (CA)     08 = Colorado (CO)
09 = Connecticut (CT)    10 = Delaware (DE)       11 = DC
12 = Florida (FL)        13 = Georgia (GA)        15 = Hawaii (HI)
16 = Idaho (ID)          17 = Illinois (IL)       18 = Indiana (IN)
19 = Iowa (IA)          20 = Kansas (KS)         21 = Kentucky (KY)
22 = Louisiana (LA)     23 = Maine (ME)          24 = Maryland (MD)
25 = Massachusetts (MA) 26 = Michigan (MI)       27 = Minnesota (MN)
28 = Mississippi (MS)   29 = Missouri (MO)       30 = Montana (MT)
31 = Nebraska (NE)      32 = Nevada (NV)         33 = New Hampshire (NH)
34 = New Jersey (NJ)    35 = New Mexico (NM)     36 = New York (NY)
37 = North Carolina (NC) 38 = North Dakota (ND)   39 = Ohio (OH)
40 = Oklahoma (OK)      41 = Oregon (OR)         42 = Pennsylvania (PA)
44 = Rhode Island (RI)  45 = South Carolina (SC) 46 = South Dakota (SD)
47 = Tennessee (TN)     48 = Texas (TX)          49 = Utah (UT)
50 = Vermont (VT)       51 = Virginia (VA)       53 = Washington (WA)
54 = West Virginia (WV) 55 = Wisconsin (WI)      56 = Wyoming (WY)
```

## 5. Occupation Codes (SOC)
Source: https://www.bls.gov/soc/2018/major_groups.htm

Major Groups:
```
11-0000 = Management Occupations
13-0000 = Business and Financial Operations Occupations
15-0000 = Computer and Mathematical Occupations
17-0000 = Architecture and Engineering Occupations
19-0000 = Life, Physical, and Social Science Occupations
21-0000 = Community and Social Service Occupations
23-0000 = Legal Occupations
25-0000 = Educational Instruction and Library Occupations
27-0000 = Arts, Design, Entertainment, Sports, and Media Occupations
29-0000 = Healthcare Practitioners and Technical Occupations
31-0000 = Healthcare Support Occupations
33-0000 = Protective Service Occupations
35-0000 = Food Preparation and Serving Related Occupations
37-0000 = Building and Grounds Cleaning and Maintenance Occupations
39-0000 = Personal Care and Service Occupations
41-0000 = Sales and Related Occupations
43-0000 = Office and Administrative Support Occupations
45-0000 = Farming, Fishing, and Forestry Occupations
47-0000 = Construction and Extraction Occupations
49-0000 = Installation, Maintenance, and Repair Occupations
51-0000 = Production Occupations
53-0000 = Transportation and Material Moving Occupations
```

## 6. Data Types
Known codes from testing:
```
01 = Employment count
03 = Hourly mean wage
13 = Annual mean wage
04 = (needs verification)
```

## 7. Example Requests

### National Level - Registered Nurses Employment
```json
{
  "seriesid": ["OEUN00000000000029114101"],
  "startyear": "2023",
  "endyear": "2023",
  "registrationkey": "YOUR_KEY"
}
```

### State Level - California RN Wages
```json
{
  "seriesid": ["OESN0600000000029114103"],
  "startyear": "2023",
  "endyear": "2023",
  "registrationkey": "YOUR_KEY"
}
```

## 8. Response Format
Example response:
```json
{
    "status": "REQUEST_SUCCEEDED",
    "responseTime": 154,
    "message": [],
    "Results": {
        "series": [
            {
                "seriesID": "OEUN000000000000291141XX",
                "data": [
                    {
                        "year": "2023",
                        "period": "A01",
                        "periodName": "Annual",
                        "latest": "true",
                        "value": "XXXXX",
                        "footnotes": [{}]
                    }
                ]
            }
        ]
    }
}
```

Note: This documentation is based on testing and available BLS documentation. Some details may need verification or updates as the API evolves.
