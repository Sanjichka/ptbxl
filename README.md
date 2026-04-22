**Jupyter Notebook - Precleaning & preprocessing the data**

🔍 1. Data Exploration and Initial Assessment

The process begins with the ingestion of the PTB-XL dataset. I conduct a "health check" of the raw data, look at the shape of the dataframe and the presence of null values, analyze the frequency and unique counts of variables, understand the columns and the data they represent.

✂️ 2. Dimensionality Reduction

To streamline the dataset for dashboard performance, I drop technical metadata and noise-related columns (such as burst_noise, static_noise, and filename_hr) that do not provide direct clinical or business value for a high-level dashboard.

🏷️ 3. Semantic Mapping and Readability

To ensure the final dashboard is intuitive for the stakeholders, I translate encoded values into human-readable labels.
Numerical identifiers for Nurse, Site, and Validated_by are converted into descriptive strings (e.g., "Nurse 1" instead of "1").
Binary codes in the sex column are explicitly mapped to 'male' and 'female', removing the need for users to memorize coding schemas.

⚙️ 4. Precision Type Casting and Standardization

By casting age, patient_id, height, and weight to the Int64 type, I preserve the integer format (avoiding the common issue of IDs appearing as 1024.0) while safely maintaining NaN values for missing patient data.
Temporal Alignment: The recording_date is converted from a generic string to a formal Datetime object with UTC normalization. This standardizes all timestamps to a global constant, which is essential for accurate time-series analysis in BigQuery.

☁️ 5. Cloud Integration (BigQuery Load)

The final stage is the automated migration to Google Cloud Platform (GCP). After installing the necessary client libraries and authenticating via a Service Account, the cleaned df_reduced dataframe is pushed to a specific BigQuery dataset.


**BigQuery - final data processing before forwarding to the visual dashboard**

📊 1. SQL Transformation & Feature Engineering

Once the data is migrated to the cloud, I use BigQuery SQL to finalize the data processing, creating final datasets optimized for BI dashboards and deep analytical queries.

🧬 2. Diagnosis Aggregation & Demographic Stratification

To enhance clinical insights, I expand the dataset with custom dimensions:
- Automated Parsing: Using Regex and CTEs, I extract diagnoses from encoded strings where the confidence value is $\ge 50$ and aggregate them into a readable format.
- Age Categorization: I implement CASE logic to stratify patients into demographic groups (e.g., 19-39, 40-59), enabling seamless filtering and cohort analysis within the dashboard.

📈 3. Clinical Superclass Mapping

I generate specialized tables that simplify complex medical codes into intuitive categories:
- Data Unnesting: I transform nested SCP codes into a flat structure via UNNEST and SPLIT for better compatibility with visualization tools.
- Superclass Logic: Individual diagnoses are mapped into high-level clinical groups like Myocardial Infarction, Conduction Disturbance, and Hypertrophy, making the data accessible for non-technical stakeholders.
