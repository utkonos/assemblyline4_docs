[comment]: # (AUTOGENERATED MARKDOWN CONTENT. UPDATES TO ODM DOCUMENTATION SHOULD BE DONE THROUGH ASSEMBLYLINE-BASE REPO!)
# Heuristic
> Model of Service Heuristics

| Field | Type | Description | Required | Default |
| :--- | :--- | :--- | :--- | :--- |
| attack_id | List [Keyword] | List of all associated ATT&CK IDs | :material-checkbox-marked-outline: Yes | `[]` |
| classification | Classification | Classification of the heuristic | :material-checkbox-marked-outline: Yes | `TLP:W` |
| description | Text | Description of the heuristic | :material-checkbox-marked-outline: Yes | `None` |
| filetype | Keyword | What type of files does this heuristic target? | :material-checkbox-marked-outline: Yes | `None` |
| heur_id | Keyword | ID of the Heuristic | :material-checkbox-marked-outline: Yes | `None` |
| name | Keyword | Name of the heuristic | :material-checkbox-marked-outline: Yes | `None` |
| score | Integer | Default score of the heuristic | :material-checkbox-marked-outline: Yes | `None` |
| signature_score_map | Mapping [Integer] | Score of signatures for this heuristic | :material-checkbox-marked-outline: Yes | `{}` |
| stats | [Statistics](/assemblyline4_docs/odm/models/statistic/#statistics) | Statistics related to the Heuristic | :material-checkbox-marked-outline: Yes | See [Statistics](/assemblyline4_docs/odm/models/statistic/#statistics) for more details. |
| max_score | Integer | Maximum score for heuristic | :material-minus-box-outline: Optional | `None` |

