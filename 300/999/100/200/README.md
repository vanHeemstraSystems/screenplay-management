# 200 - Nx Cloud Monorepo of a Screenplay

I'll create an artifact that outlines how the screenplay elements could be structured as apps within an NX Cloud Monorepo.

Let me break down the NX Cloud Monorepo structure for the screenplay components:

1. **Monorepo Architecture**:
   - Each major screenplay element becomes a separate service or app
   - Separate services for: 
     - Screenplay Core
     - Act Service
     - Character Service
     - Scene Service
     - Slugline Service
     - Dialogue Service
     - Plot Point Service
     - Theme Service

2. **Key Design Principles**:
   - Shared core interfaces in `screenplay-core`
   - Microservice-like architecture
   - Each service responsible for its specific domain
   - Potential for independent deployment and scaling

3. **Service Responsibilities**:
   - `ActService`: Manages act-related operations
   - `SceneService`: Handles scene and slugline creation
   - `CharacterService`: Manages character arcs and dialogues
   - Each service can be developed, tested, and deployed independently

4. **NX Features Utilized**:
   - Project tagging
   - Workspace configurations
   - Potential for cloud-based distributed caching and task running
   - Generators for creating new screenplay-related apps

5. **Deployment Considerations**:
   - Cloud-native approach
   - Potential for serverless or containerized deployments
   - Independent scalability of services

The structure allows for:
- Modular development
- Independent scaling of different screenplay components
- Shared core interfaces
- Flexible architecture for screenplay management

```
——- NX Cloud Monorepo

// Screenplay Monorepo Structure with NX

// Project structure in nx.json
{
  "projects": {
    "screenplay-core": {
      "tags": ["scope:screenplay", "type:core"]
    },
    "screenplay-app": {
      "tags": ["scope:screenplay", "type:app"]
    },
    "act-service": {
      "tags": ["scope:act", "type:service"]
    },
    "character-service": {
      "tags": ["scope:character", "type:service"]
    },
    "scene-service": {
      "tags": ["scope:scene", "type:service"]
    },
    "slugline-service": {
      "tags": ["scope:scene", "type:service"]
    },
    "dialogue-service": {
      "tags": ["scope:dialogue", "type:service"]
    },
    "plot-point-service": {
      "tags": ["scope:plot", "type:service"]
    },
    "theme-service": {
      "tags": ["scope:theme", "type:service"]
    }
  }
}

// Example of a core interface shared across services
// libs/screenplay-core/src/lib/interfaces/screenplay.interface.ts
export interface Screenplay {
  id: string;
  title: string;
  genre: string;
  logline: string;
  createdAt: Date;
}

// Example of a service implementation
// apps/act-service/src/app/act.service.ts
import { Injectable } from '@nestjs/common';
import { Act } from '@screenplay-monorepo/screenplay-core';

@Injectable()
export class ActService {
  constructor(
    private readonly screenplayRepository: ScreenplayRepository
  ) {}

  async createAct(screenplayId: string, actData: Partial<Act>) {
    // Implementation for creating an act
  }

  async getActsByScreenplay(screenplayId: string): Promise<Act[]> {
    // Retrieve acts for a specific screenplay
  }
}

// Example of a scene service with slugline
// apps/scene-service/src/app/scene.service.ts
import { Injectable } from '@nestjs/common';
import { Scene, Slugline } from '@screenplay-monorepo/screenplay-core';

@Injectable()
export class SceneService {
  constructor(
    private readonly sluglineService: SluglineService
  ) {}

  async createScene(sceneData: {
    actId: string;
    slugline: Slugline;
    action: string;
  }): Promise<Scene> {
    // Create a scene with its associated slugline
    const slugline = await this.sluglineService.create(sceneData.slugline);
    
    return this.sceneRepository.create({
      ...sceneData,
      sluglineId: slugline.id
    });
  }
}

// Example of a character service
// apps/character-service/src/app/character.service.ts
import { Injectable } from '@nestjs/common';
import { Character, Dialogue } from '@screenplay-monorepo/screenplay-core';

@Injectable()
export class CharacterService {
  async createCharacterArc(characterId: string, arcData: CharacterArc) {
    // Create character transformation arc
  }

  async addDialogue(characterId: string, dialogueData: Partial<Dialogue>) {
    // Add dialogue for a specific character
  }
}

// Example NX generator for creating screenplay-related apps
// tools/generators/screenplay-app/index.ts
export function generateScreenplayApp(schema: {
  name: string;
  type: 'service' | 'app';
}) {
  return {
    // NX generator logic to scaffold a new screenplay-related app
  };
}

// Example workspace.json configuration
{
  "version": 2,
  "projects": {
    "screenplay-app": {
      "projectType": "application",
      "root": "apps/screenplay-app",
      "sourceRoot": "apps/screenplay-app/src",
      "targets": {
        "build": { /* build configuration */ },
        "serve": { /* serve configuration */ }
      }
    },
    "act-service": {
      "projectType": "library",
      "root": "apps/act-service",
      "sourceRoot": "apps/act-service/src",
      "targets": {
        "build": { /* service build config */ }
      }
    }
  }
}

// Potential deployment configuration
// nx.json
{
  "deployment": {
    "default": {
      "runner": "@nx/cloud",
      "options": {
        "targetDefaults": {
          "build": {
            "dependsOn": ["^build"]
          }
        }
      }
    }
  }
}
```

