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

### Roadmap

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
