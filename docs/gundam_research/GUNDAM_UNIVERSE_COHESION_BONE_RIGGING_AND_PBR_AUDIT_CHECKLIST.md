# Gundam-Universe Cohesion, Skeletal Bone Rigging & PBR Asset Parity
## Comprehensive Technical Audit, Architecture Blueprint & Verification Checklist

> **Document ID:** `GUNDAM-UNIVERSE-COHESION-AUDIT-SPEC-2026`  
> **Scope:** Full technical audit of 3D libraries, asset provenance, Gundam-universe roster integrity, skeletal bone hardpoint mounting, slot mutual-exclusion invariants, and PBR visual parity between base Mobile Suits and autonomously generated agentic modular parts.  
> **Triggering Visual Finding:** Equipping untextured procedural Python/NumPy modular parts onto off-universe mecha (`SERAPH-03`) caused severe silhouette clash, floating offsets, multi-pack clipping, and flat-plastic vs. AAA-weathered PBR mismatch.

---

## 1. Root-Cause Audit & Library Provenance Inventory

### 1.1 Exact Technology & Library Stack Currently Used

| System Layer | Current Library / Source | License | Measured Technical Profile | Audit Verdict |
|---|---|---|---|---|
| **Web 3D Renderer** | Three.js `r170` (`GLTFLoader`, `OrbitControls`, `RoomEnvironment`, `EffectComposer`, `UnrealBloomPass`) | MIT | ACESFilmic tone mapping, PMREM IBL (`envIntensity: 0.85`), controlled bloom (`threshold: 1.45, strength: 0.26`) | **PASS** — Crisp, eye-comfortable studio rendering |
| **Primary Gundam-Like Hero (`VANGUARD-07`)** | Pre-sculpted `.glb` from [`RamonLinares/atlas-09`](https://github.com/RamonLinares/atlas-09) (Tripo 3D + Blender retopo/rig + PBR texture bake) | **CC0 1.0 Public Domain** | `27,320` tris, `24` rig nodes, `7` baked animations, `4` embedded PBR textures (`BaseColor`, `Normal`, `ORM`, `Emission`) + `KHR_materials_clearcoat` | **PASS** — Authentic 18m Gundam real-robot silhouette & AAA PBR quality |
| **Off-Universe Units (`SERAPH-03`, `RONIN-04`, `ATLAS-09`)** | Pre-sculpted `.glb` from `RamonLinares/atlas-09` | CC0 1.0 | Winged cyber-demon (`SERAPH-03`), feudal samurai (`RONIN-04`), heavy dieselpunk walker (`ATLAS-09`) | **FAIL (UNIVERSE MISMATCH)** — Not in the Gundam real-robot universe; incompatible with Gundam modular parts |
| **Modular Parts (`Type-A`, `Type-S`, `Type-H`, `Type-E`, `Type-C`, `Beam Rifle`, `Shield`, `Psycho-Fin`)** | Pure Python + NumPy procedural mesh builder ([cad_compiler.py](file:///d:/Linkedin_Github/Codes_Container/src/mecha_engine/cad_compiler.py)) | Internal | Beveled/lofted/revolved meshes, **0 embedded PBR textures**, **0 normal/roughness maps**, **static world coordinates** (`Y=144mm, Z=-23.5mm`) | **PARTIAL / NEEDS UPGRADE** — Geometry compiles cleanly, but lacks PBR texture parity, bone rigging, and slot exclusion |
| **Local Host 3D CAD Tooling Check** | System CLI & Python environment probe | — | Installed: `numpy`, `Pillow (PIL)`. **Not installed:** `blender`, `bpy`, `build123d`, `cadquery`, `trimesh`, `openscad`, `freecad` | **DOCUMENTED** — Required tools identified for Tier 2/3 upgrade |

### 1.2 Four Root Causes of Visual & Spatial Clash Identified in Audit

1. **Off-Universe Chassis Contamination:**  
   `SERAPH-03`, `RONIN-04`, and `ATLAS-09` belong to completely different mecha genres (cyber-angel, samurai, Titanfall-style heavy mech). Mounting Federation Gundam wings or hexagonal shields onto a winged demon mecha or katana samurai fundamentally breaks lore and mechanical aesthetics.
2. **Static World-Space Mounting vs. Skeletal Bone Rigging:**  
   Modular parts were added to a static `packsGroup` at hard-coded `RX-78` millimeter coordinates (`Y = 144 mm, Z = -23.5 mm`) rather than bound via `bone.add()` to the active rig's `chest`, `forearm.L`, or `hand.R` bones. Result: parts floated above the head (`Y = 144` vs actual `VANGUARD-07` dorsal thruster height `Y = 129.6 mm`) and stayed frozen in mid-air while the mecha animated.
3. **Missing Slot Mutual-Exclusion & Conflict Matrix:**  
   Multiple `HP-00` dorsal packs (`Type-A Aero` + `Psycho-Fin Vernier Array`) could be equipped simultaneously, stacking and intersecting each other behind the head. Similarly, parts targeting `HP-07`/`HP-11` clipped into built-in arms/cannons.
4. **PBR Material & Texture Parity Gap:**  
   `VANGUARD-07` uses baked `Vanguard_BaseColor`, `Vanguard_Normal`, `Vanguard_ORM` (Occlusion/Roughness/Metalness), `Vanguard_Emission`, and `KHR_materials_clearcoat (0.22)`. In contrast, NumPy procedural parts used flat untextured `MeshStandardMaterial` colors without panel seams, surface grain, ambient occlusion, or edge wear.

---

## 2. Strict Gundam-Universe Cohesion Policy (Canon Roster Rules)

> **IRONCLAD ROSTER RULE:** Every Mobile Suit displayed in the 3D Gundam-Like Hangar **MUST** exist strictly within the **Gundam Real-Robot Universe** (18 m military humanoid Mobile Suit proportions, V-Fin or Mono-Eye optical head, standardized dorsal backpack interface, beveled military armor paneling, vernier thruster geometry). Non-Gundam robots (`SERAPH-03`, `RONIN-04`, `ATLAS-09`) must be **removed or archived** from the Gundam-Universe hangar roster.

### 2.1 Approved Gundam-Universe Chassis Architecture (Hero vs. Authentic Zeon / Neo-Zeon Enemies)

| Unit Designation | Faction / Role | Visual Standard | Hardpoint Compatibility | Status |
|---|---|---|---|---|
| **VANGUARD-07 (RX-Type Standard)** | Federation Hero Gundam-Like Real-Robot | Satin Polar White armor, Cobalt chest, Signal Red abdomen/skirts, Gold V-Fin, Emerald twin optics, dual backpack verniers, 7 combat animations (`27.3k` tris CC0 1.0) | 100% Live Chest-Bone `ADP-HP00-DORSAL` + Pelvis Rear-Waist Aux Racks | `[x]` Active (CC0 1.0 Verified) |
| **MS-06Z Zaku-II Infantry** | Principality of Zeon Mono-Eye Enemy | Master Grade Auto-Smooth (`72` nodes, `16.3k` tris): smooth spherical olive dome helmet, 180° sweeping Ruby Mono-Eye, braided cheek/waist/leg hoses, Left olive 3-spike dome pauldron, Right curved shoulder shield, 120mm drum-mag MG & Heat Hawk | 100% Physical `ADP-HP00-DORSAL` + Rear-Waist Aux Racks + Faction Olive PBR Sync | `[x]` Active (`zeon_enemies.py`) |
| **MS-07B Gouf Close-Combat Ace** | Zeon Ground-Combat Mono-Eye Ace | Master Grade Auto-Smooth (`71` nodes, `15.8k` tris): swept commander fin dome, sweeping Ruby Mono-Eye, twin curved horned shoulder pauldrons, two-tone military cornflower-blue/Prussian-navy armor, curved Knight Shield & molten-orange Thermal Heat Saber | 100% Physical `ADP-HP00-DORSAL` + Rear-Waist Aux Racks + Faction Blue/Navy PBR Sync | `[x]` Active (`zeon_enemies.py`) |
| **MSN-04S Sazabi Commander** | Neo-Zeon Heavy Mono-Eye Commander | Master Grade Auto-Smooth (`71` nodes, `14.2k` tris): sweeping Neo-Zeon crested helmet + tall blade, sweeping Emerald Mono-Eye, multi-panel Crimson-Maroon/Obsidian torso with Golden Abdominal Mega-Particle Cannon, massive flared shoulder binders, bell-bottom hover legs & Beam Shot-Rifle | 100% Physical `ADP-HP00-DORSAL` + Rear-Waist Aux Racks + Faction Crimson/Gold PBR Sync | `[x]` Active (`zeon_enemies.py`) |
| **RX-78 Parametric CAD Inner Frame** | Engineering Disassembly Lab | 95-node exploded Ver.Ka Movable Frame for pure mechanical inspection & bare-arm direct mounting | Full 26-station CAD reference (`HP-00`, `HP-07` Forearm, `HP-11` Hand) | `[x]` Active (CAD Lab Mode) |

> **Anti-Monochrome Rule:** Hero Gundams are NEVER painted into flat all-red or all-blue monochromatic recolors. Enemy slots are strictly filled by distinct, canon Gundam-Universe Mono-Eye Mobile Suit silhouettes (`Zaku-II`, `Gouf`, `Sazabi`) with authentic multi-tone military color blocking, connected inner-frame skeletons, and Blender-grade area-weighted Auto-Smooth normals (`42°` crease threshold).

---

## 3. Exact Measured Skeletal Bone Map (`vanguard-07.glb`)

Extracted directly from the binary glTF 2.0 scene graph of [vanguard-07.glb](file:///d:/Linkedin_Github/Codes_Container/assets/mecha/models/cc0/vanguard-07.glb) (`18.0 m` native height → `10.0x` scene scale = `180.0` Hangar units):

### 3.1 Full 24-Node Skeletal Hierarchy

```
[23] VANGUARD_07_RIG
 └── [21] root
      └── [20] pelvis (trans: [0.000, 8.700, 0.000])
           ├── [11] chest (trans: [0.000, 1.900, 0.000])
           │    ├── [00] head (trans: [0.000, 4.754, 0.000])
           │    ├── [09] Muzzle_Thruster_L (trans: [+0.900, 2.361, -2.998])  ◄── BUILT-IN DORSAL LEFT
           │    ├── [10] Muzzle_Thruster_R (trans: [-0.900, 2.361, -2.998])  ◄── BUILT-IN DORSAL RIGHT
           │    ├── [03] upper_arm.L (trans: [+3.300, 4.311, -0.169])
           │    │    └── [02] forearm.L (trans: [0.000, 2.513, 0.000])       ◄── LEFT FOREARM SHIELD BONE
           │    │         └── [01] hand.L (trans: [0.000, 2.540, 0.000])
           │    └── [08] upper_arm.R (trans: [-3.300, 4.311, -0.169])
           │         └── [07] forearm.R (trans: [0.000, 2.513, 0.000])
           │              └── [06] hand.R (trans: [0.000, 2.540, 0.000])     ◄── RIGHT HAND WEAPON BONE
           │                   └── [05] rifle.R (trans: [-1.346, 1.749, -1.127])
           │                        └── [04] Muzzle_Rifle (trans: [0.000, 5.359, 0.000])
           ├── [14] thigh.L → [13] shin.L → [12] foot.L                      ◄── LEFT LEG CHAIN
           ├── [15] skirt.L (trans: [+1.900, 0.000, 0.000])                  ◄── LEFT SKIRT HARDPOINT
           ├── [18] thigh.R → [17] shin.R → [16] foot.R                      ◄── RIGHT LEG CHAIN
           └── [19] skirt.R (trans: [-1.900, 0.000, 0.000])                  ◄── RIGHT SKIRT HARDPOINT
```

### 3.2 UHIS Hardpoint-to-Bone Socket Binding Specification

Instead of placing modular parts at static world coordinates, every modular `.glb` **MUST** be normalized around its own local mounting origin `[0, 0, 0]` (dovetail plug origin) and attached directly to the target rig bone via `targetBone.add(mountPivot)`:

| UHIS Station | Role | Target Bone in `VANGUARD_07_RIG` | Exact Local Offset (Rig Meters) | CAD-to-Rig Scale | Animation Sync |
|---|---|---|---|---|---|
| **`HP-00`** | Dorsal Spine Mission Pack Hub | `chest` | `[0.000, 2.361, -2.998]` (exact midpoint of `Muzzle_Thruster_L/R`) | `0.1` (`100mm -> 1.0m`) | Moves 100% with torso tilt, `BoostJump`, `KneelFire`, `Run`, `Backflip` |
| **`HP-01`** | Cranial Sensor / Recon Pod | `head` | `[0.000, 1.650, -0.450]` | `0.1` | Tracks head pitch/yaw |
| **`HP-03 / HP-04`** | Shoulder Artillery / Binder Pylons | `chest` | `[±2.600, 4.100, -1.600]` | `0.1` | Stays locked to upper shoulder clavicle |
| **`HP-07`** | Left Forearm Auxiliary Shield / Pile-Bunker | `forearm.L` | `[+0.850, 1.250, 0.000]` | `0.1` | Moves with `ShieldGuard` block animation! |
| **`HP-11`** | Right Hand Primary Ordnance | `hand.R` | `[0.000, 0.650, 0.000]` | `0.1` | Aims dynamically during `RifleBurst` & `KneelFire`! |
| **`HP-13 / HP-14`** | Side Hip / Skirt Weapon Racks | `skirt.L` / `skirt.R` | `[±0.450, -0.600, 0.000]` | `0.1` | Pivots with hip skirt articulation |
| **`HP-21 / HP-22`** | Heel Ground Recoil Spades | `foot.L` / `foot.R` | `[0.000, 0.300, -1.250]` | `0.1` | Plants solidly with foot pose |

---

## 4. Slot Mutual-Exclusion & Unit Compatibility Contract

To guarantee zero stacked/clipping parts (as occurred when `Type-A Aero` + `Psycho-Fin` + `Federation Shield` were equipped simultaneously):

1. **Exclusive Primary Slot Rule (`1 Primary Station = 1 Active Part`):**
   - Each modular part declares a `primary_hardpoint` (e.g., `HP-00`, `HP-07`, `HP-11`).
   - Equipping Part B onto an already-occupied `primary_hardpoint` **automatically dismounts Part A** from that station with a clean HUD toast notification (`Swapped HP-00: Type-A Aero → Type-S Space`).
2. **Built-In Equipment Awareness (`VANGUARD-07` Native Rifle & Shield):**
   - Because `VANGUARD-07` already includes a sculpted beam rifle on `hand.R` and forearm shield on `forearm.L`:
     - **Dorsal Packs (`HP-00`)** — `Type-A Aero`, `Type-S Space`, `Type-H Over-Shoulder Artillery`, `Type-E AWACS Radome`, `Psycho-Fin Wing-Binder` — mount cleanly onto the empty dorsal spine (`[0, 2.361, -2.998]`) with **zero overlap**!
     - **Hand / Forearm Parts (`HP-07`, `HP-11`)** — Either carry explicit `compatible_units: ["cad_rx"]` (for bare-handed CAD frames) OR mount as **Stowed Dorsal / Shoulder Auxiliary Ordnance** when equipped on `VANGUARD-07` so they NEVER clip through `VANGUARD-07`'s native rifle or shield!
3. **Zero-Origin Local Part Authoring Standard:**
   - Every modular part `.glb` generated by `cad_compiler.py` (or Blender/build123d) **MUST** place its `mounting_dovetail_plug` at local `[0, 0, 0]` so bone attachment requires zero magic per-part world offsets.

---

## 5. PBR Visual Parity Engineering Specification

### 5.1 Ground-Truth Material Profile Extracted from `vanguard-07.glb`

Every modular part attached to a Gundam-Universe chassis must match these exact measured PBR parameters:

| Material Zone | Base Color / Hex | Metallic | Roughness | Clearcoat | Emissive |
|---|---|---|---|---|---|
| **Satin Armor Plating** | `#dfe5ee` (slightly weathered polar white, NEVER `#ffffff`) | `0.15` | `0.32` (with roughness map variation `0.24–0.48`) | `factor: 0.22`, `roughness: 0.22` | `0` |
| **Cobalt Chest Accent** | `#244a96` | `0.24` | `0.34` | `0.22` | `0` |
| **Signal Crimson Accent** | `#b82424` | `0.22` | `0.34` | `0.22` | `0` |
| **Graphite Actuators / Frame** | `#08090a` (`[0.03, 0.035, 0.04]`) | `0.72` | `0.36` | `0.0` | `0` |
| **Signal Yellow / Gold Trim** | `#b38014` (`[0.70, 0.50, 0.08]`) | `0.60` | `0.35` | `0.15` | `0` |
| **Sensor Optics Emission** | `#59ff80` (`[0.35, 1.0, 0.50]`) | `0.10` | `0.30` | `0.0` | `strength: 4.0` |
| **Beam / Plasma Emission** | `#d973ff` (`[0.85, 0.45, 1.0]`) | `0.10` | `0.25` | `0.0` | `strength: 5.0` |

### 5.2 Three-Tier Roadmap for Autonomous Agentic 3D Part Generation

| Tier | Pipeline / Library | Output Quality | Requirements | Best Use |
|---|---|---|---|---|
| **Tier 1 (Immediate — Zero New Installs)** | **Procedural Canvas PBR Texture Synthesizer** in Three.js (`512×512` tileable Albedo + Panel Seams + Micro-Rivets + Edge Wear + Sobel Normal Map + Roughness/Metalness Map + `MeshPhysicalMaterial` `clearcoat: 0.22`) + **Bone Mounting** + **Auto Faction Color Matching** | Immediately eliminates "flat white cardboard" look; parts match weathered satin white, Titans navy, or Commander crimson armor dynamically | Runs 100% client-side in browser (`mecha.html`) | Immediate visual parity & bone-synced animations |
| **Tier 2 (Local Parametric CAD + Headless Blender)** | `pip install build123d trimesh` + **Blender CLI (`bpy` headless)** — real OpenCASCADE fillets, boolean vents, greebles, bevel modifiers, and baked `Normal` + `AO` + `ORM` textures embedded into `.glb` | True industrial Ver.Ka hard-surface CAD quality with real baked ambient occlusion & normal maps (`5k–15k` tris per part) | Install `build123d` & Blender/bpy on build runner | Fully autonomous zero-human-intervention engineering CAD synthesis |
| **Tier 3 (Full Agentic AI-3D Pipeline)** | **Tripo3D / Hunyuan3D / TRELLIS** API or open-weights generator → automated Blender Python retopology, decimation, UV/PBR bake, and local-origin dovetail alignment → automated Three.js headless screenshot QA | Indistinguishable from `VANGUARD-07` (`20k–30k` tris sculpted PBR) for both new Gundam-universe Rival Mobile Suits (Zaku/Sazabi-like) and complex mission packs | GPU / 3D generation API key | Next-generation autonomous Mobile Suit & Rival generation |

---

## 6. Universal Gunpla Snap-Fit Adapter & Mechanical *"KLIK!"* Latch Architecture

> **Visual Audit Proof (`VANGUARD-07` Close-Up Inspection):**
> 1. **Coordinate Chirality Inversion Caught:** In `vanguard-07.glb`, the **Right Arm (holding the Beam Rifle)** sits on **`-X`** (`upper_arm.R trans = [-3.30, 4.31, -0.17]`) and the **Left Arm (built-in shield)** sits on **`+X`** (`upper_arm.L trans = [+3.30, 4.31, -0.17]`). Our old static parts assumed `+X = Right / -X = Left`, causing the modular `Federation Shield` (`X = -46.5`) to clip directly through VANGUARD-07's right forearm and Beam Rifle!
> 2. **Dorsal Backpack Interpenetration & Floating Sub-Parts:** Equipping `Type-A Aero` + `Psycho-Fin Vernier Array` simultaneously drove the aero fuselage and rudder straight through VANGUARD-07's native cobalt dorsal backpack and helmet, while child vernier bells (`fin_vernier_*`) drifted detached in mid-air because their offset was computed relative to world origin instead of their parent fin tip.
> 3. **Missing Physical Adapter Bridge:** Real Gunpla kits (Bandai HG/MG/RG/30MM Striker Pack & F90 Mission Pack systems) NEVER jam wings directly into armor — they use a **dedicated Rear Backpack Adapter Bracket** (twin 3.0mm pegs + beveled docking clamp seated flush on the backpack rear face) with a tactile **slide-in snap-fit lock** that produces a crisp mechanical ***"KLIK!"*** when seated to exact zero-gap clearance.

### 6.1 Universal Adapter System Architecture (3-Layer Physical Seating)

Every modular attachment MUST pass through a **3-Layer Adapter Pipeline** (zero raw coordinate floating):

```
[BASE CHASSIS BONE]          [PHYSICAL ADAPTER BRACKET]            [MODULAR PART PAYLOAD]
  VANGUARD_07_RIG              uhis_adapter_hp00_dorsal              Local Origin [0,0,0]
  └─ bone: "chest"    ──►    (Twin 3.0mm Pegs + Graphite     ──►    Slides in along -Z→+Z
     socket: [0, 2.36, -3.0]   Dovetail Clamp seated flush            until flush at Adapter
     normal: (0, 0, -1)        on rear blue backpack face             Socket Face -> "KLIK!"
                               at Z = -3.45m, zero clip!)             + Cyan Lock Pulse Ring
```

#### Defined Mechanical Adapters (Per Hardpoint Class)

| Adapter ID | Mounted Bone | Physical Geometry Built into Adapter | Clearance Offset (From Armor Surface) | Accepted Payload Categories |
|---|---|---|---|---|
| **`ADP-HP00-DORSAL`** | `chest` | Graphite machined twin-peg docking plate (`22×26×5 mm`) with beveled clamp rails, 2x chamfered `3.0mm` Bandai pegs seated flush on VANGUARD-07's rear blue backpack face, and rear female UHIS dovetail channel | Rear mount face cleanly offset at **`Z_local = -3.55 m`** (clears native backpack & dual thrusters with **0.0 mm interpenetration**) | `Type-A Aero`, `Type-S Space`, `Type-H Over-Shoulder Artillery`, `Type-E AWACS Radome`, `Psycho-Fin Wing-Binder` |
| **`ADP-HP03-SHOULDER`** | `chest` (dorsal clavicle yoke) | Articulated twin over-shoulder bridge arm wrapping cleanly around neck/collar clearance (`±2.4m X, +4.2m Y, -2.2m Z`) | Clears head V-Fin & shoulder pauldrons across full `Sentinel` / `KneelFire` / `RifleBurst` motion | Over-shoulder dual cannons, missile pods, sensor masts |
| **`ADP-HP07-FOREARM`** | `forearm.L` (`+X` side!) | Wrap-around C-clamp graphite forearm bracket with `3.0mm` polycap swivel peg aligned to **`+X` outer forearm normal** | Auto-detects if unit has built-in shield (if `hasNativeShield: true`, mounts auxiliary shield onto **`ADP-HP00-STOW`** dorsal sling or unlocks only on bare-arm units!) | Composite Shields, CQC Pile-Bunker Breacher |
| **`ADP-HP11-WEAPON`** | `hand.R` (`-X` side!) / Dorsal Weapon Rack | Articulated hand grip adapter or dorsal fold-out weapon sub-arm | If `hasNativeRifle: true`, stows auxiliary rifle cleanly on dorsal waist/backpack rack instead of clipping native rifle! | Beam Rifles, Hyper Bazookas, Railguns |
| **`ADP-HP13-SKIRT`** | `skirt.L` (`+X`) / `skirt.R` (`-X`) | Dual side-skirt polycap rail adapter locked to hip skirt plates | Moves naturally with leg stride & `BoostJump` | Spare E-Pacs, Grenade Racks, Beam Saber Holsters |

### 6.2 Tactile Gunpla Assembly Sequence — Visual Slide-In + Acoustic *"KLIK!"* Lock

When the user clicks **`+ EQUIP`** on any modular part, the hangar executes a **3-Stage Mechanical Snap-Fit Sequence** (instead of teleporting a static mesh):

1. **Stage 1 — Adapter Engagement (`0–120 ms`):**
   - If not already present on that station, the physical graphite **Adapter Bracket** (`ADP-HP00-DORSAL`, etc.) materializes seated flush on the target bone socket; any previously equipped part occupying the same exclusive slot smoothly slides out and dismounts first (zero stacking!).
2. **Stage 2 — Guided Rail Slide-In (`120–340 ms`):**
   - The modular part spawns offset `+2.8 m` (`28 mm`) along the adapter's outward insertion rail normal (`-Z` for dorsal packs) and eases rapidly along the rail (`cubic-bezier` mechanical slide with subtle deceleration right before contact) until its male dovetail plug bottoms out flush against the adapter clamp (`offset = 0.0`).
3. **Stage 3 — Mechanical *"KLIK!"* Snap-Lock (`340 ms` impact frame):**
   - **Physical Recoil Micro-Settle:** Tiny `0.4 mm` high-frequency damped snap impulse (`spring overshoot 1.0 → 0.985 → 1.0` over `70 ms`) so the eye *feels* the solid plastic/titanium lock-in!
   - **Acoustic Web Audio *"KLIK!"* Synthesizer (`playGunplaSnapClick()`):** Multi-layered physical snap sound synthesized cleanly via Web Audio API (zero external audio files):
     - Layer 1 (`0 ms`): Crisp `3.8 kHz–6.2 kHz` bandpass transient crack (`12 ms` decay) — authentic ABS/PS Gunpla polycap snap-fit *"KLIK!"*.
     - Layer 2 (`14 ms`): Tight `420 Hz → 190 Hz` metallic titanium latch thud (`45 ms` decay) — mechanical detent pin seating.
     - Layer 3 (`28 ms`): Subtle `1,480 Hz` short telemetry confirmation ping (optional HUD lock chime).
   - **Visual Lock Confirmation:** Brief `250 ms` glowing cyan lock-ring flash around the adapter seam + HUD toast: `🔒 [KLIK!] HP-00 Dorsal Adapter Locked — Type-A Aero Mounted`.
   - **Dismount Feedback:** Clicking an equipped part plays a reverse metallic *unlatch-slide* sound (`playGunplaUnlatch()`) and slides the part smoothly off the adapter rail before removal.

### 6.3 Sub-Part Local Parent-Hierarchy Rule (Zero Floating Verniers)
- Every multi-piece modular pack (e.g., `propulsion_vernier_wing_pod` fin binders + micro-verniers, `type_a_aero` wings + thrusters, `type_h_artillery` barrels + recoil sleeves) **MUST** compute child nozzle positions directly from the parent wing/fin's local end-cap vector **before** rotation, or bind them as children of the fin node — **eliminating 100% of detached floating thruster cones** seen in `media_1795262386813_a8b659a3.jpg`.

---

## 7. Master Implementation & Audit Checklist

### Phase A — Gundam-Universe Roster Cleanup & Canon Zeon / Neo-Zeon Enemy Mobile Suits
- [x] Remove off-universe units (`SERAPH-03`, `RONIN-04`, `ATLAS-09`) from the Gundam-Universe roster in `mecha.html` and delete unused off-universe GLBs (~21 MB saved).
- [x] Establish **VANGUARD-07** as the canonical Federation Hero Gundam-Like Real-Robot baseline (zero weird monochrome Gundam recolors!).
- [x] Sculpt & deploy **3 Authentic Gundam-Universe Enemy Mobile Suits** (`src/mecha_engine/zeon_enemies.py`) with Blender-grade `42°` Auto-Smooth Weighted Normals, connected inner-frame skeletons, live sweeping Mono-Eye scanner animation, and multi-tone matte-military PBR:
  - [x] `MS-06Z Zaku-II Infantry` (Two-Tone Zeon Olive / Charcoal / Ruby Mono-Eye / Left 3-Spike Dome + Right Curved Shield / 120mm Drum MG & Thermal Heat Hawk)
  - [x] `MS-07B Gouf Close-Combat Ace` (Two-Tone Military Blue / Prussian Navy / Ruby Mono-Eye / Twin Curved Horned Pauldrons / Knight Shield & Molten Heat Saber)
  - [x] `MSN-04S Sazabi Commander` (Crimson-Maroon / Oxblood / Obsidian / Gold / Emerald Mono-Eye / Flared Shoulder Binders / Belly Mega-Particle Cannon / Bell-Bottom Hover Legs)
- [x] Retain `RX-78 CAD Inner Frame` (`95` nodes) strictly as dedicated **Exploded CAD Lab** inspection mode.
- [x] Add full **Mateus Yonathan** author branding across `<title>`, `<meta name="author">`, JSON-LD, single-row top nav (`MATEUS YONATHAN // MECHA HANGAR`), Telemetry Architect badge, and Info Guide modal.

### Phase B — Physical Adapters, Chirality Fix, Bone Mounting & *"KLIK!"* Snap-Fit
- [x] Fix **Left/Right Chirality (`-X = Right Arm/Rifle`, `+X = Left Arm/Shield`)** to match `vanguard-07.glb`'s verified glTF skeleton.
- [x] Refactor `cad_compiler.py` so every modular part's `mounting_dovetail_plug` sits cleanly at **local origin `[0, 0, 0]`** AND fix `propulsion_vernier_wing_pod` / `type_s_space` vernier bell attachment so **0 floating cones** exist.
- [x] Build physical machined graphite **Adapter Brackets** (`adapter_hp00_dorsal.glb` — `ADP-HP00-DORSAL`, `ADP-HP07-FOREARM`, `ADP-HP11-WEAPON`) seated flush onto `VANGUARD_07_RIG` live bones (`chest`, `pelvis`) and all 3 Zeon enemy dorsal bosses with **0.0 mm backpack/head/rifle clipping**.
- [x] Implement **3-Stage Gunpla Snap-Fit Animation** (Adapter engage → Guided rail slide-in → Micro-recoil detent settle + Razor-thin Cyan lock-seam ring flash) + **3-Layer Web Audio Mechanical *"KLIK!"* Snap-Fit Sound** (`playGunplaSnapClick()` / `playGunplaUnlatch()`).
- [x] Enforce **1-Slot Mutual Exclusion** (`dorsal_pack`, `aux_shield`, `aux_weapon` auto-swap — zero overlapping wings!).
- [x] Enforce **Native-Equipment Awareness** so armed suits (`VANGUARD-07`, `Zaku-II`, `Gouf`, `Sazabi`) stow extra shields/rifles cleanly onto sleek rear-waist tactical racks instead of clipping built-in weapons.
- [x] Verify all bone-attached adapters & packs stay 100% locked across `Sentinel`, `RifleBurst`, `ShieldGuard`, `BoostJump`, `KneelFire`, `Run`, and `Backflip`.

### Phase C — PBR Texture & Weathering Parity + Portfolio Navigation Polish
- [x] Add **Organic Brushed Weathered Gundarium PBR Material Factory** (`MeshPhysicalMaterial` with organic brushed grain, subtle panel seams, Sobel normal map, weathered roughness map — zero tiled grid artifacts) matching `VANGUARD-07`'s military matte-satin finish.
- [x] Sync modular part color palette dynamically with active Gundam-Universe unit faction (`Federation White/Cobalt/Red`, `Zeon Olive`, `Zeon Blue/Navy`, `Neo-Zeon Crimson-Maroon/Gold`).
- [x] Fix `index.html` top navigation dropdown hover-collapse dead gap (invisible hover bridge `::before` + 240ms hover-grace & click-toggle JS verified via headless Chrome).
- [ ] Optional future **Tier 2 (`build123d` + headless `bpy`)** / **Tier 3 (Hunyuan3D / Tripo3D)** upgrade path documented for next-gen 25k-tri sculpted rival assets.

### Phase D — Already Verified & Protected (Do Not Regress!)
- [x] **Top Navigation Cleanliness:** 1 standalone `🏠 Main Page` button + 1 dedicated `🤖 3D Pages ▾` dropdown (no mixed LinkedIn/GitHub/Macro links).
- [x] **Application Menu Show/Hide & Show/Hide All:** Persistent top-right pill bar (`◧ Parts`, `◨ Specs`, `🎨 Scene`, `▬ Dock`, `👁️ HIDE ALL / SHOW ALL (H)`) + zero scrollbar artifact in Cinema Mode.
- [x] **Autonomous Agentic Guide Modal (`ℹ️ Guide`):** Explains zero-human-intervention agentic synthesis pipeline, 26-station UHIS hardpoints, shaders, and controls.
- [x] **Gundam-Style Battle BGM (`MechaBattleAudioEngine`):** 3 pre-rendered Web Audio orchestral-synth battle tracks (`Orbital Sortie`, `Zero-G Combat`, `Launch Deck`).
- [x] **Comfortable Studio Lighting:** Balanced `RoomEnvironment` IBL + `UnrealBloomPass` threshold `1.45` (zero blinding glare).
- [x] **Portfolio Integration & Typography:** `🤖 3D Gundam-Like Hangar` Hero CTA + Flagship Card in `index.html`, zero `1/100 Lab` jargon, and zero `C\#` escapes across all pages & generators (`data_collector.py`, `paper_landing_generator.py`).
- [x] **Automated QA Gate:** `pytest tests/test_mecha_engine.py` (100% PASS) + real headless Chrome WebGL screenshot verification (0 console errors) + surgical `git diff` audit before every commit.
