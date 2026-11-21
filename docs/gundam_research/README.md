# 🤖 GUNDAM X MECHA ENTHUSIAST
## Autonomous Mecha Engineering Knowledge Base & Technical Blueprint Index
**Classification:** OPEN TECHNICAL RESEARCH & FAN ENTHUSIAST SPECIFICATION  
**Domain:** Autonomous Mobile Suit Synthesis, 3D Procedural Assembly, CAD Tolerances & Mechatronic Knowledge Grounding  
**Target Repository:** `Codes_Container` (`docs/gundam_research/`)

---

## 📑 Master Architecture Overview

Proyek **"Gundam X Mecha Enthusiast"** adalah sistem perancangan mecha otonom (*Agentic Mecha Engineering Engine*) yang memadukan rekayasa mekatronika fiksi ilmiah dengan prinsip-prinsip fisika riil (Newtonian mechanics, material science, high-energy plasma, kinematic joint tolerances, and real-time control systems).

Arsitektur sistem terdiri dari empat komponen utama:
1. **The Autonomous Agentic Synthesis Engine (Backend / Pipeline):**
   - Menggunakan LLM yang dilengkapi kemampuan riset online otonom (*autonomous web retrieval*), pengujian batas invarian struktural, dan guardrails validasi multi-tahap.
   - **Continuous Generation Loop:** Menghasilkan baseline chassis (inner frame / movable frame) serta seluruh modul periferal secara terus-menerus.
   - **Zero Duplicate Invariant ("Gak Ada Satupun Part yang Sama"):** Setiap part memiliki seed entropi unik, permutasi topologi bevel, kanal panel line, dan penomoran serial militer eksklusif.
   - **Strict Mechanical Coherence ("Hasilnya Harus Make Sense"):** Menolak bentuk sederhana/kotak ala Roblox; setiap facet, chamfer, silinder hidrolik, louver pendingin, dan hardpoint dovetail memiliki fungsi mekanis nyata (Master Grade / Ver.Ka depth).
2. **Deterministic Manual Action Trigger (`siyoyo/runner-hub`):**
   - Dieksekusi secara manual via GitHub Action (`workflow_dispatch`) di repositori `https://github.com/siyoyo/runner-hub`.
   - Menjalankan pipeline sintesis, simulasi tabrakan mesh (*anti-clipping clearance sandbox*), dan otomatis commit-push ke repositori publik.
3. **The 3D Mecha WebGL Rendering Engine (Frontend / Web Showcase):**
   - Halaman web interaktif berbasis Three.js (`mecha.html`) yang berfungsi sebagai *Interactive Hangar & Maintenance Dock*.
   - Mengambil data part modular secara dinamis dari repositori publik dan merendernya secara real-time:
     - **Assembled Combat Stance:** Mecha berdiri utuh dengan pose stabil berkat kalkulasi Center of Mass (CoM).
     - **Exploded View Breakdown:** Seluruh pelat zirah terurai di sumbu XYZ memperlihatkan rangka baja dalam.
     - **Dynamic Hydraulic Simulation:** Piston hidrolik bergerak masuk-keluar secara simultan mengikuti sudut tekuk sendi (`.lookAt()` inverse kinematics).
     - **Hot-Swap Modular Mission Packs:** Pengguna dapat mengganti pack (Type-A Aero, Type-S Space, Type-H Artillery, Type-E EW/Recon, Type-C CQC) secara instan.
     - **Holographic HUD Telemetry:** Spesifikasi teknis setiap part saat diklik/diinspeksi.
4. **Ecosystem Discoverability Linkage:**
   - Terhubung langsung ke **GitHub Profile README (`myonathanlinkedin/myonathanlinkedin`)** dan **Main GitHub Pages Portal (`myonathanlinkedin.github.io`)**.

---

## 🗂️ Struktur Folder Knowledge Base & CAD Specifications

Knowledge base ini disusun ke dalam 7 sub-folder teknis dan checklist audit menyeluruh:

