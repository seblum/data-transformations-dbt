# dbt-cloud-model-repository

This repository contains code and resources for the **dbt Cloud + BigQuery** tutorial, covering all 15 steps from the [official dbt documentation](https://docs.getdbt.com/guides/bigquery). Follow this guide to set up a complete analytics environment with dbt Cloud and Google BigQuery.

---

## Prerequisites

Before starting, ensure you have:
- **dbt Cloud Account**: Sign up for free at [dbt Cloud](https://cloud.getdbt.com/signup/).
- **Google Account**: Required for accessing Google Cloud Platform (GCP) and BigQuery.

---
# dbt Cloud + BigQuery Quickstart Guide

This repository contains resources and code for the **dbt Cloud + BigQuery Quickstart** tutorial. Follow this guide to set up and use dbt Cloud with BigQuery, learn the basics of building models, adding tests and documentation, and automating workflows.

---

## Overview

This guide will teach you how to:
1. Create a Google Cloud Platform (GCP) project.
2. Access sample data in a public dataset.
3. Connect dbt Cloud to BigQuery.
4. Turn a sample query into a dbt model.
5. Add tests to your models.
6. Document your models.
7. Schedule a job to run automatically.

---

## Prerequisites

Before starting, ensure you have:
- **Google Cloud Account**: To create and access your BigQuery project.
- **dbt Cloud Account**: Sign up for free at [dbt Cloud](https://cloud.getdbt.com/signup/).

---

### Create a GCP Project
- Log in to the [Google Cloud Console](https://console.cloud.google.com/).
- Create a new project (e.g., `dbt-bigquery-tutorial`).
- Enable the **BigQuery API** for your project.
- Create an example Dataset
- An Infrastructure as Code Stack to generate the resources can be found at [seblum/iac-google-cloud](https://github.com/seblum/iac-google-cloud/tree/main/projects/data-transformations-dbt).

### Access Sample Data
- Open the [BigQuery Console](https://console.cloud.google.com/bigquery).
- Use the following queries to explore sample data:
  ```sql
  SELECT * FROM `dbt-tutorial.jaffle_shop.customers` LIMIT 10;
  SELECT * FROM `dbt-tutorial.jaffle_shop.orders` LIMIT 10;
  SELECT * FROM `dbt-tutorial.stripe.payment` LIMIT 10;
  ```

### Connect dbt Cloud to BigQuery
1. In dbt Cloud, create a new project.
2. Select **BigQuery** as the data warehouse.
3. Generate and download a service account JSON key file from GCP.
4. Upload the key file in dbt Cloud to configure the connection.
5. Test the connection to ensure it works correctly.

### Create Your First Model
- In the dbt Cloud IDE, create a new file under the `models/` directory.
- Write a SQL `SELECT` statement to transform data:
  Example: `models/customers.sql`
  ```sql
  SELECT * FROM `dbt-tutorial.jaffle_shop.customers`;
  ```
- Run the model to generate a table in BigQuery.

### Change the Way Your Model is Materialized
One of the most powerful features of dbt is that you can change how a model is materialized in your warehouse simply by updating configuration values. 

1. Set Default Materialization 
   Edit the `dbt_project.yml` file:
   ```yaml
   name: 'jaffle_shop'

   models:
     jaffle_shop:
       +materialized: table
     example:
       +materialized: view
   ```
   - Save the file.
   - Run `dbt run`. The `customers` model will now be built as a table.

2. Override Materialization for a Specific Model 
   In `models/customers.sql`, add a configuration snippet to override the project-level default:
   ```sql
   {{
     config(
       materialized='view'
     )
   }}

   with customers as (
       select
           id as customer_id
           ...
   )
   ```
   - Save the file.
   - Run `dbt run --full-refresh` to rebuild the `customers` model as a view.

### Add Tests to Your Models
- Add schema tests in a `schema.yml` file to validate your model:
  Example:
  ```yaml
  version: 2

  models:
    - name: customers
      description: "This table contains customer data."
      columns:
        - name: customer_id
          tests:
            - unique
            - not_null
  ```

### Document Your Models
- Use the `schema.yml` file to add documentation for your models and columns.
- Run `dbt docs generate` to build the documentation site.

### Schedule a Job
- In dbt Cloud, go to the **Jobs** tab and create a new job.
- Configure the job to:
  1. Run your models.
  2. Test your models.
  3. Generate documentation.
- Set the schedule for automatic execution (e.g., daily).

---

## Resources

- [dbt Documentation](https://docs.getdbt.com/)
- [BigQuery Documentation](https://cloud.google.com/bigquery/docs)
- [dbt Learn Courses](https://learn.getdbt.com/)

This repository provides the completed files and templates for each step of the guide. Feel free to explore and modify as needed!
