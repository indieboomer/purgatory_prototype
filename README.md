PROJECT: PURGATORY WORKS
Playable prototype brief — working title, not a cleared commercial name.

MISSION

Build a small, polished, playable Windows FPS prototype that communicates:

“Painkiller-like combat energy, a world made from small destructible
voxels, and bright yellow acid splashing through the destruction.”

Use those games as experiential references, not as sources of copied
assets, characters, weapons, levels, or branding.

This must feel like a fast action game from the beginning.
Environmental interaction happens while moving and fighting.
Do not turn it into a slow demolition puzzle or a physics sandbox
without combat.

The prototype is successful when a short, unedited gameplay clip
clearly shows:
- satisfying FPS movement and shooting;
- visibly small voxels;
- real geometric destruction;
- liquid escaping through damage;
- acid damaging enemies and changing the arena;
- a distinctive monochrome retro visual identity.

TECHNOLOGY DECISION

First inspect the available repository and its instructions.

Default:
Reuse the existing Rust/Vulkan voxel project if it already provides
a functioning renderer, input, player movement, and usable world data.

Evaluate it with a small end-to-end experiment:
remove voxels, update visible geometry and collision, and let liquid
flow through the resulting opening.

Keep existing working systems unless there is concrete evidence
that they prevent the prototype.

Do not migrate to BRUTAL merely because it is new or uses C#.
BRUTAL is a foundation for a custom engine, not a ready-made voxel
renderer, destruction system, or fluid simulator.

If no usable project exists:
Prefer Rust + wgpu for the first implementation, using a supported
native backend. Pin dependency versions and consult their actual APIs.

If this repository already contains a functioning BRUTAL/C# engine,
reuse it rather than rewriting it in Rust.

Record the chosen stack and the evidence behind that choice.
Do not create multiple competing implementations.

SCOPE

One compact arena.
One enemy archetype.
One distinctive weapon with two complementary firing modes.
One simulated liquid: yellow acid.
One short encounter lasting approximately 2–4 minutes.
Immediate restart.

No multiplayer, LLM integration, open world, procedural campaign,
inventory system, crafting, skill tree, or general-purpose editor.

SETTING

A broken industrial purgatory.

Build an underground purification chamber combining:
- vaulted masonry;
- heavy pipes;
- acid cisterns;
- iron platforms;
- drainage channels;
- machinery embedded in religious-looking architecture.

The place should feel like an ancient processing plant for souls.
Communicate this visually; do not spend the prototype on lore text.

LEVEL

Suggested initial envelope: approximately 24 x 24 x 10 metres.
Adjust dimensions if traversal or performance testing justifies it.

Use one main combat chamber with:
- a central fighting area;
- an elevated side platform;
- a shallow lower drainage area;
- two elevated acid reservoirs;
- a small number of destructible pillars and wall sections;
- a loop that supports continuous movement;
- ramps or stairs that remain readable during combat.

Create purposeful material placement.
Do not fill the room with uniformly noisy voxel detail.

Spawn the player with a clear view of an enemy and an acid reservoir.
The first satisfying interaction should be available within seconds.

Use a fixed initial layout and seed for repeatable testing.

DESTRUCTION

Structural voxel size: 0.10 metres.

This scale must be visible at broken edges, impact craters and debris.
Do not use metre-sized Minecraft blocks.

All ordinary exposed arena structures should be damageable.
An outer containment shell and foundational anchors may remain
indestructible; keep them visually distinct and explain the boundary
in developer documentation.

Weapon impacts must remove actual world material.
Do not substitute decals for destruction.

Destroyed space must become empty for:
- rendering;
- player and enemy collision;
- projectile queries;
- fluid movement.

Support partial wall damage, holes, broken tank walls, and destroyed
platform sections.

Include one convincing localized collapse:
removing a support causes an unsupported section to detach or break.

Use bounded local connectivity checks or a similarly practical method.
Do not build a full engineering stress simulator.

Do not create a rigid body for every voxel.
Use capped physical chunks and pooled cosmetic debris.

The player must traverse rubble without constantly snagging.
Tiny decorative fragments should not block movement.

FLUID AND ACID

Implement a real, bounded simulation of liquid volume.