```
docs/gundam_research/
├── README.md                                                   <- Master Index & Blueprint Konsep
├── MASTER_MECHA_ENTHUSIAST_IMPLEMENTATION_AND_AUDIT_CHECKLIST.md <- Checklist Audit & Standar Mutu Universal
├── GUNDAM_UNIVERSE_COHESION_BONE_RIGGING_AND_PBR_AUDIT_CHECKLIST.md <- Audit Universe Cohesion, Gunpla Snap-Fit Adapters & PBR Parity
├── FULL_BODY_MODULAR_CUSTOMIZATION_AND_AGENTIC_SYNTHESIS_CHECKLIST.md <- Spesifikasi 8-Zone Full-Body Modular Customization & Agentic Zero-Duplicate Synthesis
├── cad_blueprints_and_3d_print_specs/                          <- Spesifikasi Toleransi CAD & 3D Print Riil
│   ├── CAD_TOLERANCES_AND_PRINTED_JOINT_CLEARANCES.md          <- Clearances Ball Joint (0.1mm), Engsel (0.2mm), Rail (0.4mm)
│   ├── INNER_FRAME_ARTICULATION_GEOMETRY_GUIDE.md              <- Kinematika Sendi Lutut/Siku Ganda, Piston & Sliding Armor
│   ├── F90_STANDARDIZED_HARDPOINT_CAD_SPECIFICATION.md         <- Standar Matriks 26 Hardpoint UHIS, Dovetail & Bus Optik
│   ├── STATE_OF_THE_ART_DYNAMIC_CAD_AND_MODULAR_CONFIGURATORS.md <- Benchmark Industri (OpenSCAD, CadQuery, Dummy 13, Three.js)
│   └── KINEMATIC_MATRICES_AND_DH_CONSTRAINTS.md                <- Formulasi Matriks D-H, Transformasi Homogen & Solusi Anti-Gimbal-Lock
├── 01_chassis_and_movable_frame/
│   ├── MOVABLE_FRAME_AND_STRUCTURAL_MECHANICS.md               <- Konsep Movable Frame, Sendi 6-DoF, Kokpit Panoramik
│   ├── ARMOR_MATERIALS_AND_DEFENSIVE_ALLOYS.md                 <- Gundarium Alloy, Phase Shift, E-Carbon, Anti-Beam Coating
│   └── ENEMY_RIVAL_BASELINE_CHASSIS_ARCHITECTURE.md            <- Frame Musuh / Rival: Mono-Eye 180°, Cowling Melengkung & Hover Jet
├── 02_powerplant_and_reactor_systems/
│   ├── MINOVSKY_ULTRALIGHT_FUSION_REACTOR.md                   <- Reaktor Fusi He-3 + D, I-Field, Minovsky Physics
│   └── ALTERNATIVE_AND_HYPER_POWERPLANTS.md                    <- GN Drive, Hyper-Deuterion, Ultracapacitors
├── 03_propulsion_and_vernier_dynamics/
│   ├── AMBAC_ZERO_G_MOMENTUM_DYNAMICS.md                       <- Hukum Newton III, AMBAC, Manuver Tanpa Propelan
│   └── THERMONUCLEAR_ROCKET_AND_VERNIERS.md                    <- Roket Termonuklir, Apogee Motor, Vernier Array 34-Titik
├── 04_avionics_sensors_and_fire_control/
│   ├── RADOME_OPTICAL_SENSORS_AND_ECM.md                       <- Lingkungan Jamming Minovsky, Sensor Multi-spektral, V-Fin
│   └── OPERATING_SYSTEM_AND_INTERFACE.md                       <- G.U.N.D.A.M. OS, 360 Panoramic Cockpit, Neural Interface
├── 05_armaments_and_tactical_mission_packs/
│   ├── BEAM_AND_DIRECTED_ENERGY_WEAPONRY.md                    <- E-CAP, E-PAC, Mega Particle Cannon, Beam Saber, VSBR
│   ├── BALLISTICS_AND_KINETIC_ARSENAL.md                       <- Railguns, Hyper Bazooka, Micro-Missiles, Cheek CIWS
│   └── MODULAR_HARDPOINTS_AND_MISSION_PACKS.md                 <- Doktrin Mission Packs (Type-A, S, H, E, C, D)
└── 06_agentic_synthesis_and_3d_schema/
    ├── AGENTIC_MECHA_SYNTHESIS_ARCHITECTURE.md                 <- Pipeline Multi-Agent, Guardrails, Validasi Sandboxing
    ├── MECHA_PART_JSON_SCHEMA.md                               <- Skema JSON Otoritatif Part Modular untuk Three.js
    ├── TWO_TIER_PRIVATE_PUBLIC_PIPELINE_AND_DYNAMIC_UI.md      <- Arsitektur Tri-Repo Decoupled & UI Hangar Interaktif Tiga Dimensi
    └── PROCEDURAL_CADQUERY_GENERATION_PIPELINE.md              <- Protokol Code-as-Geometry, OpenCASCADE & Kompresi Draco/Meshopt
```

---

## 🎯 Standar Kualitas Geometri: Anti-"Roblox" Guarantee

Setiap modul yang disintesis oleh agent harus mematuhi aturan topologi industri berikut:
1. **Bevels & Chamfers:** Sudut tajam $90^\circ$ dilarang pada pelat luar. Setiap tepi harus memiliki chamfer $45^\circ \times 0.3 - 0.5 \text{ mm}$ untuk menangkap pantulan cahaya specular dan mencegah delaminasi material.
2. **Panel Line Grooving:** Alur sambungan zirah diukir dengan kedalaman $0.4 \text{ mm}$ dan lebar $0.3 \text{ mm}$, meniru teknik scribing pembuat model profesional (*pro modeler panel lining*).
3. **Piston & Actuator Visibility:** Sendi utama (siku, lutut, pergelangan kaki, dan punggung) wajib menampilkan silinder hidrolik dengan batang piston berlapis krom yang bergerak secara kinematis saat sendi ditekuk.
4. **Heat Vents & Thruster Bells:** Nozel propulsi harus memiliki kontur de Laval bertingkat, cincin sirip pendingin (*heat sink fins*), dan gimbal 2-axis untuk pembelokan vektor dorong.
