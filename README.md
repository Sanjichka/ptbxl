*Jupyter Notebook - Precleaning & preprocessing the data*

1. Data Exploration and Initial Assessment

The process begins with the ingestion of the PTB-XL dataset. I conduct a "health check" of the raw data, look at the shape of the dataframe and the presence of null values, analyze the frequency and unique counts of variables, understand the columns and the data they represent.

2. Dimensionality Reduction

To streamline the dataset for dashboard performance, I drop technical metadata and noise-related columns (such as burst_noise, static_noise, and filename_hr) that do not provide direct clinical or business value for a high-level dashboard.

3. Semantic Mapping and Readability

To ensure the final dashboard is intuitive for the stakeholders, I translate encoded values into human-readable labels.
Numerical identifiers for Nurse, Site, and Validated_by are converted into descriptive strings (e.g., "Nurse 1" instead of "1").
Binary codes in the sex column are explicitly mapped to 'male' and 'female', removing the need for users to memorize coding schemas.

4. Precision Type Casting and Standardization

By casting age, patient_id, height, and weight to the Int64 type, I preserve the integer format (avoiding the common issue of IDs appearing as 1024.0) while safely maintaining NaN values for missing patient data.
Temporal Alignment: The recording_date is converted from a generic string to a formal Datetime object with UTC normalization. This standardizes all timestamps to a global constant, which is essential for accurate time-series analysis in BigQuery.

5. Cloud Integration (BigQuery Load)

The final stage is the automated migration to Google Cloud Platform (GCP). After installing the necessary client libraries and authenticating via a Service Account, the cleaned df_reduced dataframe is pushed to a specific BigQuery dataset.
