# 100 - Entity-Relationship Diagram of a Screenplay

I'll create an Entity-Relationship Diagram for screenplay elements based on general screenwriting knowledge, being careful not to directly reproduce copyrighted material from any specific source.

Let me explain the Entity-Relationship Diagram I've created:

The diagram captures the key structural and narrative elements of a screenplay, showing how different components interact:

1. **Screenplay** is the central entity, which:
   - Contains multiple Acts
   - Features Characters
   - Includes Scenes
   - Explores a Theme

2. **Characters** are linked to:
   - Dialogues they speak
   - Their character arc (transformation)

3. **Scenes** are connected to:
   - Plot Points they advance
   - Conflicts they contain

The relationships show how these elements are interconnected, representing the complex structure of a screenplay. Each entity has key attributes that define its characteristics:
- Screenplay has title, genre, logline
- Characters have name, archetype, main goal
- Scenes have slugline, setting, action
- Plot Points have type and page number
- Conflicts have type and intensity

This diagram provides a comprehensive view of how different screenplay elements relate to each other, capturing the narrative structure from a systemic perspective.

I'll update the ERD to explicitly break down the slugline into its components.

I've made the following key changes:

1. Created a new **SLUGLINE** entity with specific attributes:
   - `sceneType`: Represents EXT (Exterior) or INT (Interior)
   - `location`: The specific location of the scene
   - `timeOfDay`: Represents DAY or NIGHT

2. Established a one-to-one relationship between SCENE and SLUGLINE, reflecting that each scene has exactly one slugline.

This update more precisely captures the standard screenplay slugline format, which provides crucial information about the scene's setting at a glance. The slugline is a critical element in screenwriting, quickly communicating to the reader (and potential production team) the basic environmental context of the scene.

```mermaid
---
title: Screenplay Entity-Relationship Diagram
---
erDiagram
    SCREENPLAY ||--o{ ACT : contains
    SCREENPLAY ||--o{ CHARACTER : features
    SCREENPLAY ||--o{ SCENE : includes
    SCREENPLAY {
        string title
        string genre
        string logline
        date creationDate
    }

    ACT {
        int actNumber
        string primaryPurpose
    }

    CHARACTER {
        string name
        string archetype
        string mainGoal
        string backgroundStory
    }

    SCENE ||--|| SLUGLINE : has
    SLUGLINE {
        string sceneType "EXT or INT"
        string location
        string timeOfDay "DAY or NIGHT"
    }

    SCENE {
        string action
    }

    CHARACTER ||--o{ DIALOGUE : speaks
    CHARACTER ||--o{ CHARACTER_ARC : undergoes

    DIALOGUE {
        string text
        string type
        int pageNumber
    }

    CHARACTER_ARC {
        string initialState
        string transformationPath
        string finalState
    }

    SCENE ||--|| PLOT_POINT : advances
    PLOT_POINT {
        string type
        string description
        int pageNumber
    }

    SCENE ||--o{ CONFLICT : contains
    CONFLICT {
        string type
        string intensity
        string resolution
    }

    THEME {
        string primaryTheme
        string subThemes
    }

    SCREENPLAY ||--|| THEME : explores
```


I'll update the ERD to reflect the breakdown of the Slugline into its three distinct components.

I've updated the Entity-Relationship Diagram to reflect the decomposition of the Slugline into its three component entities:

1. **SCENE_TYPE**:
   - Contains the `code` attribute (EXT or INT)
   - Includes a description field for additional context
   - Connected to the Slugline through a one-to-one relationship

2. **LOCATION**:
   - Contains the `name` attribute for the specific setting
   - Includes a description field for additional details
   - Has an `isRecurring` flag to indicate locations used multiple times
   - Connected to the Slugline through a one-to-one relationship

3. **TIME_OF_DAY**:
   - Contains the `code` attribute (DAY, NIGHT, DAWN, DUSK, etc.)
   - Includes a description field for clarification
   - Connected to the Slugline through a one-to-one relationship

The SLUGLINE entity now serves as a connecting entity that references these three components, maintaining the same relationship with SCENE as before.

This structure offers several advantages:
- Clearer representation of the slugline components
- Better data normalization
- Support for reusing locations across scenes
- Ability to standardize scene types and times of day
- Improved query capabilities (e.g., finding all scenes at the same location)

This updated ERD aligns perfectly with the microservice architecture in the NX monorepo structure we'll discuss later, where each of these entities has its own dedicated service.

```mermaid
---
title: Screenplay Entity-Relationship Diagram
---
erDiagram
    SCREENPLAY ||--o{ ACT : contains
    SCREENPLAY ||--o{ CHARACTER : features
    SCREENPLAY ||--o{ SCENE : includes
    SCREENPLAY {
        string title
        string genre
        string logline
        date creationDate
    }

    ACT {
        int actNumber
        string primaryPurpose
    }

    CHARACTER {
        string name
        string archetype
        string mainGoal
        string backgroundStory
    }

    SCENE ||--|| SLUGLINE : has
    SLUGLINE ||--|| SCENE_TYPE : uses
    SLUGLINE ||--|| LOCATION : references
    SLUGLINE ||--|| TIME_OF_DAY : indicates
    
    SCENE_TYPE {
        string code "EXT or INT"
        string description
    }
    
    LOCATION {
        string name
        string description
        boolean isRecurring
    }
    
    TIME_OF_DAY {
        string code "DAY, NIGHT, etc."
        string description
    }

    SCENE {
        string action
    }

    CHARACTER ||--o{ DIALOGUE : speaks
    CHARACTER ||--o{ CHARACTER_ARC : undergoes

    DIALOGUE {
        string text
        string type
        int pageNumber
    }

    CHARACTER_ARC {
        string initialState
        string transformationPath
        string finalState
    }

    SCENE ||--|| PLOT_POINT : advances
    PLOT_POINT {
        string type
        string description
        int pageNumber
    }

    SCENE ||--o{ CONFLICT : contains
    CONFLICT {
        string type
        string intensity
        string resolution
    }

    THEME {
        string primaryTheme
        string subThemes
    }

    SCREENPLAY ||--|| THEME : explores
```
