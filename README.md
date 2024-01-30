# screeps-cheat-sheet

### Screeps Cheat Sheet for Beginners

#### Table of Contents

1. [Understanding the Basics](#understanding-the-basics)
2. [Basic Commands](#basic-commands)
3. [Basic Strategy Tips](#basic-strategy-tips)
4. [Code Management](#code-management)
5. [Advanced Concepts](#advanced-concepts)

#### 1. Understanding the Basics

- **Creeps**: Units performing tasks like harvesting, building, or fighting.
- **Spawns**: Produce new creeps.
- **Energy**: Primary resource used for spawning creeps and constructing buildings.
- **Rooms**: Areas you control or can explore.

#### 2. Basic Commands

- **Creating a Creep**

  ```javascript
  Game.spawns["Spawn1"].spawnCreep([WORK, CARRY, MOVE], "Harvester1");
  ```

  _Explanation_: Spawns a creep named 'Harvester1' with work, carry, and move parts at 'Spawn1'.

- **Moving a Creep**

  ```javascript
  Game.creeps["Harvester1"].moveTo(Game.flags.Flag1);
  ```

  _Explanation_: Moves 'Harvester1' to the location of 'Flag1'.

- **Harvesting Energy**

  ```javascript
  var sources = creep.room.find(FIND_SOURCES);
  if (creep.harvest(sources[0]) == ERR_NOT_IN_RANGE) {
    creep.moveTo(sources[0], { visualizePathStyle: { stroke: "#ffaa00" } });
  }
  ```

  _Explanation_: Finds energy sources and harvests from the first one. If not in range, moves towards it.

- **Upgrading a Controller**

  ```javascript
  if (creep.upgradeController(creep.room.controller) == ERR_NOT_IN_RANGE) {
    creep.moveTo(creep.room.controller, { visualizePathStyle: { stroke: "#ffffff" } });
  }
  ```

  _Explanation_: Upgrades the room's controller; moves to it if not in range.

- **Building Structures**

  ```javascript
  var constructionSite = creep.pos.findClosestByPath(FIND_CONSTRUCTION_SITES);
  if (creep.build(constructionSite) == ERR_NOT_IN_RANGE) {
    creep.moveTo(constructionSite, { visualizePathStyle: { stroke: "#ffffff" } });
  }
  ```

  _Explanation_: Finds the closest construction site and builds it; moves to it if not in range.

- **Repairing Structures**

  ```javascript
  var closestDamagedStructure = creep.pos.findClosestByRange(FIND_STRUCTURES, {
    filter: (structure) => structure.hits < structure.hitsMax,
  });
  if (creep.repair(closestDamagedStructure) == ERR_NOT_IN_RANGE) {
    creep.moveTo(closestDamagedStructure);
  }
  ```

  _Explanation_: Finds the closest damaged structure and repairs it; moves to it if not in range.

#### 3. Basic Strategy Tips

- **Balancing Creeps**: Ensure a balance of different creep roles (harvesters, builders, upgraders).
- **Energy Management**: Keep an eye on energy levels and prioritize energy gathering in the early game.
- **Room Defense**: Create creeps with ATTACK or RANGED_ATTACK parts for defense.
- **Expansion**: Explore opportunities to expand into new rooms for more resources.

#### 4. Code Management

- **Organizing Code**: Use modules and separate files for different roles and tasks.
- **Error Checking**: Always check for errors in commands (like `ERR_NOT_IN_RANGE`) and handle them.

#### 5. Advanced Concepts (As You Progress)

- **Automating Creep Production**: Create logic to automatically spawn creeps based on your needs.
- **Memory Usage**: Use the Memory object to store information persistently.
- **CPU Management**: Optimize your code to use less CPU.

---

### Using Modules in Screeps

#### Table of Contents

- [screeps-cheat-sheet](#screeps-cheat-sheet)
  - [Screeps Cheat Sheet for Beginners](#screeps-cheat-sheet-for-beginners)
    - [Table of Contents](#table-of-contents)
    - [1. Understanding the Basics](#1-understanding-the-basics)
    - [2. Basic Commands](#2-basic-commands)
    - [3. Basic Strategy Tips](#3-basic-strategy-tips)
    - [4. Code Management](#4-code-management)
    - [5. Advanced Concepts (As You Progress)](#5-advanced-concepts-as-you-progress)
  - [Using Modules in Screeps](#using-modules-in-screeps)
    - [Table of Contents](#table-of-contents-1)
    - [1. Understanding Modules](#1-understanding-modules)
    - [2. Basic Module Creation and Usage](#2-basic-module-creation-and-usage)
    - [3. Example: Creating a Builder Module](#3-example-creating-a-builder-module)
    - [4. Tips for Effective Module Usage](#4-tips-for-effective-module-usage)
    - [5. Advanced Module Concepts](#5-advanced-module-concepts)
    - [6. Debugging Modules](#6-debugging-modules)
    - [7. Maintaining Your Codebase](#7-maintaining-your-codebase)

#### 1. Understanding Modules

- **Purpose**: Modules help organize code by functionality, making it easier to maintain and debug.
- **Structure**: Each module is a separate JavaScript file that exports functions, objects, or values.
- **Usage**: You can require a module in other parts of your code and use its exported elements.

#### 2. Basic Module Creation and Usage

- **Creating a Module (Harvester)**

  - **File**: `harvester.js`

  ```javascript
  var harvester = {
    run: function (creep) {
      // Harvester code here
    },
  };
  module.exports = harvester;
  ```

  _Explanation_: Creates a module for a harvester creep with a `run` function.

- **Using a Module in Main**

  - **File**: `main.js`

  ```javascript
  var harvester = require("harvester");

  for (var name in Game.creeps) {
    var creep = Game.creeps[name];
    if (creep.memory.role == "harvester") {
      harvester.run(creep);
    }
  }
  ```

  _Explanation_: Requires the `harvester` module in the main script and applies it to creeps with the role of 'harvester'.

#### 3. Example: Creating a Builder Module

- **Builder Module**

  - **File**: `builder.js`

  ```javascript
  var builder = {
    run: function (creep) {
      // Builder code here
    },
  };
  module.exports = builder;
  ```

  _Explanation_: A dedicated module for builder creeps.

- **Usage in Main**

  ```javascript
  var builder = require("builder");

  // ... in your main loop
  if (creep.memory.role == "builder") {
    builder.run(creep);
  }
  ```

#### 4. Tips for Effective Module Usage

- **Single Responsibility**: Each module should have a single responsibility.
- **Naming Conventions**: Use clear and descriptive names for modules.
- **Shared Code**: Place common functions in a utility module.
- **Memory Management**: Use modules to organize Memory object usage.

#### 5. Advanced Module Concepts

- **Inter-Module Communication**: Modules can import each other as needed. Avoid circular dependencies.
- **Dynamic Module Loading**: Load modules dynamically based on game conditions.

#### 6. Debugging Modules

- **Individual Testing**: Test each module separately.
- **Console Logging**: Use `console.log` for debugging.
- **Typos and Errors**: Check for correct module names and exports.

#### 7. Maintaining Your Codebase

- **Refactoring**: Regularly update and improve modules.
- **Adapting to New Features**: Modify modules as new game features become available.

Modules in Screeps are crucial for managing complex AI behaviors and ensuring that your code is reusable and understandable. The key to effective module use is organization and clarity.
