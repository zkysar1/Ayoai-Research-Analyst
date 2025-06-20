# Roblox JSON Examples

This document contains examples of Roblox JSON data structures used in the Ayoai cognitive architecture, extracted from the Cognition_Answer_List_Refactor_try_2.md file.

## Full World Example

This example shows the complete structure of a world update message sent from Roblox to the Ayoai Environment Server.

### Top-Level Structure
```json
{
  "ayoServerKey": "1726625801.948982",
  "ayoEnvironmentKey": "ZakFirst_Ayo_01.01",
  "environmentName": "default_server",
  "descendants": [...]
}
```

### Key Components

#### Units Container
The world contains a hierarchical structure starting with a "units" container:
- **pathArray**: [1] - Root level identifier
- **name**: "units"
- **ayoType**: "unit"
- **descendants**: Contains all world objects

#### Workspace
The main workspace contains all game objects:
- **ayoKey**: "ZakFirst_Ayo_01.01"
- **ClassName**: "Workspace"
- **pathArray**: [1, 1]
- **uniqueAyoUnitServerId**: "studioTesting_ZakFirst_Ayo_01.01_21be8f01-c108-4c84-9a4b-f6501df994eb"

Key workspace properties:
- **Gravity**: "35"
- **StreamingEnabled**: "false"
- **DistributedGameTime**: Current game time
- **GlobalWind**: "0, 0, 0"
- **CollisionGroup**: "AyoEnvironment"

#### Character Model Example
Characters are represented as Model objects with detailed properties:

```json
{
  "pathArray": [1, 1, 1, 1],
  "tags": [],
  "ayoKey": "behavior girl key",
  "ayoType": "character",
  "ClassName": "Model",
  "attributes": {
    "ayoPath": "1_1_1_1",
    "ayoKey": "behavior girl key",
    "uniqueAyoUnitServerId": "studioTesting_behavior girl key_a2fb8950-a938-462c-bbbd-2319d0cfddb4"
  },
  "PrimaryPart": "Head",
  "WorldPivot": "42, 0, -44.5, -1, 0, 0, 0, 1, 0, 0, 0, -1",
  "name": "behavior girl name",
  "descendants": [...]
}
```

#### Character Parts
Each character consists of multiple parts (Head, Torso, etc.) with detailed physics properties:

**Head Example**:
- **CFrame**: Position and rotation matrix
- **Mass**: "1.399999976158142"
- **Material**: "Enum.Material.Plastic"
- **Size**: "2, 1, 1"
- **CollisionGroup**: "AyoEnvironment"
- **AssemblyAngularVelocity**: "0, 0, 0"
- **AssemblyLinearVelocity**: "0, 0, 0"

**Attachments**: Characters have multiple attachment points:
- HairAttachment
- HatAttachment
- FaceFrontAttachment
- FaceCenterAttachment

#### Change Log System
Each unit maintains a detailed change log tracking all property changes:

```json
"changeLog": [
  {
    "1.7266258503784194E9": "Added data point RotVelocity with this value 0, 14.43499755859375, 0"
  },
  {
    "1.7266258510584493E9": "Updated data point RotVelocity; old value was 0, 14.43499755859375, 0"
  },
  {
    "1.7266258529381356E9": "Updated data point CFrame; old value was 42, 4.5, -44.5, -1, 0, 0, 0, 1, 0, 0, 0, -1"
  }
]
```

#### Behavior Journal
Each unit can maintain a behavior journal for tracking actions:
```json
"behaviorJournal": []
```

## Data Types and Patterns

### Path Arrays
- Used for hierarchical identification
- Example: [1, 1, 1, 1, 2] represents units → workspace → BehaviorTests → character → part

### ayoType Values
- "unit" - General game objects
- "character" - NPC or player characters

### Key Identifiers
- **ayoServerKey**: Timestamp-based server identifier
- **ayoEnvironmentKey**: Environment instance identifier
- **ayoKey**: Individual unit identifier
- **uniqueAyoUnitServerId**: Globally unique identifier combining server, key, and UUID

### Physical Properties
- **CFrame**: 12-value matrix for position and rotation
- **Velocity**: 3D vector for linear velocity
- **RotVelocity**: 3D vector for angular velocity
- **Mass**: Object mass for physics
- **AssemblyMass**: Total mass of connected parts

### Update Tracking
- Change logs use high-precision timestamps (nanoseconds)
- Track both additions and updates of properties
- Old values preserved for rollback/debugging

## Message Flow Integration

These JSON structures represent the "Unit Update", "Key Update", "Behavior Trees Update", and "Environment Update" messages that:
1. Flow from Roblox to the World Translator verticle
2. Get cleaned and distributed as Update.* messages
3. Feed into various perception verticles
4. Eventually influence NPC behavior through the cognitive architecture

## Ayoai Impact

This JSON structure design enables:
- **Hierarchical World Representation**: Path arrays allow efficient navigation of complex game worlds
- **Change Tracking**: Detailed logs enable temporal reasoning and learning
- **Physics Integration**: Full physics properties support realistic NPC movement
- **Unique Identification**: Multiple ID systems prevent conflicts in distributed environments
- **Behavior Attribution**: ayoType and behavior journals link objects to AI behaviors
- **Scalability**: Structure supports thousands of objects with efficient updates