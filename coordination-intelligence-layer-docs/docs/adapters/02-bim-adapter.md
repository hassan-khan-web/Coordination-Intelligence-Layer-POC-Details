# BIM Adapter

## Overview
The `BIMAdapter` is the specific implementation that allows CIL to understand and coordinate Building Information Modeling (BIM) data within the A.E.C.O industry.

## Purpose
To bridge the gap between complex architectural schemas (such as IFC) and the domain-neutral CIL contracts (`Node`, `Dependency`).

## Architecture
Located at `cil-runtime/adapters/bim_adapter.py`, the adapter registers itself with the CIL runtime upon startup. It listens for domain-specific events (e.g., "Wall Removed") and translates them.

## Internal Workflow
1. **Event Ingestion**: Receives an event from an external BIM pipeline.
2. **Entity Translation**: Maps a BIM element (e.g., an `IfcWall`) to a generic `Node`. It attaches crucial metadata, such as `discipline` (Structural, MEP, Arch) and `ifc_type`.
3. **Relationship Mapping**: Derives physical relationships (e.g., "Beam supports Slab" or "Duct passes through Wall") and writes these as edges to the Graph Engine.

## Discipline-Aware Coordination
While CIL is domain-neutral, the BIMAdapter encodes A.E.C.O logic into the metadata. This allows the Decision Engine to execute policies based on discipline.
- **Structural**: High-impact changes to load-bearing elements.
- **MEP**: Changes to Mechanical, Electrical, and Plumbing systems.
- **Cross-Discipline**: The adapter ensures that a structural change (wall removal) properly maps its impact to attached MEP elements (a duct).

## Examples
```python
# A conceptual example of BIM translation
def translate_to_node(ifc_element):
    return Node(
        id=ifc_element.guid,
        metadata={
            "discipline": "Structural" if ifc_element.is_load_bearing else "Arch",
            "ifc_type": ifc_element.type
        }
    )
```

## Related Topics
- [Adapter Architecture](01-adapter-architecture.md)
- [Domain Neutrality](../concepts/03-domain-neutrality.md)