Required behaviour:
- stored liquid leaves a damaged reservoir;
- liquid falls under gravity;
- spreads across connected surfaces;
- collects in depressions;
- passes through newly created holes;
- falls from broken platforms;
- damages enemies and the player;
- gradually corrodes selected materials.

A voxel or cellular volume solver is acceptable.
Scientific fluid accuracy is not required.

Start with active fluid regions rather than a dense update of the
whole arena. A 0.20-metre fluid grid is an acceptable initial setting
while structural voxels remain 0.10 metres.

Explicitly handle coupling between resolutions:
a visible opening should not mysteriously remain sealed to fluid.
Use face openness, refined boundary handling, or another documented
method that passes a small-opening test.

Use fixed simulation timesteps, bounded substeps and non-negative
fluid amounts. Track volume transfers and intentional sinks.

Define material behaviour:
- stone: destructible by weapon, slow acid corrosion;
- metal: destructible by weapon, acid-resistant for this prototype;
- brittle deposits: rapidly corroded;
- containment material: indestructible.

Corrosion should consume a finite acid activity budget or have another
explicit limiting rule, preventing tiny spills from dissolving the
entire level forever.

Use additional visual droplets and splash particles to communicate
speed and impact. Cosmetic droplets must not be the only liquid system.

Avoid realistic ocean rendering, full Navier–Stokes simulation,
multiple liquids, steam chemistry, and elaborate fire propagation
in this prototype.

PLAYER MOVEMENT

Prioritize immediate, responsive FPS movement:
- fast ground movement;
- jump;
- useful air control;
- smooth stepping over small voxel irregularities;
- responsive mouse look;
- consistent behaviour across frame rates.

Start around 8 m/s ground speed and tune in play.
Do not add stamina.

Keep camera shake short and adjustable.
No forced head bob, heavy motion blur, or long damage interruptions.

The player must be able to shoot while moving and jumping.

WEAPON: THE PENITENT ENGINE

A compact industrial-religious weapon:
a heavy piston mechanism around a small illuminated chamber,
with a readable silhouette and visible moving parts.

Build it from simple coherent forms.
It must look intentionally designed rather than like random pipes.

Primary fire:
- launches a fast kinetic spike;
- causes strong direct damage and stagger;
- excavates a narrow, clearly visible path through weak material;
- breaks tanks and supports;
- has a forceful mechanical firing sound and short recoil.

Use swept collision queries to prevent fast projectiles passing
through enemies or thin structures.

Secondary fire:
- emits a short-range pressure burst;
- pushes enemies and loose physical debris;
- breaks nearby weakened material;
- produces a bounded splash/displacement of existing acid.

Do not invent liquid mass for the secondary effect.
If coupling impulse to the fluid solver is too expensive initially,
use a bounded redistribution of fluid volume and cosmetic droplets.

Make the two modes complementary:
pierce to open, burst to scatter.

Use a simple regenerating charge or unlimited primary ammunition
for the prototype. Avoid inventory and ammunition starvation.

ENEMY: THE CONDEMNED

One readable humanoid archetype with a dark body, pale mask,
and aggressive forward movement.

It must:
- pursue the player;
- telegraph a short melee lunge;
- react visibly to impacts;
- take acid damage;
- fall when support disappears;
- die with satisfying bounded voxel breakup.

Spawn approximately 6–10 over the encounter, with a sensible cap
on simultaneously active enemies.

Do not require a separate physical simulation for every body voxel.
Use a simple animated body representation and a controlled death
fragment effect.

Navigation must respond to destroyed walkable surfaces.
Use a practical local navigation representation updated in dirty areas.

Enemies may make dangerous choices, but must not repeatedly run
against a removed staircase or float over missing floors.

Prioritize silhouette, animation timing, impact response and sound
over model detail.

ART DIRECTION

Monochrome retro 3D with small voxels and selective colour.

World:
- black;
- off-white;
- controlled ordered dithering for intermediate tones.

Accent colours:
- sulfur yellow for acid;
- restrained crimson for blood and hit traces.

Do not introduce coloured PBR lighting, blue electricity, orange fire,
or a full-colour environment.

The generated concept images are composition references.
Their realistic water, smooth shading and dense detail are not
requirements. Push the actual renderer toward a clearer retro look.

Use large calm light and dark areas.
Do not apply uniform noise over the whole screen.

