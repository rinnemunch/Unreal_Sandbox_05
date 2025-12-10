# SCP-049 Attacker AI

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
