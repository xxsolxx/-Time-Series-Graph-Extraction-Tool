# Time Series Extraction and Forecasting Pipeline

## Project Overview
This project implements an end-to-end automated pipeline that extracts time-series data from images of line charts and performs advanced forecasting. It bridges the gap between raw visual data (pixels) and mathematical time-series modeling, effectively reverse-engineering a graph back into actionable predictive insights.

## Acknowledgements
This project relies on the core extraction capabilities of the `extract-line-chart-data` repository to initially process the images and extract raw pixel coordinates. 
* **Original Repository:** [(https://github.com/tdsone/extract-line-chart-data)]
* We highly appreciate the author's work in providing the underlying OCR and line-parsing framework.

## Environment Setup
This pipeline was developed and tested using **WSL (Windows Subsystem for Linux)**. It is highly recommended to run this project within a Linux environment (or WSL on Windows) to ensure seamless compatibility with the underlying extraction scripts and file path operations.

## How It Works (Methodology)

The pipeline executes the following sequence of operations:

1. **Visual Data Extraction:** Utilizing the aforementioned base repository, the system reads a target line chart image and outputs the raw pixel coordinates (X, Y) of the plotted line into a JSON format.
2. **Mathematical Normalization (Min-Max Scaling):** Raw pixel coordinates are inherently meaningless to predictive models. The code dynamically maps these pixels to the real-world numerical scale of the graph's axes using linear interpolation.
3. **Discretization (Downsampling):** The initial extraction produces thousands of dense pixel points (noise). The pipeline rounds the X-axis values to integers and calculates the mean Y-value for each distinct time step, condensing the continuous line into a finite set of clean, discrete data points (e.g., 20 historical steps).
4. **Time Series Forecasting:** The cleaned dataset is fed into the `statsforecast` library. We utilize an **AutoARIMA** model to perform rolling forecasts via Temporal Cross-Validation.
5. **Evaluation & Probability:** The model's accuracy is evaluated using the sMAPE (Symmetric Mean Absolute Percentage Error) metric. Finally, by mapping the errors to a Normal Distribution (`scipy.stats.norm`), the system calculates the exact statistical probability of the next prediction deviating beyond an acceptable threshold.

## Requirements
* Python 3.10+
* WSL (Ubuntu recommended)
* `statsforecast`, `utilsforecast`, `pandas`, `numpy`, `scipy`

## Usage
Simply run the `main_pipeline.ipynb` Jupyter Notebook. Ensure that your target image is placed in the designated `Test_image` folder before executing the cells.
