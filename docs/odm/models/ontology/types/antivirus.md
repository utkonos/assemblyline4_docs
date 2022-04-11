[comment]: # (AUTOGENERATED MARKDOWN CONTENT. UPDATES TO ODM DOCUMENTATION SHOULD BE DONE THROUGH ASSEMBLYLINE-BASE REPO!)
# Antivirus
> Antivirus Ontology Model

| Field | Type | Description | Required | Default |
| :--- | :--- | :--- | :--- | :--- |
| odm_version | Text | Version of antivirus ontological result | :material-checkbox-marked-outline: Yes | `2.0` |
| detections | List [[Detection](/assemblyline4_docs/odm/models/ontology/types/antivirus/#detection)] | List of antivirus detections | :material-checkbox-marked-outline: Yes | `None` |


[comment]: # (AUTOGENERATED MARKDOWN CONTENT. UPDATES TO ODM DOCUMENTATION SHOULD BE DONE THROUGH ASSEMBLYLINE-BASE REPO!)
## Detection
> Antivirus Detection Model

| Field | Type | Description | Required | Default |
| :--- | :--- | :--- | :--- | :--- |
| engine_name | Keyword | Name of antivirus engine | :material-checkbox-marked-outline: Yes | `None` |
| engine_version | Keyword | Version of antivirus engine | :material-minus-box-outline: Optional | `None` |
| engine_definition_version | Keyword | Version of definition set | :material-minus-box-outline: Optional | `None` |
| virus_name | Keyword | The name of the virus | :material-minus-box-outline: Optional | `None` |
| category | Enum | What category does the verdict fall under?<br><ul><li>`type-unsupported`: File sent to antivirus is unsupported</li><li>`undetected`: File not detected by antivirus</li><li>`failure`: Antivirus failed during detection</li><li>`suspicious`: Antivirus deems suspicious</li><li>`malicious`: Antivirus deems malicious</li></ul><br>Values:<br>`"undetected", "failure", "malicious", "type-unsupported", "suspicious"` | :material-minus-box-outline: Optional | `None` |

