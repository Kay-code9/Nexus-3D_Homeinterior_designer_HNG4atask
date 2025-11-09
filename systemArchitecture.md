# System architecture of Nexus - The 3D interior Home designer 

## 1. Overview
Nexus is a  mobile 3D interior design application that enables users to scan or enter room dimensions, auto-generate layouts, and create realistic 3D visualizations and AR previews. Includes platform compatibility (iOS and Android) using a modern tech stack focused on performance, scalability, and ease of maintenance. 

This app emphasizes client-side 3D rendering to provide a seamless, offline-capable experience, with optional cloud integration for collaboration and asset storage. This document outlines the high-level architecture, components, data flow,


# Tech Stack Overview



##  Frontend

**Framework:** `Flutter (Dart)`  
- Cross-platform UI/UX development ensuring native performance on both **iOS** and **Android**.  
- Responsive, smooth, and visually consistent interfaces using **Material (Android)** and **Cupertino (iOS)** design systems.  



##  3D Rendering & AR Engine

**Engine:** `Unity` (integrated via Flutter plugins)  
- Delivers **realistic 3D visualization** using:
  - Ray tracing & physics simulations  
  - Real-time lighting and material reflections (PBR shading)  
- **AR Integration:**
  - **ARKit** for iOS  
  - **ARCore** for Android  
- Uses a **Flutter-Unity bridge** for seamless communication between the UI and rendering layers.



## Backend Services

**Platform:** `Firebase`  
- Authentication (emailor Google) 
- Real-time Database for project sync and collaboration  
- Cloud Storage for images, textures, and small assets  
- Optional **AWS S3** for large 3D model libraries or backups  



## Database

- **Local:** `SQLite` or `Hive` for offline project data, preferences, and cache  
- **Cloud:** `Firestore` for backups, real-time updates, and collaboration sync  



## API Integrations

- **Furniture / Asset Libraries:**  
  Integrates with **Sketchfab API** or custom 3D model endpoints for importing assets.

- **Payment Processing:**  
  Powered by **Paystack** for in-app subscriptions and premium upgrades.



##  Other Core Tools

| Purpose | Tools |
|----------|------------------|
|  State Management | Provider or  Riverpod |
| Testing | Flutter Test, Appium |
|  CI/CD | GitHub Actions  |
| Analytics | Google Analytics  |
|  Error Tracking | Firebase or  Sentry |



# High-Level Components



##  User Interface Layer
- Built entirely with **Flutter widgets**  
- Screens include: Home, Project Editor, Gallery Browser, and Settings  
- Gesture driven interactions:
  - Drag and drop  
  - Pinch to zoom  
  - Pan & rotate in 3D view  



## Logic Layer
- Manages **core design operations**:
  - Room scanning (via camera + LiDAR)  
  - Object placement and transformation  
  - Scene rendering and saving  
- **Offline-first** approach:
  - Local edits stored in SQLite  
  - Syncs with Firestore when online  
- Plugins to use:  
  - `camera`, `AR_flutter_plugin`, and custom Unity bridge  


`
##  Data Layer`
- **Local Cache:**  
  Stores 3D models, user settings, and unsynced changes.  
- **Cloud Sync:**  
  Firebase Realtime Database pushes instant updates using WebSockets.  
- **Conflict Handling:**  
  Timestamp-based merge for real-time collaboration.



## Rendering Engine
- **Unity Engine** handles:
  - 3D scene graph  
  - Lighting, materials, and animations  
  - Export of final renders or 3D walkthroughs  
- **Bridge Communication:**
  - Flutter  Unity via JSON events  
  - Syncs object transforms and user actions  



##  Security & Authentication
- OAuth via **Firebase Authentication** (Email or Social logins)  
- **AES encryption** for local files  
- **HTTPS** enforced for all network calls  
- **Role-based access control:**
  - Basic users: limited saves  
  - Pro users:full library + cloud features  



#  Data Flow



1. **User Input:**  
   Camera feed captured, Processed via ML models (for dimension detection).  
   The app builds a **3D model** of the room in Unity.  

2. **Editing:**  
   Drag and drop updates the scene graph instantly in real time.  

3. **Sync:**  
   On save or share, data is serialized into JSON  Uploaded to Firebase 
   Collaborators receive **push notifications**.  

4. **Offline Handling:**  
   Pending updates queued locally until connection is restored.  
   Optional **PWA mode** via Workbox for web deployments.  

   ### Data Flow Plan
!(images/dataflow 1.png) !(images/dataflow 2.png) 

- !(images/dataflow_3.png)

 `[Link To Dataflow](https://miro.com/app/board/uXjVJ4XIMXQ=/?share_link_id=562397381747)`



# Scalability Considerations



###  Asset Optimization
- Compress 3D models into **GLTF** format  
- Use **lazy loading** for large model libraries  
- Cache commonly used assets locally  

### Performance
- Local caching for quick access  
- Offload heavy renders to **cloud functions**  
- Parallelize texture compression & lighting baking  

###  Horizontal Scaling
- Firebase scales automatically with user base  
- Monitor with **Google Cloud Monitoring & Analytics**  



# Deployment & Maintenance



| Process | Tools |
|----------|----------------|
|  Build | `flutter build apk` |
| CI/CD | GitHub Actions pipelines |
| Versioning | Semantic (`v1.0.0`, `v1.1.0`, ) |
|  Monitoring | Firebase Crashlytics, Sentry |
|  Store Deployment | Automated publish to App Store & Play Store |








## 11. Diagrams and Framework
- 
![From App launch through the Dashboard to Template Browse](images/userflow_1.png)

![Through the "Tour/Template Edit mode/Draw from scratch" to "Drag and drop/add rooms"](images/userflow_2.png)

![Through "Template Edit mode" to "Save design plan/download"](images/userflow_3.png)





 ![Link To UserFlow](https://miro.com/app/board/uXjVJ4XIMXQ=/?share_link_id=562397381747)


## 12. Further Development steps
- Finalize AR implementation choice: **Unity AR Foundation** vs **native ARKit/ARCore** with React Native bridge.  
- Decide on 3D engine: **Three.js (WebGL)** for lightweight client vs **Unity** for advanced Augumented Reality.  
- Define partner data sources for localized price feeds and furniture catalogs.

