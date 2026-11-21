# Full-Body 8-Zone Modular Gunpla Customization, Hierarchical Bone Sockets & Agentic Zero-Duplicate Synthesis
## Engineering Research Report, Mathematical Specification & Implementation Checklist

> **Document ID:** `GUNDAM-FULL-BODY-CUSTOMIZATION-SPEC-2026`  
> **Author:** Mateus Yonathan  
> **Scope:** Industry & academic research synthesis (*Bandai 30 Minutes Missions 30MM*, *Gundam Breaker 4*, *FA-78-1 Full Armor Gundam*, *ZGMF-X20A Strike Freedom*, and *Three.js Hierarchical Bone-Socket Kinematics*) establishing an **8-Zone Full-Body Modular Customization Architecture** across all anatomical stations (`Head`, `Chest`, `Shoulders`, `Backpack`, `Right Arm Weapon`, `Left Forearm Shield`, `Waist Hip Skirts`, and `Leg Calf Pods`), paired with an **Autonomous Agentic Zero-Duplicate Parametric CAD Synthesizer** and universal panel `✕` close ergonomics.

---

## 1. Web & Industry Research Findings

### 1.1 Bandai *30 Minutes Missions (30MM)* & *High Grade Build Custom (HGBC)* 3.0mm Universal Hardpoint Standard
Research into Bandai's 1/144 *30 Minutes Missions (30MM)* and *1/100 Master Grade F90 A-to-Z Project* reveals why real-world modular mecha kitbashing succeeds without part interpenetration:
1. **Standardized 3.0mm Cylindrical Pegs & UHIS Dovetail Ports:** Every anatomical block features recessed female sockets or dovetail rails (`3.00 mm` peg into `3.20 mm` socket, or `24.0 mm` tapered dovetail key) oriented along the local outward surface normal $\hat{\mathbf{n}}$ of the armor plate.
2. **Anatomical Section-by-Section Customization:** Rather than restricting add-ons to the backpack, full-body Option Armor Sets divide the Mobile Suit into **8 distinct anatomical zones**:
   - **Head / Cranial Unit:** Forehead crest mounts and temple 3mm ports for Vulcan pods, commander blade antennas, high-gain radome visors, and psycho-frame sensor hoods.
   - **Chest / Pectoral Torso Block:** Front pectoral peg sockets for *FA-78-1 Chobham* reactive breastplates, *Callidus*-style abdominal mega-particle emitters, or *I-Field* barrier generators.
   - **Shoulder Pauldrons (Left & Right):** Lateral and superior shoulder hardpoints (`HP-03` / `HP-05`) for 6-tube micro-missile pods, reactive shoulder shields, or side vernier thruster binders.
   - **Dorsal Spine Backpack (`HP-00`):** Primary striker/mission pack interface for swept aerodynamic wings, orbital booster pods + LH2 propellant tanks, twin 380mm over-shoulder cannons, AWACS rotodomes, and psycho-fin vernier arrays.
   - **Right Arm / Manipulator Weapon (`HP-11`):** Forearm/hand rail socket for long-barrel mega-beam rifles, twin-barrel Full Armor beam cannons, and 360mm hyper bazookas.
   - **Left Forearm / Gauntlet Shield (`HP-07`):** Outer lateral forearm clamp for hexagonal Gundarium tower shields, pneumatic CQC pile-bunker breachers, and anti-beam gatling shields.
   - **Waist / Hip Skirt Pylons (`HP-13` / `HP-17`):** Lateral hip mounts for *Strike Freedom Xiphias-3* folding electromagnetic railguns, spare E-Pac racks, and conformal grenade skirts.
   - **Lower Limbs / Calf Lateral Pods (`HP-24` / `HP-26`):** Outer calf hardpoints for *FA-78* auxiliary vernier thruster pods, 3-tube leg missile bays, and high-G thermonuclear hover units.

