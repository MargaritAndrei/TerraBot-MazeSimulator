TerraBot – Autonomous Planetary Ecosystem Simulator

TerraBot is an object-oriented planetary exploration and ecosystem simulation engine implemented in Java. The project models an autonomous NASA rover deployed on an alien world to analyze environmental conditions, monitor ecological evolution, and apply targeted interventions to terraform the planet for human colonization.

Core Simulation Architecture & Lifecycle
Simulation Control: Each run is bounded by startSimulation and endSimulation. State (robot battery, knowledge base, inventory, map entities) resets between independent simulation runs.

Turn Execution Order (per timestamp iteration):
Environmental & Ecological Updates: Natural evolution processes occur first (entity interactions, growth, feeding, and air/soil quality updates).

Robot Actions: Execution of the dispatched command (moveRobot, scanObject, learnFact, improveEnvironment, etc.).
Entities & Environmental Modeling

The grid-based map contains multiple interacting entities:
Air: Modeled across multiple types (Tropical, Polar, Temperate, Desert, Mountain). Computes air quality and toxicity based on atmospheric parameters (O₂, CO₂, humidity, temperature, dust/pollen). Supports dynamic weather events (e.g., storms, rainfall, seasonal shifts).
Soil: Subtypes (Forest, Swamp, Desert, Grassland, Tundra) with calculated quality scores based on nitrogen, organic matter, and moisture retention.
Water: Physical sources with metrics for salinity, pH, purity, and freezing state, providing environmental hydration and animal sustenance.
Plants: 5 botanical classifications (Flowering, Gymnosperms, Ferns, Mosses, Algae). Produce O₂ as they progress through growth stages (young → mature → old → dead).
Animals: Categorized by diet (Herbivores, Carnivores, Omnivores, Detritivores, Parasites). Move autonomously every 2 iterations to hunt/forage, digest food into soil organic matter, and react to toxic atmospheric conditions.
Entity Discovery Gate: Plants, animals, and water sources must be scanned by TerraBot before they can actively participate in automatic ecosystem interactions.

The classes are made using classic inheritance.

Entity: Base class defining common attributes (name, mass) and contains the static method Entity.round() that replaces the round method specified in the statement.

Abstract Classes (Animal, Soil, Air, Plant): These classes define the respective entity, but not a specific type of that entity like Carnivores or Algae.

Cell: Contains the five unique entity types. It is responsible for most of the interactions between the entities.

Entity Subclasses (FloweringPlants, Herbivores, ForestSoil, TropicalAir, etc): These are the specific implementations. They have the common logic (e.g., Plant.grow(), Animal.move()) and their unique logic (categoryOxygen(), isPredator(), calculateQuality(), etc).

The Simulation constructor creates the simulation from the input. It creates unique instances for the entities and assigns them to the appropriate cells. Command handler methods in Simulation (dispatchCommand) call the specialized entity methods (air.handleWeatherEvent, soil.addSpecificFieldsToJson, etc).

The Main class implements a for-loop that simulates every single timestamp between commands most notably when the robot is charging to ensure the environment is up to date.


The implementation solves some of the tasks that were a bit poorly explained in the statement:

The animal movement algorithm (Animal.move) now integrates feeding/hunting logic. To respect the maximum 1 animal per cell rule, a predator attempts to eat its prey on the target cell before moving there (processMoveInteraction). This prevents two animal being on the same cell and fixes the logic. The feeding algorithm had an issue that made an animal eat itself that was fixed eventually.

The code incorporates logic to manage the lastInteractionTimestamp for water/soil interactions, ensuring that the delays specified in the requirements are correctly applied after TerraBot scans an entity.