Evaluate dithering during camera rotation and translation.
Choose a stable implementation; compare screen-space and
surface-anchored patterns rather than assuming one is always better.

Keep the crosshair and HUD sharp and outside world dithering.
Acid must remain distinguishable from architecture at a glance.

Expose:
- internal rendering resolution;
- dither intensity and scale;
- contrast thresholds;
- accent colour strength;
- camera shake.

Avoid expensive global illumination and ray tracing.
Start with simple directional/local lighting and inexpensive
depth cues sufficient for combat readability.

AUDIO AND FEEDBACK

Include working sound in the playable milestone:
- weapon firing and mechanical recovery;
- material impacts;
- tank rupture;
- liquid pouring and nearby splashing;
- corrosion;
- enemy attacks, damage and death.

Use properly licensed assets or generated procedural sounds.
Audio must make firing and material interactions satisfying.

Keep impact flashes brief and protect visibility of the target.

ARCHITECTURE AND PERFORMANCE

Use chunked voxel storage.
An initial chunk size of 16^3 or 32^3 is reasonable;
choose based on measured rebuild behaviour.

Do not represent each voxel as an entity or draw call.

Reuse an existing viable renderer.
Otherwise begin with visible-surface chunk meshes;
add meshing optimizations only where needed.

Update only dirty chunks.
Budget remeshing and collision updates.
Handle chunk boundaries and stale asynchronous results correctly.

For this small prototype, prefer CPU-authoritative world and fluid
state unless profiling demonstrates that a different approach is
necessary. Avoid building a complex GPU/CPU synchronization system
before the gameplay works.

Separate:
- fixed-step gameplay and simulation;
- rendering;
- cosmetic effects.

Set explicit limits for:
- active fluid cells;
- physical fragments;
- decorative particles;
- remeshing work;
- enemy count.

Degrade cosmetic effects before gameplay-critical state.

Performance goal:
Aim for stable 60 FPS at 1080p output on the available test machine,
using a lower internal resolution if appropriate.

This is a target, not a claim about untested hardware.
Report the exact CPU, GPU, resolution, build configuration,
frame-time percentiles and remaining spikes.

Include an optional debug overlay for:
- CPU and GPU frame time where measurable;
- active fluid cells;
- dirty chunks and remesh backlog;
- physical fragment count;
- enemy count.

PLAYABLE IMPLEMENTATION ORDER

1. Inspect and choose the existing technical base.
2. Implement movement, the weapon's primary fire, and one enemy.
3. Make voxel damage update visuals and collision.
4. Add a finite acid tank that leaks through damage.
5. Add acid damage and corrosion.
6. Add localized collapse and the secondary weapon mode.
7. Build the compact arena and encounter.
8. Tune retro rendering, sound and combat readability.
9. Profile the complete encounter and fix demonstrated bottlenecks.

Each stage must remain runnable.
Do not spend the project building a reusable general engine.

ACCEPTANCE CHECKS

Demonstrate all of these in the actual executable:

1. Shooting a wall removes small voxels and opens a traversable hole.
2. Shooting a tank makes acid leave through the damaged location.
3. Breaking the floor redirects the liquid into the lower area.
4. Acid damages an enemy and corrodes the intended material.
5. Removing support produces a visible localized collapse.
6. The player can keep moving and shooting during these events.
7. Enemies respond to the changed environment without floating.
8. The arena can be restarted reliably.
9. Repeated combat does not cause unbounded particle, fragment,
   fluid-state or memory growth.

Use targeted automated checks for volume accounting, chunk-boundary
edits and corrosion rules, plus a real gameplay run for feel and visuals.

DELIVERABLES

- Runnable Windows build.
- Source changes and concise run/build instructions.
- Short architecture and limitations note.
- Performance measurements with hardware details.
- Actual gameplay screenshots.
- A short unedited gameplay recording if capture is available.

Do not use generated concept images as proof of implementation.

WORKING STYLE

Make reasonable reversible implementation decisions autonomously.
Do not stop at a plan or ask approval for routine choices.

Preserve existing user work.
Do not perform a destructive migration.

If execution, GPU testing or recording is unavailable, state that
precisely and provide the runnable result and verification steps.
Do not claim unperformed testing.

The final result should be a small, exciting game slice:
fast combat, granular destruction, and yellow acid changing
the battlefield while the player is still fighting.