### 1.2 *Gundam Breaker 4* Full-Body Slot & Builder Parts Architecture
In *Gundam Breaker 4*, modular customization is governed by two structural invariants:
1. **Core Anatomical Slot Groups with Mutual Exclusion:** Each body zone (`Head`, `Body/Chest`, `Shoulders`, `Backpack`, `Right Weapon`, `Left Shield`, `Waist`, `Legs`) operates as an independent exclusive slot group (`1 Zone = 1 Active Attachment`). Equipping a new part into the `Chest` slot replaces any existing `Chest` attachment while leaving `Head`, `Shoulders`, `Backpack`, `Arms`, `Waist`, and `Legs` intact.
2. **Bone-Local Coordinate Transformation:** Every attachment is authored around its own local mounting plug origin $\mathbf{0} = [0, 0, 0]^T$ and parented directly to the underlying skeletal bone (`head`, `torso`, `armL`, `armR`, `forearmL`, `forearmR`, `pelvis`, `shinL`, `shinR`). When the Mobile Suit executes dynamic combat stances, every attached part inherits the exact world transformation matrix of its parent bone.

### 1.3 Three.js Hierarchical Bone-Socket Matrix Formulation
In Three.js (`r170`), when an articulated Mobile Suit is bound to an 11-Bone Hierarchical Kinematic Rig:

$$\mathbf{T}_{\text{world}}(\text{Part}_z) = \mathbf{T}_{\text{world}}(\text{Bone}_z) \cdot \mathbf{T}_{\text{socket}}(z) \cdot \mathbf{T}_{\text{slide}}(\tau) \cdot \mathbf{T}_{\text{explode}}(\lambda)$$

where:
- $\mathbf{T}_{\text{world}}(\text{Bone}_z) \in SE(3)$ is the live animated world matrix of the anatomical bone owning zone $z$ during 60fps `Combat & Kinetic Stance` animations (`Sentinel`, `RifleBurst`, `ShieldGuard`, `BoostJump`, `MeleeSlash`, `PatrolMarch`).
- $\mathbf{T}_{\text{socket}}(z)$ is the physical Gunpla adapter bracket offset and orientation on that bone.
- $\mathbf{T}_{\text{slide}}(\tau)$ is the 3-stage Guided Rail Slide-In & Damped Spring Detent Recoil matrix during the *"KLIK!"* snap-fit assembly sequence ($\tau \in [0, 1]$).
- $\mathbf{T}_{\text{explode}}(\lambda)$ is the radial CAD exploded-view displacement along the adapter's outward normal $\hat{\mathbf{n}}_z$ ($\lambda \in [0, 1]$).

For **bilateral limb zones** (`shoulder_pods` on `armL` + `armR`, and `leg_thrusters` on `shinL` + `shinR`), the attachment loader automatically splits left (`_l`, $X < 0$) and right (`_r`, $X > 0$) sub-meshes of the `.glb` assembly and parents them to their respective left and right limb bones (`armL`/`armR` or `shinL`/`shinR`), ensuring independent left/right limb articulation with zero cross-limb distortion!

---

## 2. 8-Zone Full-Body Anatomical Hardpoint Matrix

Every Mobile Suit in the unified 4-unit Gundam-Universe roster (`⚔️ RX-78 VANGUARD`, `👁️ MS-06Z ZAKU-II`, `⚡ MS-07B GOUF`, `🔥 MSN-04S SAZABI`) supports full 8-zone modular customization:

