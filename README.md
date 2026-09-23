# IT-Kamianets 3D Engine: Scene Schema

`3d-scene-schema` defines the engine-independent data model used across the **IT-Kamianets 3D Engine ecosystem**.

It is the shared contract between Unity, future Unreal Engine applications, web applications, AI systems, geometry services, APIs, and other tools.

### Logic

The schema should be the **source of truth for a 3D scene**.

It must not depend on concepts specific to Unity or Unreal Engine.

Instead, it describes universal concepts such as scenes, objects, transforms, assets, materials, lights, cameras, relationships, metadata, and behaviors.

This allows:

**one scene → Unity / Unreal / Web / AI / external tools**

without rewriting the scene format for every runtime.

### Schema v1 (scanner MVP)

The first milestone across this workspace is getting `vroom-scanner` working end-to-end, so v1 only covers what a Meta Quest room scan actually produces — not the full roadmap below. Files live under [`schema/v1/`](schema/v1/), written as JSON Schema (2020-12), since JSON is what gets stored in Firestore and it's the one format every consumer (Unity, Unreal, web, AI tooling) can read without extra tooling.

* [`scene.schema.json`](schema/v1/scene.schema.json) — root document. Holds `schemaVersion`, `sceneId`, timestamps, units/coordinate convention, an `origin`, and the flat list of `objects`. Nothing else — no identifiers for what building, floor, room, or product this scene belongs to. That organizational/business context is entirely out of scope for this repo; it's owned by whatever application stores and indexes scenes (e.g. `vroom-sync`), the same way this repo carries no Unity- or Unreal-specific fields.
* [`scene-object.schema.json`](schema/v1/scene-object.schema.json) — one scanned element: a `SemanticLabel` (`ROOM`, `FLOOR`, `CEILING`, `WALL_FACE`, `DOOR_FRAME`, `WINDOW_FRAME`, plus common furniture labels), a `transform`, an optional `boundary` (2D polygon, for planar surfaces) or `volume` (3D bounds, for furniture), and a `parentId` for hierarchy. The label set mirrors Meta Quest's Scene API / MRUK semantic classifications so `3d-unity-spatial` can map scan results across with minimal translation.
* [`transform.schema.json`](schema/v1/transform.schema.json) — position/rotation/scale.
* [`common.schema.json`](schema/v1/common.schema.json) — shared primitives (`Vector2`, `Vector3`, `Quaternion`, `Polygon2D`, `Bounds3D`, `Metadata`).

**Conventions:** meters, right-handed Y-up. Engine adapters (`3d-unity-scene`) are responsible for converting to/from their own engine's handedness — this schema does not assume any engine's conventions, including Unity's.

**A scene doesn't know where it lives.** Scanning a large interior in one continuous walkthrough (no re-anchoring mid-walk) naturally produces several scenes that share one coordinate frame — but this schema has no concept of "floor" or "space" to group them, because that's an application-level organizational choice, not an engine one. `origin` is how a scene records its offset *within whatever shared parent frame the caller defines*: composing `origin` with an object's `transform` places that object in the parent frame. What that parent frame represents (a floor of a building, a wing, anything else) is entirely up to the application storing the scenes — see `vroom-sync` for how VRoom organizes them.

**Versioning:** every root document carries a `schemaVersion` (semver). Breaking changes get a new `schema/v{n}/` directory rather than mutating v1 in place; consumers read `schemaVersion` to know which schema a stored document conforms to.

Example instances: [`examples/room-scan.example.json`](examples/room-scan.example.json) and [`examples/corridor-scan.example.json`](examples/corridor-scan.example.json) — two scenes sharing a coordinate frame, each with its own `origin`.

Deliberately deferred to a later milestone: materials, lights, cameras, asset references, constraints, behaviors, and Unreal integration — none of these are produced by the first application built on this schema (`vroom-scanner`).

### Full roadmap

* Scene definition
* Object definition
* Transform definition
* Asset references
* Materials
* Lights
* Cameras
* Object hierarchy
* Metadata
* Tags
* Constraints
* Behaviors
* Custom properties
* Units and coordinate conventions
* Schema validation
* Versioning
* Migration strategy
* TypeScript models
* C# models
* Python models
* Unity integration
* Unreal integration
* Documentation and examples
* Stable Scene Schema v1
