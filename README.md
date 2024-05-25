# Mining Impact Analysis Project

## Overview

This project aims to analyze the impact of mining activities on NDVI (Normalized Difference Vegetation Index) loss in the Central African Republic and the Democratic Republic of Congo (DRC). The analysis involves spatial data processing, sensitivity analysis, and visualization of various geographic features that influence mining activities.

## Sensitivity Analysis on Central African Republic

The initial phase of the project focused on conducting a sensitivity analysis on the Central African Republic. The following visualizations were generated as part of this analysis:

### Visualizations

1. **Mining and NDVI Loss Locations (Without Villages)**
   ![Mining and NDVI Loss Locations (Without Villages)](/mnt/data/caf-folium-without-villages.png)
   - **Description**: This map shows the locations of mines (in red) and NDVI loss areas (in blue) in the Central African Republic. It helps in understanding the spatial distribution of these locations without considering the village locations.

2. **Mining, NDVI Loss, and Village Locations**
   ![Mining, NDVI Loss, and Village Locations](/mnt/data/caf-folium-with-villages.png)
   - **Description**: This map adds village locations (in green) to the previous visualization, providing a comprehensive view of how mining and NDVI loss areas are distributed in relation to villages.

## Main Focus on Democratic Republic of Congo

The primary focus of the project is on the Democratic Republic of Congo. The analysis considers five key geographic features: waterways, protected areas, main roads, local roads, and village distributions. The following visualizations represent these features:

### Visualizations

3. **Waterways and Specific Point**
   ![Waterways and Specific Point](/mnt/data/waterways_congo.png)
   - **Description**: This map shows the distribution of waterways (in blue) in the DRC, with a specific point highlighted in red. It helps in analyzing the proximity of mining locations to water sources.

4. **Protected Areas and Specific Point**
   ![Protected Areas and Specific Point](/mnt/data/protected_areas.png)
   - **Description**: This map displays the protected areas (in green) in the DRC, with a specific point marked in red. It is used to assess the potential impact of mining activities on protected regions.

5. **Main Roads Distribution**
   ![Main Roads Distribution](/mnt/data/main_roads.png)
   - **Description**: This map illustrates the distribution of main roads (in blue) in the DRC. Understanding the road network is crucial for analyzing the accessibility and potential transportation routes for mining activities.

6. **Local Roads Distribution**
   ![Local Roads Distribution](/mnt/data/local_roads.png)
   - **Description**: This map highlights the local roads (in blue) in the DRC. It provides insights into the local connectivity and infrastructure around mining sites.

7. **Village Distribution**
   ![Village Distribution](/mnt/data/village_distribution.png)
   - **Description**: This map shows the distribution of villages (in blue) in the DRC, with a specific point marked in red. It is used to evaluate the impact of mining activities on local communities.

## Data Collection and Preprocessing

### Initial Data Collection

Initially, we attempted to use Google Earth Engine to select areas in a specific part of the DRC with NDVI loss > 0.15. However, this process took a long time without the possibility of parallelization and did not yield many data points (around 4000 points in an area of 4 latitude by 3 longitude).

### Alternative Approach

We then moved on to using AWS and S3 buckets, allowing us to add NDVI values and other band values to this preliminary data. After obtaining these locations, the focus was on matching and adding features of distances.

### Matching Strategy

1. **Matching**: 
   - **Strategy**: Use the mining location as the center of a circle, select a radius, and see which locations in the initial CSV file are contained within the circles.
   - **Labeling**: Points contained within the circle are labeled as 1, and others as 0.
   - **Radii Used**: 1000 meters and 5000 meters.
   - **Implementation**: Initially attempted on PySpark, but due to environment configuration issues, the process was run on the Midway SSD cluster.

2. **Adding Local Features**:
   - **Nearest Waterways, Local Roads, Main Roads, Protected Areas, and Villages**: Calculated the distance to the nearest road or feature using GeoPandas and GIS analysis.

### Data Preprocessing and Analysis

The data preprocessing involves several steps, including loading CSV and GeoJSON files, converting dataframes to GeoDataFrames, reprojecting CRS for distance calculations, and buffering mine locations. The main analysis calculates the nearest distances from NDVI loss points to various geographic features and checks if NDVI loss points are within buffered mine locations.

## Running the Analysis

The analysis is performed using MPI (Message Passing Interface) to parallelize the processing of NDVI loss locations across multiple processes. The results are gathered and saved to a new CSV file, which includes the potential mining indicator for each NDVI loss point.

## Conclusion

This project provides a comprehensive spatial analysis of mining impacts on vegetation loss in the DRC. The visualizations and distance calculations offer valuable insights into the relationship between mining activities and various geographic features.

---

Feel free to modify this README file as needed to better fit your project details and presentation style.
