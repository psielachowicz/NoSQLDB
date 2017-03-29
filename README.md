# NoSQLDB
projekt zaliczeniowy nosql

## Paweł Sielachowicz


### ZadanieGEO

Wybrany zbior danych:
https://catalog.data.gov/dataset/waste-water-facilities

Podstawowe pola:

- "FacilityName":{"type":"text"} (Nazwa placówki)
- "FacilityID":{"type":"integer"} (ID)
- "WWInventoryURL":{"type":"text"} (Adres strony www)
- "geometry":{"type": "geo_point"} (kordynaty)

#### Elasticsearch
0. Fiddler: http://www.telerik.com/fiddler
1. Import danych z użyciem fiddler'a i pliku:
-MAPDA.geojson
- composer > post > http://localhost:9200/water/facilities
2. Zapytania
- a) Koperta (Dokumentacja mock)
``` json
{
    "query":{
        "bool": {
            "must": {
                "match_all": {}
            },
            "filter": {
                "geo_shape": {
                    "location": {
                        "shape": {
                            "type": "envelope",
                            "coordinates" : [[13.0, 53.0], [14.0, 52.0]]
                        },
                        "relation": "within"
                    }
                }
            }
        }
    }
}
```
- b) Dystans (Dokumentacja mock)
``` json
{
    "query": {
        "bool" : {
            "must" : {
                "match_all" : {}
            },
            "filter" : {
                "geo_distance" : {
                    "distance" : "200km",
                    "pin.location" : {
                        "lat" : 40,
                        "lon" : -70
                    }
                }
            }
        }
    }
}
```

- c) Poligon (Dokumentacja mock)
``` json
{
    "query": {
        "bool" : {
            "must" : {
                "match_all" : {}
            },
            "filter" : {
                "geo_polygon" : {
                    "person.location" : {
                        "points" : [
                        {"lat" : 40, "lon" : -70},
                        {"lat" : 30, "lon" : -80},
                        {"lat" : 20, "lon" : -90}
                        ]
                    }
                }
            }
        }
    }
}
```

#### PostgreSQL
1. Utworzenie tabeli z pliku:
-postgreSQL.txt
2. Import danych za pomocą pgAdmin
- tabela wwf > PPM import... c:/wwf.csv
3. Zapytania (WWW mock) 
``` SQL
SELECT * FROM wwf WHERE ST_MakeEnvelope(-79.9, 40.44, -79.899, 40.441, 4326) ~ coordinates;
```


- [ ] Aggregation Pipeline

(egzamin)

- [ ] MapReduce

Informacje o komputerze na którym były wykonywane obliczenia:

| Nazwa                 | Wartość    |
|-----------------------|------------|
| System operacyjny     | Win 8.1 Pro x64 |
| Procesor              | AMD APU A10-5700 |
| Pamięć                | 4GB |
| Dysk                  | SSD SP S55 240GB |
| Baza danych           | PostgreSQL |

