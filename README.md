# Episode 13 - Avn Data Genie

Introducing an OSS system to allow users to query a database using natural language. This is a work in progress, and is not available to the public as of January 14, 2026 when the episode aired.

YouTube: https://www.youtube.com/watch?v=4xteVXJYbo0

Website: https://codeitwithai.com

## Overview

Jeff Fritz and I had been trying to use the various SQL MCP Servers that exist to give users the ability to generate reports from a text prompt. We found that this approach is flawed in the following ways:

* **Security**
  * Giving an MCP carte-blanche access to your database is a bad idea. No constraints.
* **Performance**
  * The MCPs we saw were gathering metadata on every request
* **Flexibility**
  * Tight-coupling to models and databases

We wanted something we could more easily control. Our answer is **AvnDataGenie**.

* **Generate Metadata Beforehand**
  * Include a Metadata Management tool so users can:
    * Annotate Tables with descriptions.
    * Provide aliases for tables and columns.
    * Provide constraints such as number of records returned.
    * Select fields that should not ever be displayed. Ex: SSN or other PII
* **Use a local model (Ollama) to generate SQL Queries** (optional)
  * Ollama runs locally and never shares your data structures with Internet models. 
* **Performant architecture**
  * Metadata is loaded in before use.

## Features

### Database Schema Generation

- Connect to any SQL Server database using a connection string
- Automatically extract complete database metadata including:
  - Tables and columns with data types
  - Primary keys
  - Foreign key relationships
  - Indexes
  - Column constraints (nullable, default values, etc.)
- Export schema to JSON format for easy sharing and version control

### LLM Configuration Management

Configure how the LLM interacts with your database through a comprehensive web interface:

#### Tables & Columns Configuration

- **Friendly Names**: Provide user-friendly names for tables and columns (e.g., "Customers" instead of "tbl_cust")
- **Descriptions**: Add detailed descriptions explaining what each table and column represents
- **Aliases**: Define comma-separated alternative names that users might use (e.g., "Cust, Customer, Clients")
- **PII Marking**: Flag columns containing Personally Identifiable Information
- **Restricted Fields**: Mark columns that should never be included in query results
- Collapsible table view for easy navigation of large schemas

#### Join Hints

- **Auto-Generation**: Automatically generate join hints from foreign key relationships
- **Manual Control**: Add, edit, or remove join hints as needed
- **Custom Descriptions**: Provide hints to help the LLM understand complex relationships
- One-click regeneration from foreign keys

#### Required Filters

- Define filters that must always be applied to queries:
  - **Tenant Isolation**: Ensure multi-tenant data separation
  - **Soft Delete**: Automatically exclude deleted records
  - **Custom Filters**: Define any other mandatory filtering rules
- Specify default values for each filter

#### Business Terms

- Define domain-specific terminology (e.g., "Submitted Grants", "Active Customers")
- Provide definitions and examples for each term
- Help the LLM understand your business context and generate more accurate queries

### Configuration Persistence

- Save configuration to JSON file
- Load existing configurations
- Export configurations with timestamps
- Auto-load last configuration on startup

### 