| Zone ID (`slot_group`) | UI Badge | UHIS Station(s) | Target Rig Bone(s) | Physical Adapter ID | Insertion Rail Vector $\hat{\mathbf{n}}_z$ | Default Socket Coordinates (1/100 mm) |
|---|---|---|---|---|---|---|
| **`head_sensor`** | `👑 HEAD` | `HP-01`, `HP-02` | `head` | `ADP-HP01-CRANIAL` | $(0, +0.45, +0.89)$ | $[0.0, 173.5, 4.5]$ (Cranial crest / temple ring) |
| **`chest_armor`** | `🛡️ CHEST` | `HP-09`, `HP-10` | `torso` | `ADP-HP09-PECTORAL` | $(0, 0, +1.0)$ | $[0.0, 138.5, 14.8]$ (Front pectoral breastplate) |
| **`shoulder_pods`** | `🚀 SHOULDERS` | `HP-03`, `HP-05` | `armR` (`_r`) + `armL` (`_l`) | `ADP-HP03-SHOULDER` | $(\pm 1.0, +0.25, 0)$ | $[\pm 43.5, 152.5, 0.0]$ (Outer pauldron hardpoints) |
| **`dorsal_pack`** | `🎒 BACKPACK` | `HP-00` | `torso` | `ADP-HP00-DORSAL` | $(0, 0, -1.0)$ | $[0.0, 142.0, -23.5]$ (Dorsal backpack socket) |
| **`aux_weapon`** | `🔫 R-WEAPON` | `HP-11` | `forearmR` / `pelvis` | `ADP-HP11-WEAPON` | $(-1.0, 0, +0.35)$ | $[-38.5, 102.0, 6.5]$ (Right forearm / weapon mount) |
| **`aux_shield`** | `🔰 L-SHIELD` | `HP-07` | `forearmL` / `pelvis` | `ADP-HP07-FOREARM` | $(+1.0, 0, 0)$ | $[+40.5, 104.5, 3.2]$ (Left outer forearm clamp) |
| **`waist_ordnance`** | `⚡ WAIST` | `HP-13`, `HP-17` | `pelvis` | `ADP-HP13-WAIST` | $(\pm 1.0, 0, 0)$ | $[\pm 21.5, 101.5, 0.0]$ (Side hip skirt pylons) |
| **`leg_thrusters`** | `🦵 LEGS` | `HP-24`, `HP-26` | `shinR` (`_r`) + `shinL` (`_l`) | `ADP-HP24-CALF` | $(\pm 1.0, 0, -0.25)$ | $[\pm 27.5, 38.0, -1.5]$ (Outer calf booster mounts) |

---

## 3. Agentic Continuous Zero-Duplicate Parametric Synthesizer

To fulfill the invariant: *"Jadi total catalog, packs, weapons, armor itu mestinya di generate agentic & selalu baru & tidak pernah ada yang sama"*:

### 3.1 Cryptographic Entropy Seed & Parametric Genome Derivation
1. **Deterministic Entropy Seed (`derive_entropy_seed`):**
   $$\text{seed\_hex} = \text{SHA-256}\left(\text{Timestamp}_{\text{UTC}} \parallel \text{ZoneArchetype} \parallel \text{SequenceNonce} \parallel \text{GXM\_UC2026}\right)$$
2. **Continuous Parametric CAD Genome (`build_agentic_parametric_part`):**
   - Bytes $b_0 \dots b_{31}$ of $\text{seed\_hex}$ are mapped into continuous engineering parameters (`wing_span_mm`, `sweep_deg`, `cannon_caliber_mm`, `barrel_length_mm`, `fin_count`, `breastplate_thickness_mm`, `missile_tube_count`, `railgun_length_mm`, `calf_nozzle_exit_r_mm`, `crest_blade_height_mm`).
   - Because the geometry vertices $\mathbf{V} \in \mathbb{R}^{N \times 3}$ are compiled directly from these continuous seed parameters, every synthesized `.glb` binary has a mathematically unique vertex buffer and a unique 64-character **SHA-256 binary asset hash**.
3. **Strict Zero-Duplicate Catalog Invariant (`register_modular_part`):**
   - Before registering any newly synthesized part into `data/mecha_catalog.json`, `catalog_manager.py` verifies:
     - $\text{part\_id} \notin \text{Catalog}$
     - $\text{serial\_number} \notin \text{Catalog}$
     - $\text{sha256\_hash} \notin \text{Catalog}$
     - $\text{entropy\_seed} \notin \text{Catalog}$
   - Any collision is rejected or re-seeded with a incremented cryptographic nonce.

