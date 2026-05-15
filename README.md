# GeoSpatial_DS
Source code for Geospatial Data Science - Exam Project, Spring 2026

## Contributors 
Caroline Sofie Skovby (cssk@itu.dk) & Miriam Espinosa Solana (miri@itu.dk)

## Important Note 

There was a bug in git from the 22/4 to 29/4 where it would say that code was co-authored by Copilot, even though it wasn't and the user had disabled AI features. This is why it appears that one of our commits used AI, even though we did NOT. Source: https://github.com/microsoft/vscode/issues/314311

## Environment 
We are using a pixi environment like the one used in class. 

## Data folder structure

The `data/` directory contains raw, intermediate, and cleaned geospatial and tabular datasets used in the project.

```
data/
├── buildings_copenhagen/
│   ├── buildings.*
│   └── buildings_within_city.*
│
├── buildings_denmark/ # extracted from geoFabrik
│   └── gis_osm_buildings_a_free_1.*
│
├── clean_data/
│   ├── accommodation.gpkg
│   ├── air_quality.gpkg
│   ├── buildings.gpkg
│   ├── bus_stops.gpkg
│   ├── clinic.gpkg
│   ├── green_areas.gpkg
│   ├── hospital.gpkg
│   ├── qol_index.gpkg
│   ├── schools.gpkg
│   ├── train_stations.gpkg
│   ├── transport.gpkg
│   └── voting_areas.gpkg
│
├── copenhagen_boundaries/
│   └── boundaries.*
│
└── raw_data/
    ├── CAV_25May2021.geojson
    ├── employment.xlsx
    ├── population_district.xlsx
    ├── unemployment.xlsx
    └── voting_areas.gpkg

´´´