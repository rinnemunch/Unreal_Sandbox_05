# 🧟 Project 1 – SCP-049 Attacker AI

## 🖼️ Preview

![SCP-049](Media/1.gif)

## 🧱 Features

### **AI Character Setup**

- **BP_SCP_049 Blueprint**
  - Duplicate of the Third Person Character stripped of player input
  - Clean mesh setup using `SKM_Manny_Simple` with `ABP_Manny`
  - Full black silhouette material for the iconic SCP-049 visual
- **NavMesh Configuration**
  - Large-volume Nav Mesh to guarantee stable pathfinding
  - Ensures SCP-049 and SCP-049-2 zombies navigate the level correctly

### **Collision & DeathTouch System**

- **Camera-safe collision setup**
  - Mesh and Capsule ignore camera and visibility traces
  - Prevents clipping and keeps third-person view clean
- **Randomized Voice Line System**
  - Array-based selection of five voice assets
  - Uses Random Array Item before attack montage
- **DeathTouch Event**
  - Plays randomized voice line
  - Triggers attack montage
  - Provides animation-driven callbacks for BT timing

### **Behavior Tree & Blackboard**

- **Blackboard (BB_SCP)**
  - Stores Player reference as Object key
- **Behavior Tree (BT_SCP)**
  - Core chase loop:
    Unfocus → Move To → Focus → Attack → Wait → Repeat
  - Supports animation-driven task completion using dispatcher events

### **AI Controller (AIC_SCP)**

- **PlayerKeyName variable**
  - Ensures Blackboard key names match exactly
- **Automatic Player assignment**
  - Uses Get Player Character on possess
- **Behavior Tree initialization**
  - Delay → Run BT_SCP for stable startup

### **Attack System**

- **Event Dispatcher (OnEnd)**
  - Registered to montage complete + interrupted outputs
  - Notifies Behavior Tree that attack is finished
- **BTT_Attack**
  - Calls DeathTouch
  - Waits for dispatch to finish before returning Success
- **Wait Node**
  - Adds pacing for SCP-049’s attack cycle

### **Focus + Unfocus Tasks**

- **BTT_Focused**
  - Sets AI focus to player during chase
  - Creates intentional, locked-on body rotation
- **BTT_Unfocus**
  - Clears prior focus each loop
  - Prevents direction drift or incorrect rotations

### **Movement Behavior**

- **Max Walk Speed: 650**
  - Faster than player, enabling inevitable pursuit
- **Use Controller Desired Rotation: Enabled**
  - AI turns toward player consistently
- **Orient Rotation to Movement: Disabled**
  - Gives SCP-049 a rigid, eerie movement style

### **Mask + Visual Add-Ons**

- **Plague Doctor Mask (Optional)**
  - Socketed to head and aligned via transform offsets
  - Supports optional bloodless black material override

### **KillZone Instant-Death Trigger**

- **Box Collision attached to hand_r**
  - Precisely positioned for animation-accurate hits
- **Collision Profile**
  - Query-only
  - Overlap only Pawn
- **BP_ThirdPersonCharacter Integration**
  - On overlap → call InstantDeath

### **Death Screen System**

- **WBP_DeathScreen**
  - Background Blur for dramatic fog effect
  - Centered 80pt message with outline
- **InstantDeath Sequence**
  - Camera fade 0 → 1 over 3.5s
  - Add widget to viewport
  - Lock input to UI Only
  - Timer-based restart using Open Level
  - Input reset after reload

### **Wandering SCP-049-2 Zombies**

- **Prebuilt wandering AI included**
  - Drag-and-drop placement into NavMesh
- **Ambient world behavior**
  - Zombies roam while SCP-049 hunts, enhancing atmosphere

## 🚀 Result

A fully functional attacker AI inspired by SCP-049:

- Behavior Tree–driven detection and chase
- Montage-driven attack logic
- Random voice lines
- Instant-death system with cinematic fade and UI
- Optional wandering zombies for immersive worldbuilding

Press Play and watch SCP-049 stalk the player with deliberate, eerie precision.

---

# 😇 Project 2 - Golden Halo Aura (Niagara)

## 🖼️ Preview

![HaloAura](Media/2.gif)