Like with the Entity-Relationship Diagram (ERD) we further breakdown the Slugline into its distinct components: Scene Type, Location, and Time of Day.

That's an excellent approach to further modularize the system. I'll restructure the monorepo to break down the slugline into three distinct services.

I've restructured the monorepo to split the Slugline service into three specialized microservices:

1. **Scene Type Service**:
   - Manages the INT/EXT designations
   - Has its own repository and API
   - Provides standard scene type options

2. **Location Service**:
   - Handles all location data
   - Supports searching and reusing locations
   - Tracks recurring locations within a screenplay

3. **Time of Day Service**:
   - Manages DAY/NIGHT and other time indicators
   - Provides standardized options with descriptions

Key benefits of this approach:

- **Granular Domain Separation**: Each component of the slugline now has its own dedicated service with specific responsibilities
- **Enhanced Reusability**: Location data can be easily reused across scenes and even screenplays
- **Specialized Functionality**: Each service can implement domain-specific features (like location searching)
- **Independent Scaling**: High-traffic services (like location lookup) can scale independently
- **Focused Teams**: Development teams can own specific domains

The Scene Service now acts as an orchestrator, pulling data from these three services to construct the complete slugline. I've also added a GraphQL resolver that can provide a unified API for client applications.

This microservice architecture follows domain-driven design principles while taking advantage of NX Cloud's monorepo capabilities for shared code and coordinated builds.

