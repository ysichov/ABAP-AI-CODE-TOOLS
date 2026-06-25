# ABAP-AI-CODE-TOOLS

Optional **write / code-mutation** add-on for the [ABAP-AI-Code](../ABAP-AI-Code) platform:
tools that **create, modify and delete** ABAP repository objects, plus the object
saver that actually writes to the system.

These are deliberately kept **out of the read-only base** so a system can run the
AI platform (and add-ons such as the OTR translator) **without** the ability to
mutate ABAP code. Install this package only where autonomous/assisted code
changes are intended.

## Contents
- `zcl_code_object_saver` — performs the actual write/delete via standard SAP
  APIs (`RPY_*`, `RS_DELETE_PROGRAM`, `RS_CORR_INSERT`, `RS_WORKING_OBJECTS_ACTIVATE`).
- `zcl_aitool_create` / `zcl_aitool_modify` / `zcl_aitool_delete` — the CUD tool
  plugins (`zif_ai_tool` via `zcl_aitool_base`).

## Dependencies & install order
abapGit does not resolve dependencies automatically. Install **base first**:

1. `ABAP-AI-Code` (read-only base: core, engine, base/read/review tools) — required.
2. `ABAP-AI-CODE-TOOLS` (this repo).

The base does **not** depend on this package. The runner and the code UI call the
saver purely by dynamic name (`CALL METHOD ('ZCL_CODE_OBJECT_SAVER')=>...`); when
this package is absent the write paths fail gracefully ("read-only platform").

## Agent files (not abapGit objects)
Each CUD tool needs its companion `<tool_name>.json` (schema) and `<tool_name>.md`
(prompt fragment) copied into the agents folder. Without them the tool factory
**refuses to register the tool** with an explicit warning naming the missing file.