## 🧱 Features

### Post-Process Setup

- **Bloom**
  - Method set to **Convolution**
  - Intensity set to **4**
- **Exposure**
  - Metering Mode set to **Manual**
  - Exposure Compensation set to **1**
  - Physical Camera Exposure disabled
- **Optional Scene Boost**
  - **ExponentialHeightFog** → Fog Density **1.0**
  - **Directional Light** → Intensity **1.0 lux**

---

### Halo Material (M_Halo)

- **Translucent Material**
  - Receives particle color via **Particle Color**
  - Supports emissive control via **Dynamic Parameter** renamed to **Emissive Color**
- **Emissive Setup**
  - Multiply node combining Particle Color and Emissive parameter
- **Diamond Mask**
  - **Diamond Gradient** with falloff constant set to **10**
  - **Power** node for mask shaping
- **Opacity**
  - Multiply of Particle Alpha × Diamond Mask output

---

### Niagara System (NS_Halo)

- **Renderer**
  - Sprite Renderer using **M_Halo**
- **Bounds & Local Space**
  - Fixed bounds: **Min -1000** / **Max +1000**
  - Local Space enabled
- **Spawn & Lifetime**
  - Spawn Rate set to **75**
  - Lifetime range **1.5–2.5**
- **Sprite Size**
  - Uniform Size **40–50**
- **Dynamic Material Params**
  - EmissiveColor set to **50**
- **Shape**
  - Torus shape
    - Large Radius **40**
    - Handle Radius **6**
- **Motion**
  - Velocity From Point → **50**
  - Rotational Velocity → **5** on all axes
  - Gravity removed
- **Color**
  - Random Range Linear Color
    - Min **(1, 0.4, 0)**
    - Max **(3, 2, 0.5)**
- **Scale Sprite Size**
  - Auto-smoothed curve

---

### Character Integration

- **Add Component**
  - Niagara System Component named **Halo**
  - System Asset: **NS_Halo**
- **Attach to Mesh**
  - Socket: **head**
- **Transform Adjustments**
  - Location: **X 29**
  - Rotation: **Y –90**
  - Scale: **0.2 / 0.2 / 0.2**

---

## 🚀 Result

A clean, stylized golden halo aura that glows naturally with bloom, rotates gently, and attaches seamlessly to any character. Ideal for RPG buffs, magical effects, or ambient character auras.

---

# 👣 Project 3 – Synced Footstep Audio System (UE5)

## 🖼️ Preview

![Footsteps](Media/3.gif)

## 🧱 Features

### **Audio Asset Setup**

- **Footstep Sound Pack**
  - Five unique footstep wave files
  - Imported directly into the Content directory
  - Reusable across characters and projects
- **Clean Content Organization**
  - Dedicated **Footsteps** folder
  - Centralized audio assets for easy expansion

---

### **Footstep Sound Cue (SC_Footsteps)**

- **Randomized Playback**
  - Random node selects from five footstep sounds
  - Prevents repetitive audio patterns
- **Natural Variation**
  - Modulator node adds pitch variation
  - Modulator node adds volume variation
- **Reusable Design**
  - Single Sound Cue drives all footstep events
  - Drop-in solution for any animation or character

---

### **Animation Notify System**

- **Run Animation Integration**
  - Uses `MF_Run_Fwd` from the Quinn animation set
  - Works directly with the Third Person Template
- **Precise Footstep Timing**
  - Play Sound notifies placed at exact foot impact frames
  - Matches existing left/right foot timing markers
- **Animation-Driven Audio**
  - Footsteps trigger only when feet contact the ground
  - Fully synced with animation playback

---

### **Beginner-Friendly Workflow**

- **No C++ Required**
  - Blueprint-only implementation
- **No Complex Audio Systems**
  - No MetaSounds or advanced audio routing
- **Expandable Foundation**
  - Easy to add surface types, materials, or context logic later

---

## 🚀 Result

A clean, reliable footstep audio system that brings the Third Person Character to life:

- Perfectly synced footstep sounds
- Natural variation with randomized playback
- Simple Sound Cue–driven workflow
- Fully animation-based timing

Press Play and instantly feel grounded character movement with responsive, professional-quality footstep audio.

---