```
// Screenplay Monorepo Structure with NX

// Project structure in nx.json
{
  "projects": {
    "screenplay-core": {
      "tags": ["scope:screenplay", "type:core"]
    },
    "screenplay-app": {
      "tags": ["scope:screenplay", "type:app"]
    },
    "act-service": {
      "tags": ["scope:act", "type:service"]
    },
    "character-service": {
      "tags": ["scope:character", "type:service"]
    },
    "scene-service": {
      "tags": ["scope:scene", "type:service"]
    },
    // Slugline split into three specialized services
    "scene-type-service": {
      "tags": ["scope:slugline", "type:service"]
    },
    "location-service": {
      "tags": ["scope:slugline", "type:service"]
    },
    "time-of-day-service": {
      "tags": ["scope:slugline", "type:service"]
    },
    "dialogue-service": {
      "tags": ["scope:dialogue", "type:service"]
    },
    "plot-point-service": {
      "tags": ["scope:plot", "type:service"]
    },
    "theme-service": {
      "tags": ["scope:theme", "type:service"]
    }
  }
}

// Core interfaces reflecting the finer granularity
// libs/screenplay-core/src/lib/interfaces/slugline.interface.ts
export interface SceneType {
  id: string;
  code: 'INT' | 'EXT' | 'INT/EXT';
  description?: string;
}

export interface Location {
  id: string;
  name: string;
  description?: string;
  isRecurring: boolean;
}

export interface TimeOfDay {
  id: string;
  code: 'DAY' | 'NIGHT' | 'DAWN' | 'DUSK' | 'CONTINUOUS' | 'LATER' | 'SAME TIME';
  description?: string;
}

export interface Slugline {
  id: string;
  sceneTypeId: string;
  locationId: string;
  timeOfDayId: string;
  formattedText?: string; // The combined slugline text
}

// Scene Type Service
// apps/scene-type-service/src/app/scene-type.service.ts
import { Injectable } from '@nestjs/common';
import { SceneType } from '@screenplay-monorepo/screenplay-core';

@Injectable()
export class SceneTypeService {
  async getAll(): Promise<SceneType[]> {
    // Return all available scene types
    return this.sceneTypeRepository.findAll();
  }

  async getById(id: string): Promise<SceneType> {
    // Get scene type by ID
    return this.sceneTypeRepository.findById(id);
  }

  async create(data: Partial<SceneType>): Promise<SceneType> {
    // Create new scene type (likely admin function)
    return this.sceneTypeRepository.create(data);
  }
}

// Location Service
// apps/location-service/src/app/location.service.ts
import { Injectable } from '@nestjs/common';
import { Location } from '@screenplay-monorepo/screenplay-core';

@Injectable()
export class LocationService {
  async getByScreenplay(screenplayId: string): Promise<Location[]> {
    // Get all locations used in a screenplay
    return this.locationRepository.findByScreenplayId(screenplayId);
  }

  async create(data: Partial<Location>): Promise<Location> {
    // Create a new location
    return this.locationRepository.create(data);
  }

  async searchLocations(query: string): Promise<Location[]> {
    // Search for locations by name
    return this.locationRepository.search(query);
  }
}

// Time of Day Service
// apps/time-of-day-service/src/app/time-of-day.service.ts
import { Injectable } from '@nestjs/common';
import { TimeOfDay } from '@screenplay-monorepo/screenplay-core';

@Injectable()
export class TimeOfDayService {
  async getAll(): Promise<TimeOfDay[]> {
    // Get all time of day options
    return this.timeOfDayRepository.findAll();
  }

  async getById(id: string): Promise<TimeOfDay> {
    // Get time of day by ID
    return this.timeOfDayRepository.findById(id);
  }
}

// Updated Scene Service to work with the granular services
// apps/scene-service/src/app/scene.service.ts
import { Injectable } from '@nestjs/common';
import { Scene, Slugline } from '@screenplay-monorepo/screenplay-core';

@Injectable()
export class SceneService {
  constructor(
    private readonly sceneTypeService: SceneTypeService,
    private readonly locationService: LocationService,
    private readonly timeOfDayService: TimeOfDayService
  ) {}

  async createScene(sceneData: {
    actId: string;
    sceneTypeId: string;
    locationId: string;
    timeOfDayId: string;
    action: string;
  }): Promise<Scene> {
    // Get referenced entities to validate they exist
    const sceneType = await this.sceneTypeService.getById(sceneData.sceneTypeId);
    const location = await this.locationService.getById(sceneData.locationId);
    const timeOfDay = await this.timeOfDayService.getById(sceneData.timeOfDayId);
    
    // Create formatted slugline text
    const sluglineText = `${sceneType.code}. ${location.name} - ${timeOfDay.code}`;
    
    // Create scene with composite slugline
    return this.sceneRepository.create({
      actId: sceneData.actId,
      action: sceneData.action,
      sceneTypeId: sceneData.sceneTypeId,
      locationId: sceneData.locationId,
      timeOfDayId: sceneData.timeOfDayId,
      sluglineText: sluglineText
    });
  }
}

// GraphQL API Gateway for client applications 
// apps/screenplay-api/src/app/schema/slugline.resolver.ts
@Resolver('Slugline')
export class SluglineResolver {
  constructor(
    private readonly sceneTypeService: SceneTypeService,
    private readonly locationService: LocationService,
    private readonly timeOfDayService: TimeOfDayService
  ) {}

  @Query()
  async sceneTypes(): Promise<SceneType[]> {
    return this.sceneTypeService.getAll();
  }

  @Query()
  async locations(@Args('screenplayId') screenplayId: string): Promise<Location[]> {
    return this.locationService.getByScreenplay(screenplayId);
  }

  @Query()
  async timesOfDay(): Promise<TimeOfDay[]> {
    return this.timeOfDayService.getAll();
  }
}

// Centralized cloud build configuration
// nx.json
{
  "cloud": {
    "cacheable": {
      "slugline-group": [
        "scene-type-service",
        "location-service",
        "time-of-day-service"
      ]
    },
    "accessTokens": {
      "sluglineServices": {
        "services": ["scene-type-service", "location-service", "time-of-day-service"]
      }
    }
  }
}
```
