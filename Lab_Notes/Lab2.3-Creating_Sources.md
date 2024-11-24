# Lab 2.3: Create Sources

This lab demonstrates how to use the SailPoint IdentityNow REST API to create sources programmatically. The objective is to configure both a **Delimited File Source** and a **Directly Connected Source** using JSON request bodies and API endpoints. This process is fundamental for integrating external systems into SailPoint for identity management.

---

## Objectives
1. Learn how to use the IdentityNow REST API to create a new source.
2. Understand the structure of JSON request bodies (`employee_source.json`) used for creating sources.
3. Explore validation and verification steps through both the API and the IdentityNow UI.

---

## Instructions

### 1. **Retrieve Existing Sources**
   - Use the following API to retrieve a list of all existing sources in the tenant:
     ```http
     GET /v3/sources
     ```
   - This ensures you identify existing configurations and obtain any necessary IDs (e.g., for source ownership).

### 2. **Create a Delimited File Source**
   - Define the **Delimited File Source** using the `employee_source.json` file. This JSON file specifies:
     - **Source Name**: E.g., "Employees File."
     - **Owner Details**: ID of the administrator or team responsible for the source.
     - **VA Cluster Details**: Defines which Virtual Appliance manages the source.
     - **Connector Type**: File-based source configuration for delimited files.
   - Example API call:
     ```http
     POST /v3/sources?provisionAsCsv=true
     Content-Type: application/json
     Body: employee_source.json
     ```

### 3. **Create a Directly Connected Source**
   - Similarly, use the `employee_source.json` tailored for directly connected systems (e.g., Active Directory or other real-time connections). 
   - Key fields in this file include:
     - **Source Name**: E.g., "Employees Directory."
     - **Connector Type**: Real-time connector.
     - **Schema Details**: Specifies attributes such as account identifiers and entitlements.
   - Example API call:
     ```http
     POST /v3/sources
     Content-Type: application/json
     Body: employee_source.json
     ```

### 4. **Verify the Source**
   - Navigate to **Admin > Connections > Sources** in the IdentityNow UI.
   - Confirm the newly created sources appear in the list with the correct details.

### 5. **Update the Schema**
   - For Delimited File Sources:
     - Upload the schema through the IdentityNow UI to map attributes like `username` or `email`.
   - For Directly Connected Sources:
     - Configure real-time attribute mappings and provisioning options.

---

## File References
- **Delimited File Source**:
  - `employee_source.json`: Defines attributes and configurations required for creating a file-based source.
- **Directly Connected Source**:
  - `employee_source.json`: Modified JSON file for directly connected system configurations, including real-time provisioning capabilities.

---

## Example `employee_source.json`

Below is a sample JSON structure used for creating a **Delimited File Source**:

```json
{
  "name": "Employees File",
  "description": "Source for managing employee data from delimited files.",
  "type": "DelimitedFile",
  "ownerId": "12345",
  "cluster": {
    "id": "67890"
  },
  "connectorAttributes": {
    "fileType": "CSV",
    "delimiter": ","
  }
}

## Key Takeaways
- **Automate Source Creation**: Use JSON files with API calls to standardize and automate the configuration process.
- **Delimited vs. Direct Sources**:
  - **Delimited**: Ideal for batch file imports (e.g., CSV).
  - **Direct**: Real-time integrations with external systems.
- **Validation**: Always verify source creation via both API responses and the IdentityNow UI.
- For detailed specifications about the API, refer to the [SailPoint API Documentation](https://developer.sailpoint.com/docs/api/v2024/create-source).