---

## 4. Master Implementation & Verification Checklist

### Phase 1 — Research & Documentation
- [x] Research *Bandai 30 Minutes Missions (30MM)* universal 3mm joint & anatomical section customization (`Head`, `Chest`, `Shoulders`, `Backpack`, `Arms`, `Waist`, `Legs`).
- [x] Research *Gundam Breaker 4* full-body slot groups, Builder Parts hardpoints, and bone-socket attachment hierarchy.
- [x] Research *FA-78-1 Full Armor Gundam* & *ZGMF-X20A Strike Freedom* multi-zone hardpoint ordnance (Chobham chest armor, 6-tube shoulder missile pods, Xiphias folding hip railguns, calf missile/thruster pods, cranial vulcan/crest units).
- [x] Author comprehensive specification & checklist in `docs/gundam_research/FULL_BODY_MODULAR_CUSTOMIZATION_AND_AGENTIC_SYNTHESIS_CHECKLIST.md`.

### Phase 2 — 8-Zone Full-Body CAD Compiler & Agentic Zero-Duplicate Synthesizer (`src/mecha_engine/`)
- [x] Upgrade `src/mecha_engine/cad_compiler.py` with:
  - [x] `54°` Auto-Smooth area-weighted vertex normals & `32`-segment superellipse profiles + graduated fillet caps for smooth `RX-78 VANGUARD` and modular parts.
  - [x] Full-body core & parametric part builders covering all **8 Anatomical Body Zones**:
    1. `head_sensor` (`HP-01`: Cranial 60mm Vulcan Pod & High-Gain Commander Sensor Crest)
    2. `chest_armor` (`HP-09`: FA-78 Chobham Reactive Breastplate & Callidus Chest Mega-Emitter)
    3. `shoulder_pods` (`HP-03/05`: Twin 6-Tube Micro-Missile & Vernier Shoulder Pauldron Pods L+R)
    4. `dorsal_pack` (`HP-00`: Aero Wing Pack, Orbital Space Booster, 380mm Dual Artillery, AWACS Radome)
    5. `propulsion` (`HP-00`: Twin Wing-Binder Psycho-Fin Vernier Array & High-G Vector Binders)
    6. `aux_weapon` (`HP-11`: Ver.Ka Long-Barrel Beam Rifle & Twin-Barrel Heavy Mega Beam Launcher)
    7. `aux_shield` (`HP-07`: Gundarium Hexagonal Composite Shield & Type-C CQC Pile-Bunker Breacher)
    8. `waist_ordnance` (`HP-13/17`: Xiphias-Type Folding Hip Electromagnetic Railguns L+R)
    9. `leg_thrusters` (`HP-24/26`: FA-78 Dual Calf Missile & Supersonic Vernier Booster Pods L+R)
  - [x] `build_agentic_parametric_part(seed_hex, zone_archetype)` generating brand-new, seed-unique 3D `.glb` parts across all 8 body zones on every run.
- [x] Upgrade `src/mecha_engine/zeon_enemies.py` with `58°` Auto-Smooth normals, `38`-segment profiles, 3-step fillet caps, and unoccluded Mono-Eye visor tracks (`Z = +11.4..+11.8 mm`) on `Zaku-II`, `Gouf`, and `Sazabi`.
- [x] Upgrade `src/mecha_engine/catalog_manager.py` with strict zero-duplicate hash/seed/serial enforcement.
- [x] Upgrade `src/mecha_engine/synthesizer.py` with `--spawn N` continuous agentic synthesis across all 8 body zones (compiling **16+ unique non-duplicate `.glb` parts** across all 8 body stations).

