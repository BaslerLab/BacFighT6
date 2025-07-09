# BacFighT6: Simulation of T6SS-mediated Bacterial Interactions

**Version:** 4.3

**Developed by:** Marek Basler (University of Basel, Biozentrum) with assistance from Gemini 2.5 Pro.

**Inspired by:**
* Pukatzki et al. 2006 (<https://doi.org/10.1073/pnas.0510322103>)
* Russell et al. 2011 (<https://doi.org/10.1038/nature10244>)
* Basler et al. 2012 (<https://doi.org/10.1038/nature10846>)
* Alteri et al. 2013 (<https://doi.org/10.1371/journal.ppat.1003608>)
* Basler et al. 2013 (<https://doi.org/10.1016/j.cell.2013.01.042>)
* Borgeaud et al. 2015 (<https://doi.org/10.1126/science.1260064>)
* Borenstein et al. 2015 (<https://doi.org/10.1371/journal.pcbi.1004520>)
* Ringel et al. 2017 (<https://doi.org/10.1016/j.celrep.2017.12.020>)
* Smith and Vettiger et al. 2020 (<https://doi.org/10.1371/journal.pbio.3000720>)
* Smith et al. 2020 (<https://doi.org/10.1038/s41467-020-19017-z>)
* Lin et al. 2022 (<https://doi.org/10.15252/embj.2021108595>)
* Mashruwala et al. 2022 (<https://doi.org/10.1016/j.cell.2022.09.003>)
* Flaugnatti et al. 2025 (<https://doi.org/10.7554/eLife.101032>)
* Brüderlin et al. 2025 (<https://doi.org/10.1126/sciadv.adr1713>)
* Stubbusch et al. 2025 (<https://doi.org/10.1126/science.adr8286>)

**Reviews on various aspects of the Type VI Secretion System:**
* Wang et al. 2019 (<https://doi.org/10.1146/annurev-micro-020518-115420>)
* Hespanhol et al. 2023 (<https://doi.org/10.1099/mic.0.001376>)

## 1. Overview

This simulation models the dynamic interactions between up to three distinct bacterial cell types—**Predators**, **Prey**, and **Defenders**—within a shared environment. The primary mechanism of interaction is the Type VI Secretion System (T6SS), a sophisticated molecular weapon that bacteria use to inject toxins into neighboring cells upon direct contact.

Cells inhabit a two-dimensional hexagonal grid and compete for space. The simulation progresses in discrete time steps, where each step typically represents one minute of real time. During each step, cells can undergo several actions:
* **Replication:** Cells can divide to produce offspring if space is available and their internal replication cooldown has completed.
* **Replication Reward (Lysis-for-Replication):** Predator and Defender cells can be rewarded for lytic activity, earning an accelerated or entirely new replication cycle for successfully lysing a set number of targets. This creates a powerful fitness feedback loop where combat success directly drives proliferation.
* **T6SS Firing:** Predator and Defender cells can fire their T6SS, targeting adjacent cells. Predator firing is regulated by a cell density-dependent Quorum Sensing system.
* **Toxin Effects:** Cells hit by T6SS toxins accumulate damage. This can lead to cell death (growth arrest) or lysis (cell bursting), depending on the type and amount of toxins received.
* **Retaliation:** Defender cells can retaliate specifically against attackers.
* **Cell Movement:** Cells can move to adjacent empty hexes based on configurable cooldowns, probabilities, and directional preferences.
* **Predator Chemotaxis:** Predator cells can exhibit chemotactic behavior, preferentially moving towards higher concentrations of autoinducers released by Prey cells.
* **Prey Capsule Formation:** Prey cells can develop protective capsule layers in response to their own local cell density (via Prey autoinducer signals), offering a chance to resist T6SS attacks.
* **Reporter System:** Lysis of Prey cells releases a reporter enzyme (LacZ), which converts a substrate (CPRG) in the environment. This conversion is visualized by a change in the arena's background color, providing a proxy for overall Prey lysis.

The simulation allows users to configure a vast array of parameters, offering a flexible platform to explore complex ecological dynamics, the evolution of T6SS strategies, and the outcomes of bacterial warfare under different conditions. It is inspired by numerous experimental findings and theoretical models in microbial ecology and T6SS research.

## 2. Getting Started & User Interface

The simulation interface consists of four main parts:
1.  **Control Panel (Left):** Contains all settings, setup options, and simulation controls. This panel now includes the **Time-Travel Control** section for history navigation.
2.  **Simulation Arena (Center):** The hexagonal grid where bacterial interactions are visualized.
3.  **Simulation Statistics (Right):** Located on the right side of the arena, above the Hex Inspector, this panel displays real-time statistics. Cell-type specific data (live, dead/lysing, kills, lyses) are shown in a table format, while general simulation metrics (time, total cells, firings, CPRG conversion) are listed below it.
4.  **Hex Inspector (Right):** A panel that displays detailed real-time information about the hexagonal cell your mouse is currently hovering over.

### Basic Workflow:
1.  **Configure Arena & Initial Population:**
	* The simulation starts with an empty arena in **manual placement mode** by default.
    * **Select a tool** from the "Setup Arena" panel (e.g., 'Pred', 'Prey', 'Def', 'Barr', or 'Rem'). The active tool is highlighted with a colored ring.
    * **Click and drag** on the arena to place or remove cells.
    * For quick setups, you can use:
        * `Place Cells Randomly`: Fill the arena randomly based on the "Initial Count" specified for each type under "Cell-Specific Settings". The positions are defined by seed.
        * `Clear All Placed`: Empties the arena.
        * `Import Arena (TSV)`: Load a predefined spatial arrangement from a TSV file.
        * `Export Arena (TSV)`: Save your current manual layout.
    * The simulation is ready to run as soon as you have placed cells.
2.  **Adjust Parameters:**
    * Navigate through the control panel sections: "Simulation Control," "General Settings," "Exports," "Cell-Specific Settings" (for Predator, Prey, and Defender), and "CPRG Reporter Settings."
    * Modify parameters as needed for your experiment. You can also "Load Preset Scenario" for pre-configured setups.
3.  **Run the Simulation:**
    * Use the main control buttons. These can also be activated via keyboard shortcuts:
        * `Start [S]`: Begins or resumes the simulation.
        * `Pause [P]`: Temporarily halts the simulation.
        * `One Step [O]`: Advances the simulation by a single time step (one minute).
        * `Stop & Report [R]`: Ends the simulation run and displays the results report.
        * When paused, the **Left/Right Arrow Keys** can be used to scrub through the simulation's history.
	Note: For better performance on large arenas and long runs, increase the **Render Rate** in the 'Simulation Control' panel to draw the arena less frequently.
4.  **Analyze Results:**
    * Upon stopping, a **Report Modal** appears, summarizing the outcome and key statistics.
    * From the report, you can:
        * `View Graph`: Visualize population dynamics and CPRG conversion over time.
		* **`Save All Data`**: This is the main button to save archives of all currently buffered data (Images as ZIP, Arena States as ZIP, Full Session History as .bft6) and a TSV file with the data for the population dynamics graph.
        * `Load Step to Arena`: If Arena Layouts were saved, you can initialize the arena from a specific past simulation step.

## 3. The Simulation Arena

The arena is a hexagonal grid. Each hexagon represents a potential space a single bacterium can occupy.
* **Coordinates:** Cells are located using axial coordinates (`q`, `r`).
* **Arena Radius:** Set in the "Setup Arena" panel. This determines the size of the grid. This can be changed at any time **while the simulation is paused**. Changing the radius is non-destructive; it will not remove any cells you have placed, but cells outside the new radius will not be visible or interactive.
* **Cell Placement:** Cells are placed either randomly or manually. Replication results in new cells appearing in adjacent empty hexes. Additionally, cells can actively move to adjacent hexes based on defined movement parameters (see Section 6.1).

## 4. Visualizations

The simulation provides real-time visual feedback:
* **Predator Cells:** Rendered as **red** hexagons.
* **Prey Cells:** Rendered as **blue** hexagons.
* **Defender Cells:** Rendered as **yellow-orange** hexagons.
* **Barrier Cells:** Rendered as **dark brown** hexagons. These are static, impassable obstacles.
* **Cell States:**
    * **Live Cells:** Displayed in their solid type-specific color.
    * **Dead Cells:** Appear as **dark gray** hexagons. They also feature a smaller, faint inner dot matching their original cell type color (e.g., a faint red dot for a dead predator). Dead cells are metabolically inactive and cannot replicate or fire, but they continue to occupy space until they lyse.
    * **Lysing Cells:** Appear as **light gray** hexagons, also with a faint inner dot of their original color. These cells have accumulated enough lytic toxins to burst and are on a timer to be removed from the grid.
	* **Prey Capsule Indication:** A Prey cell with a capsule is indicated by a **thicker, purple inner outline**. This inner border is drawn just inside the cell's main outline. The thickness and darkness of this inner outline correspond to the number of capsule layers, providing a visual cue of its increased protection.
* **T6SS Firing Events:**
    * A **bright green sector** emanating from an attacking cell indicates a "precise hit" that can deliver toxins.
    * A **darker, semi-transparent green sector** (Predator cells only) indicates a "missed" T6SS firing attempt that resets the cooldown but delivers no toxins.
* **CPRG Reporter (Arena Background Color):** The background color changes from white towards **magenta** as the amount of converted CPRG (released from lysed Prey) increases.
* **Prey AI Field:** A faint, transparent blue overlay on empty hexes visualizes the concentration of the Prey autoinducer signal. The overlay becomes more opaque as the concentration crosses key thresholds: Predator's attraction limit and a fraction of the theoretical maximum AI level. This helps visualize areas attractive to Predators.
* **Empty Spaces:** Unoccupied hexes are rendered with a light gray outline.
* **Hex Inspector:** This panel provides detailed, real-time data for the hex under the mouse cursor, including coordinates, local AI concentrations, cell ID and type, cooldowns, toxin levels, and special states like `QS P(active)` (for Predators) or `Capsule Layers` (for Prey).

## 5. Simulation Controls & Settings Detailed

### 5.1. Setup Arena (Panel)
* **Arena Radius:** An integer defining the radius of the hexagonal grid.
* **Placement Buttons:** (`Pred`, `Prey`, `Def`, `Barr`, `Rem`): Selects the action performed when clicking on a hex in the arena. The active tool is highlighted.
* `Place Cells Randomly`: Clears the arena and populates it with a random spatial distribution of cells based on their "Initial Count" settings.
* `Clear All Placed`: Removes all cells from the arena.
* `Import Arena (TSV)`: Loads an initial cell configuration from a user-provided Tab-Separated Values (TSV) file (`q\tr\ttype`).
* `Export Arena (TSV)`: Saves the current manually placed cell configuration to a TSV file.

### 5.2. Simulation Control (Panel)
This panel features the main simulation parameters as well as advanced controls for reproducibility.
* **Step Delay (ms):** The delay in milliseconds between consecutive rendering steps. Lower values result in faster animation.
* **Render Rate:** Render the arena only every N steps. '1' renders every step. '10' renders on steps 0, 10, 20, etc. Higher numbers improve performance.
* **Duration (min):** The maximum number of simulation steps (minutes) the simulation will run before automatically pausing.
* **Seed:** The input field for the random number generator (RNG) seed. The simulation's outcome is determined by this seed.
* **New Seed Button (🔄):** Generates a new, random seed for starting a completely new experiment.
* **RNG Reset Button (🡄):** This button appears (in green) when the RNG has been used. Clicking it resets the random number sequence to the beginning for the *current* seed, without changing the seed itself. This is essential for precisely replicating starting conditions.
* **Cell Sync Button (🔄):** This button appears (in red) at the top of the "Setup Arena" panel whenever placed cells become de-synchronized from the RNG (e.g., if you change the seed *after* placing cells). Clicking the red `🔄` button will iterate through every cell on the grid and re-calculate all of its initial, randomly-determined properties, bringing the population back into a valid, reproducible state with the current seed.
* **Reset Simulation Button:** This performs a hard reset. It clears the arena, erases all history, and generates a new random seed.

### 5.3. Main Control Buttons
* **Start [S]:** Begins the simulation from step 0 or resumes a paused simulation.
* **Pause [P]:** Halts the simulation at the current step.
* **One Step [O]:** Advances the simulation by exactly one step. Useful for detailed observation.
* **Stop & Report [R]:** Terminates the current simulation run and displays the end-of-simulation report modal.

### 5.4. Time-Travel Control (Panel)
* **Scrub Through History:** This slider becomes active if 'Full State History' is enabled. When the simulation is paused, you can drag the slider (or use Left/Right Arrow Keys) to view the state of the arena at any past time step.
* **Resume Simulation from this State:** When scrubbing to a past point, this button becomes active. It is one of two ways to create a new timeline. Clicking it will discard all future history and immediately **start** the simulation from the selected point. Alternatively, you can simply start modifying the arena (e.g., placing a cell), which will also automatically branch the timeline, after which you can use the main "Start" or "One Step" buttons to proceed.
* **Import Session (.bft6):** Loads a complete, previously saved simulation session, including all settings and the full step-by-step history.
* **Import Step (JSON):** Loads a complete simulation state from a previously exported JSON file. This overwrites all current settings and cell placements to perfectly replicate the conditions of the saved step. After importing, you can either resume the simulation to continue the original timeline, or click "New Seed" to branch off and run a new experiment from that exact starting point.
* **Export Step (JSON):** Exports the complete state of the currently viewed time step (live or historical) into a single, human-readable JSON file. This file contains all settings, cell properties, AI grid concentrations, and the precise RNG state, making it ideal for debugging or for use with the "Import Step" feature.

### 5.5. General Settings (Panel)
* **Load Preset Scenario:** Opens a modal allowing selection from various pre-defined parameter configurations.
* **Import/Export Settings (TSV):** Loads or saves a full set of simulation parameters from a TSV file.

### 5.6. Exports (Panel)
* **Arena Layout:** Checkbox. If enabled, the `q,r,type` of all active cells is recorded at each step. This data can be downloaded as a ZIP of TSV files. This also enables the "Load Step to Arena" feature in the report modal.
* **Full State History:** Checkbox. If enabled, the *entire* simulation state (all cell properties, AI grids, stats) is saved at each step. This is required for the Time-Travel feature and for saving/loading complete sessions (`.bft6` files). ⚠️ **This is a memory-intensive option.**
* **Arena Images:** Checkbox. If enabled, a PNG image of the arena is captured at each step.
    * ⚠️ **Warning:** Saving states or images can consume very significant browser memory for long simulations.
* **Image Size (px):** Defines the width and height of the saved square PNG images.

### 5.7. CPRG Reporter Settings
* **Initial CPRG Substrate:** The total amount of CPRG substrate units available at the beginning of the simulation.
* **LacZ k<sub>cat</sub>:** The catalytic rate constant (turnover number) for LacZ.
* **LacZ K<sub>m</sub>:** The Michaelis constant for LacZ with CPRG.

### 5.8. Cell-Specific Settings (Panel)
This section of the control panel is tabbed for **Predator**, **Prey**, and **Defender** types. Each tab contains parameters unique to that cell's behavior.
* **Initial Count:** The number of cells to place when using `Place Cells Randomly`.
* **Replication (min):** The mean and range for the cell's standard division time. Set the mean to -1 to disable normal replication.
* **Movement Behavior:** Controls for movement cooldown, probability, and directional strategy, including Predator chemotaxis towards Prey autoinducer.
* **T6SS / Defense:** Parameters governing T6SS firing behavior (Predator), retaliation and random firing (Defender), or capsule synthesis (Prey).
* **Sensitivity & Resistance:** Thresholds for toxin-induced death/lysis and chances to resist attacks from other cell types.
* **Replication Reward (Predator & Defender Only):**
    * **`Lyses per Reward`**: The number of successful lyses a cell must cause to earn a replication reward. Set to `0` to disable this feature.
    * **`Reward Repl. CD (min)`**: The cooldown value (or reduction) applied as a reward. A mean of `-1` makes the rewarded replication immediate.

## 6. Cell Types & Detailed Mechanics ("Under the Hood")

Each cell in the simulation is an object with properties tracking its state and behavior.

### 6.1. General Cell Properties & Processes

* **Coordinates & ID:** Each cell has `q` and `r` axial coordinates and a unique `id`.
* **Type:** `predator`, `prey`, or `defender`.
* **Replication:**
    * A cell becomes eligible to replicate when its `replicationCooldown` timer reaches zero. This timer is initialized randomly for new cells and reset after each successful division based on "Mean" and "Range (±)" settings for its type (e.g., `time = mean ± random_value_within_range`).
    * **Replication for any cell type can be disabled by setting its 'Mean' replication time to -1 in the settings.**
    * For replication to occur, an empty, valid hexagonal space must be adjacent to the parent cell.
    * If multiple empty neighbors exist, one is chosen randomly for the daughter cell.
    * Both parent and new daughter cell then get their `replicationCooldown` reset.
    * If no empty space is available when a cell is eligible to divide, its `replicationCooldown` is simply reset (simulating a missed division opportunity due to crowding).
    * _Example:_ A Prey cell has replication settings: Mean=20min, Range=5min. Its next division time might be randomly set to, say, 18 minutes. After 18 simulation steps, if an adjacent hex is empty, it divides. If not, its cooldown resets, and it tries again later.
* **Replication Reward (via Lysis):**
    * To simulate a fitness advantage for successful killers, both Predator and Defender cells can earn a **replication reward** for their lytic activity. This system provides a direct feedback loop where effectiveness in combat translates to faster proliferation.
    * **Triggering Mechanism:** Each Predator and Defender maintains a personal counter for the number of cells it has successfully lysed (`lyses`). A reward is triggered when this counter reaches a multiple of the value set in **`Lyses per Reward`**. For example, if this is set to 3, a cell will receive a reward when its `lyses` count reaches 3, then again at 6, 9, and so on. The system internally tracks "claimed" rewards to ensure that a reward is given only once per threshold crossing. This feature is disabled if `Lyses per Reward` is set to 0.
    * **Reward Action:** When a reward is triggered, the cell's `replicationCooldown` is modified based on the **`Reward Repl. CD (min)`** settings:
        1.  **Accelerating an Existing Cycle:** If the cell is already counting down to a normal replication, the reward value is **subtracted** from this cooldown, accelerating the division.
        2.  **Initiating a New Cycle:** If the cell is not currently in a replication cycle (e.g., because normal replication is disabled), this reward **initiates a new, fast replication cycle**, setting the `replicationCooldown` to the reward value.
    * If the `Reward Repl. CD (min)` mean value is set to **-1**, the reward is immediate, setting the `replicationCooldown` to 0.
    * _Example:_ A Predator has its normal replication disabled (`Mean = -1`). Its reward settings are `Lyses per Reward = 2` and `Reward Repl. CD = 10 ± 2 min`. The Predator lyses its first target; its `lyses` count is now 1. Later, it lyses a second target, bringing its count to 2. This meets the threshold and triggers a reward. Since its `replicationCooldown` was `Infinity`, a new cycle is initiated with a cooldown between 8 and 12 minutes.
* **Toxin Accumulation & Cell Fate:**
    * Cells maintain counters for `accumulatedNonLyticToxins` and `accumulatedLyticToxins` from each attacker type (Predator or Defender).
    * When a cell is hit by toxins:
        1.  **Prey Capsule Resistance (Prey Only):** If the capsule system is enabled and the target is a Prey cell with active capsule layers, there's a chance the attack is rendered harmless. The maximum possible protection (e.g., 100%, 80%, etc.) is user-definable via the **'Max Protection (%)'** setting. Each of the 5 possible layers provides 1/5th of this user-defined maximum. If the attack is negated by the capsule, subsequent resistance and toxin accumulation steps are skipped for that hit.
        2.  **Innate Resistance Check (Prey & Defenders):** If the attack was not negated by a capsule, Prey and Defenders then have a chance to resist NL and L toxins independently (based on `% Resistance` settings against the specific attacker type). If resisted, no toxins of that type are added from that hit component. Predators do not have a configurable `% Resistance` setting against Defender attacks in the current model; they rely solely on their fixed sensitivity thresholds to determine the effects of accumulated toxins.
        3.  **Toxin Accumulation:** If not resisted by a capsule or innate resistance, the delivered toxin units are added to the cell's accumulators.
        4.  **State Update (`updateStateBasedOnToxins`):**
            * **Non-Lytic Death:** If `accumulatedNonLyticToxins` ≥ `NL Die Thresh.`, the cell becomes `isDead = true`. It stops replicating and firing but remains on the grid.
            * **Lytic Lysis:** If `accumulatedLyticToxins` ≥ `L Lyse Thresh.`, the cell becomes `isDead = true` and `isLysing = true`. A `lysisTimer` is initiated. The duration of this timer is calculated as `ceil(baseLysisDelay / effectiveLyticUnits)`, where `effectiveLyticUnits` is the actual accumulated lytic toxin (ensuring more toxin leads to faster lysis).
            * When `lysisTimer` reaches 0, or if lysis is immediate (e.g., delay calculates to 0), the cell becomes `isEffectivelyGone`. At this point, if it's a Prey cell, it releases its `lacZPerPrey` units. `isEffectivelyGone` cells are removed from active simulation and counts in the next step's cleanup phase.
    * _Example:_ A Prey cell (L Lyse Thresh: 5, Base Lysis Delay: 20min) is hit by a Predator delivering 3 Lytic units. Prey accumulates 3 L units. Later, another hit delivers 3 L units. Total L units = 6. Since 6 ≥ 5, Prey becomes `isLysing`. `lysisTimer` starts, e.g., `ceil(20 / 6) = 4` minutes. After 4 steps, it becomes `isEffectivelyGone`.

#### Cell Movement
For each of the three cell types (Predator, Prey, Defender), you can define their movement behavior using the following parameters, found in their respective "Cell-Specific Settings" sections under "Movement Behavior":
* **Movement Cooldown Min (min)** and **Movement Cooldown Max (min):**
    * This defines the time range (in simulation minutes/steps) a cell waits before it *can* attempt to move again.
    * When a cell needs a new movement cooldown (e.g., after an attempted move, or after deciding not to move due to probability), a random duration is chosen from this range (inclusive).
    * _Example:_ If Min is 5 and Max is 10, the cell will wait between 5 and 10 minutes before its next movement attempt becomes possible.
* **Movement Probability (%):**
    * When a cell's movement cooldown reaches zero, this parameter (0-100%) determines the chance that the cell will *actually attempt* to make a move in the current simulation step.
    * If set to **0%**, the cell is non-motile and will never attempt to move, even if its cooldown is zero.
    * If set to **100%**, the cell will always attempt to move as soon as its cooldown is zero.
    * If a cell's cooldown is zero but it does not attempt to move due to this probability (e.g., a 30% probability means a 70% chance of *not* attempting the move right now), its movement cooldown will still be reset for the next attempt.
* **Movement Directionality (%):**
    * This parameter (0-100%) influences how a cell chooses its target square if it decides to attempt a move (i.e., its cooldown is zero AND it passed the "Movement Probability" check).
    * **100% Directionality:** The cell will *only* look for adjacent empty squares. If it finds one or more, it will randomly pick one to move into. If there are no empty adjacent squares, the cell will not move in this attempt (but its cooldown will be reset).
    * **0% Directionality:** The cell will pick one of its 6 adjacent squares completely at random, without regard for whether it's empty or occupied. However, the cell will *only successfully move* if this randomly chosen square is actually empty. If it picks an occupied square (another cell or a barrier), the move is cancelled for this attempt (and its cooldown is reset).
    * **Between 0% and 100% (e.g., 75%):** The cell has a `Directionality %` chance of using the "100% Directionality" strategy (look for empty spots only). If it doesn't use that strategy (i.e., with `100 - Directionality %` chance), it will use the "0% Directionality" strategy (pick a random direction, then check if it's empty).

**General Movement Rules:**
* Cells attempt to move one hexagonal space at a time.
* Cells cannot move into squares already occupied by other cells or by barriers.
* If multiple cells attempt to move to the exact same empty square in the same simulation step, only one cell (chosen randomly from the contenders) will successfully occupy the square. The other cells' moves to that specific square will fail for that step.
* Regardless of whether a move attempt is successful, fails due to an occupied target, or fails due to conflict with another moving cell, the cell's movement cooldown is reset.
* In each simulation step, movement calculations occur after other actions like T6SS firing and cell replication. The state of the arena *after* movement becomes the state for the beginning of the next simulation minute.

#### State Synchronization & Reproducibility
For a simulation to be perfectly reproducible, two conditions must be met:
1.  The **Seed** must be the same.
2.  The **sequence of random numbers** drawn from that seed must be the same.
A cell's initial random state (e.g., its starting `replicationCooldown`) is set the moment it is created, using the next available number from the RNG sequence. If you place a cell and *then* change the seed or reset the RNG sequence, that cell's state becomes "out of sync"—it was created with a random number that no longer corresponds to the current state of the generator. The simulation tracks this and provides a tool to correct it.
* **The Cell Sync Button (🔄):**
    This button appears (in red) at the top of the "Setup Arena" panel whenever a de-synchronization event occurs. This happens under two specific conditions:
    1.  The **Seed is changed** (manually, via settings import, or with the `🔄` button) while there are already cells on the arena.
    2.  The **RNG sequence is reset** to the beginning (using the `🡄` button) while there are already cells on the arena.
    Clicking the red `🔄` button will iterate through every cell currently on the grid and re-calculate all of its initial, randomly-determined properties. This action brings the entire population of cells back into a valid, reproducible state with the current seed and RNG sequence.

### 6.2. Predator Cells
* **Role:** Actively hunt and attack Prey and Defender cells.
*   **Movement Behavior (AI-Guided):** Predators can use AI signals produced by Prey to guide their movement.
    *   **Prey AI Attraction (%):** If the Predator decides to move into an empty space (based on its 'Movement Directionality' setting), this is the chance (%) it will attempt to pick the empty neighboring hex that has the highest concentration of Prey-derived Autoinducer (Prey AI). A 0% chance means it will pick a random empty neighbor. A 100% chance means it will always try to pick the highest Prey AI spot.
    *   **Prey AI Attraction Threshold:** This is the minimum concentration of Prey AI that must be present in an empty neighboring hex for that hex to be considered a candidate for the 'highest concentration' selection during Prey AI-attracted movement. If no empty neighbors meet this threshold, the Predator will revert to choosing a random empty neighbor (if 'Movement Directionality' allows movement into empty hexes).
*   **Quorum Sensing (QS) Behavior (for T6SS Activation):** These parameters control a cell-density-dependent communication system using the Predator's *own* Autoinducer (AI). Predators produce this AI signal, and when its local concentration is high enough, Predators become "activated" and can fire their T6SS.
    * **AI Prod. Rate (U/min):** The number of AI signal units each live Predator cell produces per minute.
    * **AI Degrad. Rate (%/min):** The percentage of AI signal that degrades or disappears from each hex per minute, simulating signal instability.
    * **AI Diffusion Rate:** A coefficient (0-0.166) that determines how fast AI spreads to adjacent hexes. It is based on the concentration gradient, meaning AI flows from areas of high concentration to low concentration. A higher value means faster diffusion.
    * **Activation Midpoint (K):** The threshold for T6SS activation, functioning as an EC50. If set to a value less than 0 (e.g., -1), QS is always active and T6SS firing is constitutive (not regulated by cell density). If set to 0, T6SS is active as long as *any* AI is present. If set to a value greater than 0, it acts as the midpoint of a Hill function, where T6SS activation is at 50% when the local AI concentration equals K.
    * **Cooperativity (n):** The Hill coefficient for the QS response. This controls the steepness and sensitivity of the switch. A value of 1 gives a gradual, graded response to AI concentration. Higher values (e.g., 4 or more) create a much sharper, more "all-or-nothing" switch.
* **T6SS Firing Logic (`runSimulationStep`):**
    The process for a Predator attempting to fire follows a strict decision tree each simulation cycle:
    1.  **Activation Check:** A Predator can only attempt to fire if it first becomes "active" based on the Quorum Sensing system and its `t6ssFireCooldownTimer` is 0.
    2.  **Strategy Selection (Contact vs. Random):** The simulation first determines the firing strategy based on the **Contact Sensing Bias %**.
        * If the random check passes (e.g., a 70% bias gives a 70% chance to pass), the Predator commits to the "Contact-Biased" strategy.
        * Otherwise, it commits to the "Random" strategy.
    3.  **Preliminary Target Selection:**
        * **If using the "Contact-Biased" strategy:** The Predator scans its adjacent hexes for contacts (any other cell or barrier). If one or more contacts are found, it randomly picks one as its preliminary target. If **no contacts are found**, the firing sequence is **aborted** for this cycle. The Predator will not fire into an empty space.
        * **If using the "Random" strategy:** The Predator picks one of the six possible directions at random and sets that hex as its preliminary target, regardless of whether it is empty or occupied.
    4.  **Kin Exclusion Check:** If a preliminary target was successfully selected in the previous step, the code now checks if that target is another Predator.
        * If it is, the **Kin Exclusion %** determines if the recognition system activates.
        * If the system activates, the **Kin Exclusion Penalty** value dictates the outcome:
            * **Penalty ≥ 0 ("Cancel & Penalize" mode):** The shot is cancelled. If the penalty value is greater than 0, a special cooldown is applied.
            * **Penalty = -1 ("Smart Re-targeting" mode):** The shot is not cancelled. Instead, the Predator attempts to find a new target by running the targeting logic again, but from a pool of neighbors that has been pre-filtered to exclude all kin. This new selection still respects the original Contact Sensing Bias strategy. If no valid non-kin targets can be found, the shot is aborted.
    5.  **Final Firing Event:** If the shot has not been aborted or cancelled, the firing event is visualized and counted. The simulation then checks the **Precision %** to determine if the shot was a "precise hit" (delivers toxins) or a "miss" (no damage).
		* **_Example of the  firing logic:_**
		A Predator cell is ready to fire, with the following setup:
		* **Parameters:** `Contact Sensing Bias` = 80%, `Kin Exclusion` = 90%, `Kin Exclusion Penalty` = -1 ("Smart Targeting" mode).
		* **Environment:** The Predator is adjacent to another Predator (kin), a Barrier (a non-kin contact), and an empty hex.
		In this scenario, the Predator's decision process is as follows: With an 80% chance, it commits to a **"Contact-Biased"** strategy and selects an adjacent contact (e.g., the kin cell). The 90% kin exclusion check then succeeds and triggers **"Smart Re-targeting"** due to the `-1` penalty. A new pool of non-kin targets is created (`[Barrier, empty_hex]`). Because the original strategy was "Contact-Biased", the simulation filters this new pool for contacts, leaving only the `[Barrier]`. The final shot is therefore correctly fired at the Barrier.
		This logic ensures the `Contact Sensing Bias` has priority, preventing the Predator from firing into empty space when its intent is to hit a contact, even after a complex re-targeting event.

* **T6SS Effectors (vs Prey/Defenders):** Parameters define `NL Units/Hit`, `NL Delivery Chance %`, `L Units/Hit`, and `L Delivery Chance %` when attacking Prey or Defenders.
* **Sensitivity (to Defender Attack):** Predators have their own thresholds (`NL Die Thresh.`, `L Lyse Thresh.`, `Base Lysis Delay`) for damage received from Defender attacks. They do not have % resistance values like Prey/Defenders.

### 6.3. Prey Cells
* **Role:** Primary targets for Predators and Defenders. Can replicate.
* **Sensitivity & Resistance:** Have distinct sets of parameters for damage from Predators and damage from Defenders:
    * `NL Die Thresh.`, `L Lyse Thresh.`, `Base Lysis Delay`.
    * `NL Resist. %`, `L Resist. %`: Chance to completely negate incoming NL or L toxins from a specific hit.
* **Quorum Sensing (QS) Behavior:** Prey cells can also produce their own Autoinducer (AI) signal, which can be detected by Predators (if Predator Prey AI Attraction is enabled). This system is simpler than the Predator's QS and does not directly control Prey behavior but contributes to the AI landscape.
    *   **AI Prod. Rate (U/min):** The number of AI signal units each live Prey cell produces per minute.
    *   **AI Degrad. Rate (%/min):** The percentage of Prey AI signal that degrades or disappears from each hex per minute.
    *   **AI Diffusion Rate:** A coefficient (0-0.166) determining how fast Prey AI spreads to adjacent hexes, based on concentration gradients.
* **Capsule Formation:** Prey cells can develop a protective polysaccharide capsule as a defense mechanism.
    * **Master Switch:** The entire feature can be enabled or disabled globally using the **"Enable Capsule Synthesis"** checkbox. By default, this feature is **off**.
    * **Regulation:** When enabled, synthesis is controlled by a derepression switch that responds to local Prey AI concentration. High AI levels relieve a default state of repression, increasing the chance of synthesis.
    * **Derepression Midpoint (K):** This control works identically to the Predator's T6SS activation control:
        * If **K > 0**: This is the standard mode. K acts as the midpoint concentration (EC50) for derepression, and the chance of synthesis increases with AI.
        * If **K = 0**: Synthesis is triggered if **any** AI is present (`AI > 0`).
        * If **K < 0**: Synthesis is **Always ON** (constitutively derepressed), regardless of the AI level.
    * **Protection Effect:** When an attack hits a prey with a capsule, there is a chance the attack is rendered harmless. The maximum protection offered by a full 5-layer capsule is set by the user via the **"Max Protection (%)"** setting. Each layer provides 1/5th of this total protection (e.g., if Max Protection is 80%, each layer gives a 16% chance to block an attack).
* **Reporter System:** Upon lysis, each Prey cell releases `lacZPerPrey` units of LacZ enzyme.

### 6.4. Defender Cells
* **Role:** Can attack Predators and Prey, and importantly, can retaliate against attackers.
* **Retaliation Behavior:**
    * When a Defender is hit by a T6SS attack (from a Predator or another Defender), it "senses" the attack.
    * `Sense Chance %`: The probability that this sensed attack triggers a retaliation state.
    * If retaliation is triggered:
        * The Defender locks onto the original attacker.
        * It fires a burst of shots, with the number of shots chosen randomly between 1 and `Max Retaliations`.
        * One shot from this burst is fired per simulation step (minute) until the burst is complete.
    * _Example:_ Defender X is hit by Predator Y. Defender X's Sense Chance is 60%. If the 60% chance succeeds, Defender X will retaliate against Predator Y. If Max Retaliations is 7, it might fire (e.g.) 4 shots, one per step for the next 4 steps, at Predator Y.
* **Random Firing (`attemptDefenderRandomFire`):**
    * Defenders can also fire proactively if their `t6ssRandomFireCooldownTimer` is 0.
    * `Rand. Fire CD (min-max)`: Cooldown range after a random firing event.
    * `Rand. Fire %`: Chance per step (if not on cooldown) to fire one shot at a randomly chosen adjacent cell (Predator, Prey, or another Defender).
* **T6SS Effectors (vs Predators/Prey):** Parameters define `NL Units/Hit`, `NL Delivery Chance %`, `L Units/Hit`, and `L Delivery Chance %` for their attacks.
* **Sensitivity (to Predator Attack):** Defenders have their own thresholds and resistance values against Predator toxins.
* **Defender vs. Defender Interaction:** Defenders are **immune to toxin damage** from other Defender cells (they don't accumulate NL/L units from them). However, they **do sense these hits** and can initiate retaliation against the attacking Defender based on their `Sense Chance`.

### 6.5. Barriers
* **Role:** Static, impassable obstacles within the arena.
* **Properties:**
    * Barriers are placed manually or via arena import.
    * They **do not replicate**, **do not fire T6SS**, and are **unaffected by toxins** (they do not accumulate damage, die, or lyse).
    * They occupy a hex space, preventing other cells from moving into or replicating into that space.
    * Other cells (Predators, Defenders) can target Barriers with their T6SS, but these firings will have no effect on the Barrier itself. The firing cell will still undergo its usual cooldown.
    * Barriers remain in place for the entire simulation unless explicitly removed during manual setup.


## 7. Simulation Loop / Step Logic (`runSimulationStep`)

Each simulation step (representing one minute) executes the following sequence of events:

1.  **Initialization & Cleanup:**
    * Increment `simulationStepCount`.
    * Clear per-step counters (firings, kills, lyses for this step).
    * Clear `activeFiringsThisStep` map (used for visualization).
    * Remove any cells marked `isEffectivelyGone` from the main `simState.cells` map.
2.  **Cooldowns & Lysis Progression:**
    * Iterate through all active cells and decrement their cooldown timers:
        * `replicationCooldown`
        * `movementCooldown`
        * `t6ssFireCooldownTimer` (Predators)
        * `t6ssRandomFireCooldownTimer` (Defenders)
        * `lysisTimer` (for cells that `isLysing`). If `lysisTimer` reaches 0:
            * Mark the cell as `isEffectivelyGone`.
            * Increment per-step and cumulative lysis counters for its type.
            * If the cell is a Prey, add its `lacZPerPrey` to `totalActiveLacZReleased`.
3.  **Predator Firing Phase:**
    * Iterate through all active Predator cells that are not dead/lysing.
    * A Predator's ability to fire is first gated by **Quorum Sensing**. It calculates its probability of being "active" (`P(active)`) based on local autoinducer concentration.
    * If the Predator passes this activation check *and* its `t6ssFireCooldownTimer` is 0, it attempts to fire (`attemptPredatorT6SSFire`):
        * Determine target hex based on contact sensing bias or random direction.
        * Check for Kin Exclusion if the target is another Predator. If excluded, apply penalty cooldown.
        * If not excluded, determine if the shot is a "precise hit" based on `PredatorPrecisionInput`.
        * Record the firing event in `activeFiringsThisStep` for visualization (precise or miss). Increment `firingsThisStep`.
        * Reset the Predator's `t6ssFireCooldownTimer`.
        * If it was a precise hit and a valid target cell exists in the target hex (and is not effectively gone):
            * The target cell (Prey or Defender) receives toxins (`receiveHit('predator')`). This involves resistance checks and toxin accumulation.
            * The target cell's state is updated based on accumulated toxins (potentially becoming dead or lysing).
            * If the target is a Defender, it records the attacker's key (`sensedAttackFromKey`) for potential retaliation.
4.  **Defender Retaliation Triggering Phase:**
    * Iterate through all active Defender cells that are not dead/lysing.
    * If a Defender `sensedAttackFromKey` (i.e., was hit in the Predator phase or by another Defender in a previous step/phase) and is not already retaliating:
        * Check `defenderSenseChanceInput`. If successful, the Defender enters retaliation mode:
            * Sets `isRetaliating = true`.
            * Sets `retaliationTargetKey` to the key of the attacker.
            * Determines `retaliationsRemainingThisBurst` (randomly, 1 to `defenderMaxRetaliationsInput`).
        * Clears `sensedAttackFromKey`.
5.  **Defender Firing Phase (Retaliation & Random):**
    * Iterate through all active Defender cells that are not dead/lysing.
    * **If Retaliating:**
        * If `retaliationsRemainingThisBurst > 0`, the Defender fires one shot at `retaliationTargetKey` (`attemptRetaliationFire`).
        * Record firing in `activeFiringsThisStep`. Increment `firingsThisStep`.
        * If a valid target cell exists:
            * Target cell (Predator, Prey, or another Defender) receives toxins (`receiveHit('defender')`).
            * If the target is another Defender, it records `sensedAttackFromKey` (but is immune to damage).
        * Decrement `retaliationsRemainingThisBurst`. If it reaches 0, clear `isRetaliating` state.
    * **Else (Not Retaliating or Burst Ended), Attempt Random Fire:**
        * If `t6ssRandomFireCooldownTimer` is 0, check `defenderRandomFireChanceInput`.
        * If successful, the Defender fires one shot at a random adjacent hex (`attemptDefenderRandomFire`).
        * Record firing. Increment `firingsThisStep`. Reset `t6ssRandomFireCooldownTimer`.
        * If a valid target cell exists, it receives toxins as above.
6.  **Replication Phase:**
    * Create a list of all cells eligible for replication (`canReplicate()` is true: not dead/lysing, `replicationCooldown` is 0). This list is shuffled.
    * Iterate through these eligible cells:
        * Find valid empty adjacent hexes.
        * If empty spots exist:
            * Choose one random empty spot.
            * Create a new daughter cell of the same type in that spot. Assign it a new ID and an initial (randomized) `replicationCooldown`. Add to `simState.cells`.
            * Reset the parent cell's `replicationCooldown`.
        * If no empty spots exist, simply reset the parent cell's `replicationCooldown` (missed opportunity).
7.  **Cell Movement Phase:**
    * Iterate through all active, non-barrier cells (Predators, Prey, Defenders) that are not dead/lysing.
    * For each eligible cell:
        * If its `movementCooldown` is 0, check its `Movement Probability %`.
        * If the cell attempts to move (passes probability check):
            * Determine a target adjacent hex based on `Movement Directionality %` (preferring empty hexes if directionality is high, or choosing randomly and then checking if directionality is low).
            * If a valid, empty, and non-barrier target hex is found, the cell is added to a list of pending movements.
        * The cell's `movementCooldown` is reset, regardless of whether a move was attempted or successful in this phase.
    * Process all pending movements:
        * Shuffle pending movements to handle conflicts randomly.
        * For each pending move, if the target hex is still empty (not taken by another cell in this step's movement phase), the cell is moved.
        * Cells cannot move into hexes occupied by other cells (including barriers) or hexes that become occupied by another moving cell in the same step.
    * Update cell positions in the main simulation state (`simState.cells`).
8.  **Quorum Sensing Update Phase:**
    * **Production:** Live Predators add autoinducer (AI) to their current hex's concentration in the `aiGrid`.
    * **Diffusion & Degradation:** A new `aiGrid` is calculated for the next step. For each hex, AI diffuses to/from its neighbors based on concentration gradients and the `AI Diffusion Rate`. The total AI in each hex is then reduced by the `AI Degradation Rate %`.
9.  **Update Statistics & Reporter:**
    * Update cumulative counters for firings, kills, and lyses using the per-step values.
    * **CPRG Conversion:**
        * Calculate `Vmax_current = simState.config.cprg.k_cat * totalActiveLacZReleased`.
        * Calculate `convertedThisStep = (Vmax_current * remainingCPRGSubstrate) / (Km + remainingCPRGSubstrate)`.
        * Ensure `convertedThisStep` does not exceed `remainingCPRGSubstrate`.
        * Add `convertedThisStep` to `totalCPRGConverted` and subtract from `remainingCPRGSubstrate`.
10.  **Record History & Capture Data:**
    * Store current cell counts, cumulative stats, and CPRG converted in `historicalData` array (for graphing).
    * If `saveImagesEnabled` is true, capture an image of the arena.
    * If `saveArenaStatesEnabled` is true, capture the current cell layout as TSV.
11. **Render & Schedule Next Step:**
    * Redraw the simulation grid (`drawGrid()`).
    * Update the statistics display panel (`updateStats()`).
    * If `simState.isRunning` (not paused or just stepping), use `setTimeout(runSimulationStep, simState.config.simulationControl.simulationSpeedMs)` to schedule the next iteration.
    * If `simState.isStepping` was true, set it to false and update button states.
12. **Termination & Batching Checks:**
    * The simulation checks if the time limit has been reached.
    * It also checks if the **Image Buffer Limit** has been reached, pausing to trigger a batch download if necessary.

## 8. CPRG Reporter System Details

The CPRG reporter system simulates a common laboratory assay.
* When Prey cells lyse, they release a fixed amount of LacZ enzyme (`lacZPerPrey`) into the environment. `totalActiveLacZReleased` tracks the cumulative active enzyme.
* This enzyme converts the available `remainingCPRGSubstrate` into a colored product.
* The rate of conversion follows Michaelis-Menten kinetics:
    `Velocity = (Vmax * [S]) / (Km + [S])`
    Where:
    * `[S]` is `remainingCPRGSubstrate`.
    * `Km` is `simState.config.cprg.Km`.
    * `Vmax` (for the current total enzyme) is `simState.config.cprg.k_cat * totalActiveLacZReleased`.
* The amount converted in one step (`convertedThisStep`) is this velocity (since each step is one minute).
* The total CPRG converted (`totalCPRGConverted`) dictates the intensity of the magenta background color of the arena, scaled relative to `initialCPRGSubstrateInput`.

## 9. Preset Scenarios

The "Load Preset Scenario" button provides access to pre-configured parameter sets designed to explore specific phenomena:
* **1. Role of Initial Density (Pred & Prey):** Focuses on how varying initial cell densities and ratios between Predators and Prey affect outcomes. Defenders are typically disabled.
* **2. Prey Sensitivity / Effector Type (Pred & Prey):** Explores how different Prey sensitivities (to lytic vs. non-lytic toxins, or both) influence interactions. Defenders are typically disabled.
* **3. Contact Sensing & Kin Exclusion (Pred & Prey):** Investigates the impact of Predator T6SS firing strategies like biased targeting of adjacent cells and avoiding self-attack. Defenders are typically disabled.
* **4. Tit-for-Tat Dynamics (Pred, Prey, Def):** Sets up scenarios with all three cell types to explore retaliatory strategies, where Defenders respond to Predator attacks, and Prey might have differential susceptibility. Parameters often include varying Defender retaliation accuracy vs. random firing.

Applying a preset will adjust numerous settings across the control panel. You can then further tweak these settings if desired.

## 10. Data Export & Import

The simulation provides robust options for data management:

* **Simulation Data (TSV):** Time-series data of population counts and cumulative statistics.
* **Settings (TSV):** A file containing all simulation parameters, which can be saved and re-imported.
* **Arena Layout (TSV - Manual Mode Only):** Saves or loads a specific spatial arrangement of cells.
* **Per-Step Arena Images (ZIP of PNGs):** A ZIP archive of arena images, one for each step.
* **Per-Step Arena States (ZIP of TSVs):** A ZIP archive of cell layout files, one for each step.
* **Single Step State (JSON):**
    * **Export:** From the `Time-Travel Control` panel, you can export the complete state of any single step into a detailed, human-readable JSON file.
    * **Import:** This file can be re-imported to perfectly reset the simulation to that specific state, including all parameters and the precise state of the random number generator. This allows you to create exact replicas of a simulation state to run new experimental branches.
* **Full Simulation Session (.bft6):**
    * **Export:** If `Full State History` was enabled, the `Save Full Session (.bft6)` button appears in the report modal. This saves the *entire* simulation—all settings and the complete step-by-step history—into a single, highly compressed binary file. This is the definitive way to archive an experiment.
    * **Import:** From the `Time-Travel Control` panel on the main page, click `Import Session (.bft6)` and select a `.bft6` file.
    * **If loading the first file:** This will load the entire saved session, including all settings and the full history.
    * **Additive Loading (Advanced):** If you already have a history loaded, you can import another `.bft6` file to **merge** its history with the existing one. The application will validate that the `Seed` and `Arena Radius` match before merging. This powerful feature allows you to piece together very long simulation runs that were saved in multiple segments. The import logic correctly handles overwriting steps if there is an overlap, and can manage non-contiguous data (e.g., loading steps 0-100 and then 500-600).
	
    ⚠️ **Memory Warning:** Enabling per-step state saving of the simulation and especially **saving large arena images** can heavily tax browser memory for long/large simulations.

#### Advanced Memory Management
Saving per-step data is memory-intensive. To prevent browser crashes during very long runs, the simulation includes buffer limits for images, arena states, and history, which can be configured in the **"Exports & Buffers"** panel. The workflow for all buffer types is the same:
* When a data buffer exceeds its defined size limit, the simulation will automatically **pause**.
* A prompt will appear asking you to select a folder for data saving or to save the current data batch (e.g., a ZIP of images or a `.bft6` history file).
* You must click the **[Save & Continue]** button in the prompt. This action is required to initiate the download.
* Once the download begins, the corresponding buffer is cleared from memory, and the simulation **resumes automatically**.

### 10.1 Configuring Simulation via URL Parameters (Advanced)

You can pre-configure many aspects of the "BacFighT6" simulation by adding parameters directly to the browser's URL (the web address displayed in your browser, e.g., `bacfight6.html`). This is a powerful feature for saving specific setups, sharing them with collaborators, or scripting different starting conditions for experiments.

**How it Works:**

* Parameters are appended to the main page URL, starting with a `?`.
* Each parameter consists of a `name=value` pair.
* Multiple parameters are separated by an `&`.
    * Example: `bacfight6.html?param1=value1&param2=value2`
* **Order Matters:** Parameters are processed strictly from **left to right** as they appear in the URL. This is crucial because later parameters can override settings or modify the arena state established by earlier ones.
    * The simulation page first loads with its default blank arena and settings (due to an initial `initializeBlankSlate()` call).
    * Then, each URL parameter is applied sequentially, potentially altering the simulation state.

**Available Parameter Types:**

1.  **Loading a Full Simulation Session from a URL (Highest Priority):**
    * **Parameter:** `sessionFileURL`
    * **Value:** The full, URL-encoded web address of a `.bft6` session file.
    * **Behavior:** This parameter takes **highest priority**. If the `sessionFileURL` is present in the URL, the simulation will immediately attempt to load the complete session from that file. **All other configuration parameters in the URL will be ignored.** This provides a direct, one-click way to load and view a specific archived experiment.
    * **Example:**
        `...?sessionFileURL=https://yourserver.com/sessions/tit_for_tat_run_01.bft6`
    * **🔴 Important (File Access & CORS):** See "Important General Notes" below.

2.  **Individual Settings Overrides:**
    * You can change most simulation settings found in the control panel.
    * **Parameter Names:** These names **must exactly match** those listed in the first column of a "Settings (TSV)" file that you can export from the simulation's report modal (e.g., `Arena_Radius`, `Predator_Initial_Count`, `Predator_T6SS_Precision_Percent`, `Image_Export_Enabled`).
    * **Behavior:**
        * Changing `Arena_Radius` via a URL parameter is **non-destructive**. It will set the size of the arena boundary but will **not clear any cells** that have been placed by other preceding or subsequent parameters.
        * Other individual settings will update the simulation's configuration parameters. If these settings don't inherently trigger a full arena reset through their own event handlers, the current cell layout will be preserved.
	* **Examples:**
        * Set arena radius to 10 and Predator initial count (for random seeding if no specific arena file/data is given later):
            `...?Arena_Radius=10&Predator_Initial_Count=50`
        * Enable image saving (for checkbox settings, use `true` or `false`):
            `...?Image_Export_Enabled=true`
        * Set a specific T6SS parameter and disable Prey replication:
            `...?Predator_T6SS_Kin_Exclusion_Percent=80&Prey_Replication_Mean_min=-1`
        * Enable the capsule system and set its max protection to 50%:
            `...?Prey_Capsule_System_Enabled=true&Prey_Capsule_Max_Protection_Percent=50`

3.  **Loading All Settings from a File URL:**
    * **Parameter:** `settingsFileURL`
    * **Value:** The full, URL-encoded web address of a settings TSV file. This file must be in the same format as the one exported by the "Download Settings (TSV)" button.
    * **Behavior:** This loads all parameters from the specified file.
        * If the file contains an `Arena_Radius` setting, its value will be applied non-destructively. It will change the arena boundary size but will not clear any existing cells on the grid.
        * Settings from this file can be overridden by individual parameters that appear *after* it in the URL.
    * **Example:**
        `...?settingsFileURL=https://yourserver.com/path/my_experiment_settings.tsv`
    * **🔴 Important (File Access & CORS):** See "Important General Notes" below.

4.  **Loading/Modifying Arena Cell Layout from a File URL (TSV Format):**
    * **Parameter:** `arenaFileURL`
    * **Value:** The full, URL-encoded web address of an arena TSV file (must be `q\tr\ttype` format).
    * **Behavior (Additive/Overwriting):** This parameter **adds cells to the current arena or overwrites cells at the same coordinates.** It does *not* automatically clear the entire arena before loading. Cells are placed onto the grid whose size is determined by the `Arena_Radius` active at the moment this parameter is processed (which itself could have been set by a preceding parameter).
    * **Example:**
        `...?Arena_Radius=12&arenaFileURL=https://yourserver.com/arenas/base_colony.tsv`
    * **🔴 Important (File Access & CORS):** See "Important General Notes" below.

5.  **Loading/Modifying Arena Cell Layout with Compact `cellsData` String:**
    * **Parameter:** `cellsData`
    * **Value:** A continuous string where each cell operation is `q<number>r<number><Type>`. No spaces or other separators.
        * `q<number>`: Axial `q` coordinate (e.g., `q-5`, `q0`, `q12`).
        * `r<number>`: Axial `r` coordinate (e.g., `r3`, `r0`, `r-7`).
        * `<Type>`: A single character:
            * `A`: Place/overwrite with Predator (Attacker).
            * `P`: Place/overwrite with Prey.
            * `D`: Place/overwrite with Defender.
            * `B`: Place/overwrite with Barrier.
            * `E`: Empty/Remove cell at the specified `(q,r)`.
    * **Behavior (Additive/Overwriting/Removing):** This parameter **modifies the current arena state.** It adds new cells, overwrites existing cells if coordinates match, or removes cells if `E` is used. It does *not* clear the entire arena by itself. Operations are validated against the `Arena_Radius` active when this parameter is processed.
    * **Examples:**
        * Place Predator at (0,0) and Prey at (1,-1):
            `...?cellsData=q0r0Aq1r-1P`
        * Place a Defender at (2,0), then remove any cell at (0,0):
            `...?cellsData=q2r0Dq0r0E` *(Result: Defender at (2,0), (0,0) is empty)*

**Understanding Order and Arena State Interactions:**

The sequence of these parameters in your URL dictates the final setup.

* **Arena Sizing (Non-Destructive):**
    * The simulation page initially loads with the default radius from the HTML input.
    * The **`Arena_Radius`** can be set by an individual parameter (e.g., `?Arena_Radius=25`) or from within a loaded `settingsFileURL`.
    * Changing the `Arena_Radius` via URL **does not clear the arena or reset the simulation**. It simply defines the grid size. Cells placed by other URL parameters will be preserved, even if the radius is defined after them in the URL.

* **Cell Placement/Modification (Additive/Overwriting/Removing):**
    * Both **`arenaFileURL`** and **`cellsData`** operate on the arena state *as it exists when they are processed*. They add new cells, or if a cell already exists at a specified coordinate, the new definition from the currently processing parameter will replace it. `cellsData` with type `E` will remove a cell. These two parameters do *not* call `initializeBlankSlate()` themselves.

* **Combined Examples:**
    1.  **Settings File (no radius change), then `cellsData`:**
        `...?settingsFileURL=path/config_no_radius_change.tsv&cellsData=q0r0A`
        * Initial blank state.
        * `config_no_radius_change.tsv` loads. It *does not* change `Arena_Radius`. Other settings are applied. The grid remains empty (or as it was if modified by a preceding parameter not shown here).
        * `cellsData` then places a Predator at `(0,0)` on the current grid.
        * *Outcome:* Grid has `q0r0A`, and simulation parameters are from the file.

    2.  **Arena File for base layout, `cellsData` for specific additions/changes:**
        `...?Arena_Radius=10&arenaFileURL=https://example.com/base_arena.tsv&cellsData=q0r0Aq5r5Pq-1r-1E`
        * Initial blank state.
        * `Arena_Radius=10`: Sets radius to 10. Arena is re-initialized (empty, radius 10).
        * `arenaFileURL`: Cells from `base_arena.tsv` are loaded onto the radius 10 grid. (e.g., file has a Defender `D` at `q0r0`).
        * `cellsData`:
            * The Predator `q0r0A` **replaces** the Defender from the file at `(0,0)`.
            * The Prey `q5r5P` is **added** to an empty spot (if within radius 10 and empty).
            * Any cell at `q-1r-1E` is **removed**.
        * *Outcome:* Arena is primarily from `base_arena.tsv`, but specifically modified by `cellsData`.*

    3.  **`cellsData` first, then `arenaFileURL` (both additive/overwriting):**
        `...?Arena_Radius=10&cellsData=q0r0Pq1r1A&arenaFileURL=https://example.com/main_arena.tsv`
        * Initial blank state.
        * `Arena_Radius=10`: Arena set to radius 10, empty.
        * `cellsData`: Places Prey `P` at `(0,0)` and Predator `A` at `(1,1)`.
        * `arenaFileURL`: Loads cells from `main_arena.tsv`.
            * If `main_arena.tsv` defines a cell at `(0,0)`, it will overwrite the Prey.
            * If `main_arena.tsv` defines a cell at `(1,1)`, it will overwrite the Predator.
            * Other cells from `main_arena.tsv` are added.
            * The cells from `cellsData` remain if their coordinates are not specified in `main_arena.tsv`.
        * *Outcome:* A merged arena. Cells from `main_arena.tsv` take precedence at shared coordinates.

    4.  **Revisiting Order: `Arena_Radius` change *after* placing cells:**
        `...?cellsData=q0r0A&Arena_Radius=5`
        * Initial blank state (default radius).
        * `cellsData` places a Predator at `(0,0)`.
        * `Arena_Radius=5` is processed. The arena boundary is now set to a radius of 5. This **does not clear the cell**. The Predator at (0,0) is preserved.
        * *Outcome: An arena of radius 5 with a Predator at `(0,0)`.*
		
**Important General Notes for URL Configuration:**

* **Parameter Names:** For individual settings, always use the exact names from an exported "Settings (TSV)" file (first column).
* **File Access (CORS):** For `settingsFileURL` and `arenaFileURL`, the linked files must be publicly accessible on the internet. If they are on a different domain than your simulation page, the server hosting them **must** have Cross-Origin Resource Sharing (CORS) enabled (e.g., by setting `Access-Control-Allow-Origin: *` or to your specific domain). For local testing with your own files, running a simple local web server (e.g., Python's `http.server`) to serve your HTML and TSV files via `http://localhost/...` URLs is recommended.
* **URL Encoding:** Values in URLs, especially if they are other URLs or strings with special characters (like spaces, `&`, `?`, `=`), must be properly URL-encoded. Browsers often handle this if you copy-paste a full URL, but be mindful if constructing links manually or programmatically (e.g., JavaScript's `encodeURIComponent()`).
* **Debugging:** Open your browser's JavaScript console (usually F12). The simulation logs messages about which URL parameters it's processing and any errors encountered (e.g., file not found, CORS issues, malformed data). This is very helpful for troubleshooting your URLs.

## 11. Technical Notes & Considerations

* **Browser:** Best viewed on modern desktop browsers (Chrome, Firefox, Edge, Safari) that fully support HTML5 Canvas and modern JavaScript (ES6+).
* **Performance:** Very large arena radii (e.g., >40-50) combined with high cell densities and long simulation durations can become computationally intensive and slow down the browser, especially if per-step image/state saving is enabled.
* **Current Limitations:** The model is grid-based and does not include nutrient diffusion, complex spatial structures beyond hex occupation, or evolutionary changes within a single run (parameters are fixed once a simulation starts).


## License and Attribution

This work (BacFighT6: Simulation of T6SS-mediated Bacterial Interactions and its associated code) is licensed under the **Creative Commons Attribution 4.0 International License (CC BY 4.0)**.

[![CC BY 4.0][cc-by-shield]][cc-by]

[cc-by]: https://creativecommons.org/licenses/by/4.0/
[cc-by-shield]: https://img.shields.io/badge/License-CC%20BY%204.0-lightgrey.svg

**Copyright (c) 2025 [Marek Basler](https://www.biozentrum.unibas.ch/basler)**

You are free to:
* **Share** — copy and redistribute the material in any medium or format for any purpose, even commercially.
* **Adapt** — remix, transform, and build upon the material for any purpose, even commercially.

The licensor cannot revoke these freedoms as long as you follow the license terms.

**Under the following terms:**
* **Attribution** — You must give appropriate credit to **[Marek Basler](https://www.biozentrum.unibas.ch/basler) - University of Basel - Biozentrum**, provide a link to the license, and indicate if changes were made. You may do so in any reasonable manner, but not in any way that suggests the licensor endorses you or your use.

For the full license text, please visit: [https://creativecommons.org/licenses/by/4.0/](https://creativecommons.org/licenses/by/4.0/)

## Support
Development of BacFighT6 runs on coffee! If you find it useful, please consider a **[small donation](https://ko-fi.com/O5O11GHYWF). Thank you!**
