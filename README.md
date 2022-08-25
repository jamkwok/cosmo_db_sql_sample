# cosmo_db_sql_sample
Sample Queries for Cosmo DB, nested query search using JOINS

Data set example
```json
{
    "id": "Anderson.1",
    "Country": "USA",
    "partitionKey": "USA",
    "lastName": "Andersen",
    "parents": [
        {
            "firstName": "Thomas"
        },
        {
            "firstName": "Mary Kay"
        }
    ],
    "children": [
        {
            "firstName": "Henriette Thaulow",
            "gender": "female",
            "grade": 5,
            "pets": [
                {
                    "name": "fluffy",
                    "type": "cat"
                }
            ]
        }
    ],
    "address": {
        "state": "WA",
        "county": "King",
        "city": "Seattle"
    }
},
{
    "id": "Wakefield.7",
    "partitionKey": "Italy",
    "Country": "Italy",
    "parents": [
        {
            "familyName": "Wakefield",
            "firstName": "Robin"
        },
        {
            "familyName": "Miller",
            "firstName": "Ben"
        }
    ],
    "children": [
        {
            "familyName": "Merriam",
            "firstName": "Jesse",
            "gender": "female",
            "grade": 8,
            "pets": [
                {
                    "name": "yolo",
                    "type": "dog"
                },
                {
                    "name": "whisker",
                    "type": "cat"
                }
            ]
        },
        {
            "familyName": "Miller",
            "firstName": "Lisa",
            "gender": "female",
            "grade": 1,
            "pets": [
                {
                    "name": "rex",
                    "type": "dog"
                }
            ]
        }
    ],
    "address": {
        "state": "NY",
        "county": "Manhattan",
        "city": "NY"
    }
}
```
# Dog query
```sql
SELECT VALUE children FROM root r JOIN children IN r.children JOIN pet IN children.pets WHERE pet.type = "dog"
```
Result
```json
{
	"familyName": "Merriam",
	"firstName": "Jesse",
	"gender": "female",
	"grade": 8,
	"pets": [{
		"name": "yolo",
		"type": "dog"
	}, {
		"name": "whisker",
		"type": "cat"
	}]
}

{
	"familyName": "Miller",
	"firstName": "Lisa",
	"gender": "female",
	"grade": 1,
	"pets": [{
		"name": "rex",
		"type": "dog"
	}]
}
```

# Cat query
```sql
SELECT VALUE children FROM root r JOIN children IN r.children JOIN pet IN children.pets WHERE pet.type = "cat"
```
Result
```json
{
	"firstName": "Henriette Thaulow",
	"gender": "female",
	"grade": 5,
	"pets": [{
		"name": "fluffy",
		"type": "cat"
	}]
}

{
	"familyName": "Merriam",
	"firstName": "Jesse",
	"gender": "female",
	"grade": 8,
	"pets": [{
		"name": "yolo",
		"type": "dog"
	}, {
		"name": "whisker",
		"type": "cat"
	}]
}
```