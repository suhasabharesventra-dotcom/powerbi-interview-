# Power BI refresh options

## Which part needs to be updated?

```mermaid
flowchart TD
    A[What changed?] --> B{Changed the report file?}
    B -->|Yes| C[Package refresh<br/>OneDrive / SharePoint sync]
    C --> C1[Updates the PBIX/report artifact<br/>Does not query the data source]
    B -->|No| D{Need fresh source data?}
    D -->|Yes| E[Model refresh<br/>Dataset / semantic model]
    E --> E1[Queries the source and updates<br/>the imported model data]
    D -->|No| F{Need dashboard or on-screen visuals updated?}
    F -->|Dashboard tile| G[Tile refresh]
    G --> G1[Refreshes dashboard tile data/cache<br/>Timing depends on service and source]
    F -->|Report open in browser| H[Visual container refresh]
    H --> H1[Refreshes visuals in the report view<br/>from the model; does not itself load<br/>new source data]

    C1 --> I[Choose the refresh that matches<br/>what actually changed]
    E1 --> I
    G1 --> I
    H1 --> I

    classDef package fill:#E8F1FF,stroke:#2878D0,color:#102A43,stroke-width:2px;
    classDef model fill:#E6F6EF,stroke:#16865A,color:#12372A,stroke-width:2px;
    classDef tile fill:#FFF4D6,stroke:#D99A00,color:#493300,stroke-width:2px;
    classDef visual fill:#F3E9FF,stroke:#8055C7,color:#30204A,stroke-width:2px;
    class C,C1 package;
    class E,E1 model;
    class G,G1 tile;
    class H,H1 visual;
```

### Quick comparison

| Refresh option | What it updates | Typical use |
|---|---|---|
| **Package refresh** | The report/package file synced from OneDrive or SharePoint Online. It does **not** refresh the underlying model data by itself. | A PBIX was edited and saved to OneDrive or SharePoint, and the Service should pick up those file changes. |
| **Model (data) refresh** | Data in the dataset/semantic model, by querying its connected sources. | Bring imported data up to date on a schedule or with a manual refresh. |
| **Tile refresh** | Dashboard tile data/cache. This is separate from loading new data into the model, and its timing depends on the service and configuration. | Update dashboard tiles after the model changes, or refresh a tile when needed. |
| **Visual container refresh** | Visuals in an open report view using the model's available data. It does not independently query the original source for new model data. | Update the report's displayed visuals; use model refresh when the source data itself must be reloaded. |

> **Interview takeaway:** Match the refresh to the layer that changed: the **file**, the **model data**, the **dashboard tile**, or the **report view**. For example, after a PBIX edit stored in SharePoint, package refresh syncs the file; a separate model refresh may still be needed to load new source data.