### Phase 3 — Hangar 3D Engine, 8-Zone Bone Sockets, Kinematic Stances & UI Polish (`mecha.html`)
- [x] **Unified 4-Unit Gundam-Universe Roster (2×2 Grid):** Merge `Vanguard` & `RX-78` into 1 single smooth Hero Gundam (`⚔️ RX-78 VANGUARD`) alongside `👁️ MS-06Z ZAKU-II`, `⚡ MS-07B GOUF`, and `🔥 MSN-04S SAZABI`.
- [x] **11-Bone Hierarchical Kinematic Rig on ALL 4 Units:** Articulate `pelvis`, `torso`, `head`, `armL`, `forearmL`, `armR`, `forearmR`, `legL`, `shinL`, `legR`, `shinR` driving 6 live 60fps `Combat & Kinetic Stance` animations (`Sentinel`, `RifleBurst`, `ShieldGuard`, `BoostJump`, `MeleeSlash`, `PatrolMarch`) on every Mobile Suit.
- [x] **8-Zone Bone-Parented Gunpla Adapter Sockets & Bilateral Limb Splitting:**
  - [x] `head_sensor` parented to `head`
  - [x] `chest_armor` parented to `torso` (anterior pectoral socket)
  - [x] `shoulder_pods` split L/R and parented to `armL` & `armR`
  - [x] `dorsal_pack` parented to `torso` (posterior dorsal socket)
  - [x] `aux_weapon` parented to `forearmR` (or waist weapon rack when native weapon present)
  - [x] `aux_shield` parented to `forearmL` (compact lateral clamp, zero black box!)
  - [x] `waist_ordnance` parented to `pelvis` (lateral hip skirt sockets)
  - [x] `leg_thrusters` split L/R and parented to `shinL` & `shinR`
- [x] **1-Click `"⚡ FULL ARMOR"` & `"🧬 AGENT SYNTH"` Live Controls + 8-Zone Filter Chips:**
  - [x] Filter parts by body zone (`ALL`, `👑 HEAD`, `🛡️ CHEST`, `🚀 SHOULDER`, `🎒 BACK`, `🔫 WEAPON`, `🔰 SHIELD`, `⚡ WAIST`, `🦵 LEGS`).
  - [x] **`⚡ FULL ARMOR`** button equips a complete 8-zone custom loadout across the entire body with cascading *"KLIK!"* snap-fit assembly!
  - [x] **`🧬 NEW PART`** button synthesizes a brand-new procedural parametric part with a live SHA-256 seed genome directly in the 3D Hangar!
- [x] **Sazabi / Gouf / Zaku Mono-Eye Visibility & Silky Satin PBR:** Fix Mono-Eye cylindrical visor orbit, remove random pixel noise from normal map (`normalScale: 0.07`), and set comfortable studio exposure (`0.90`) & bloom threshold (`1.90`) so `RX-78` white armor never glares.
- [x] **Dedicated `✕` Close Button on EVERY Panel (`#nav-header`, `#left-panel`, `#right-panel`, `#lighting-panel`, `#bottom-bar`, `#guide-overlay`):** Match `office.html` and `constellation.html` close button UX + floating `▼ Top` restore pill when top bar is closed, and fix mobile bottom bar overlap (`#bottom-bar` at `8px`, `#hud-vis-bar` at `84px`).

### Phase 4 — Automated Verification & Deployment
- [x] Run `python -m src.mecha_engine.synthesizer` and `pytest tests/test_mecha_engine.py -v` (100% PASS).
- [x] Verify in headless Chrome (Desktop `1440×900` & Mobile `390×844`) with 0 console errors:
  - [x] `RX-78 VANGUARD` with Full-Body 8-Zone Customization (`Head`, `Chest`, `Shoulders`, `Backpack`, `Weapon`, `Shield`, `Waist Railguns`, `Leg Boosters`) in active `RifleBurst` stance.
  - [x] `MSN-04S SAZABI` close-up showing blazing Emerald Mono-Eye, smooth satin crimson armor, and active combat stance.
  - [x] Mobile `390×844` viewport showing clean non-overlapping bottom bars and `✕` panel close buttons.
- [x] Sync & push both `Codes_Container` and `myonathanlinkedin.github.io`.
