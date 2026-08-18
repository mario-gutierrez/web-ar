# WebXR and iOS 6DoF AR Pose Logger

A lightweight browser AR demo with two tracking paths:

* `index.html` uses the WebXR Device API for native AR sessions.
* `ios-ar.html` uses the iPhone camera and AlvaAR visual SLAM for browsers where WebXR AR is not available.

Both demos render a 3D reference cube and display live device position and orientation coordinates.

## Features

* **Real-Time 6DoF Tracking:** Displays position (x, y, z) in meters and rotation (pitch, yaw, roll) in degrees relative to session start.
* **WebXR DOM Overlay:** Retains custom HTML/CSS UI over the live camera feed.
* **iOS Camera + SLAM Fallback:** Processes rear-camera frames with AlvaAR and applies its estimated 6DoF pose to a Three.js camera.
* **Zero Build Step:** Runs purely on standard Web APIs with dependencies imported via CDN.

## Compatibility & Requirements

WebXR requires a secure context (**HTTPS**) and device camera permissions.

| Platform / Browser | Support Status | Setup Required |
| :--- | :--- | :--- |
| **Android Chrome** | Native | None (requires HTTPS) |
| **visionOS Safari** | Native | None (requires HTTPS) |
| **iOS Safari** | Experimental | Requires enabling WebXR feature flag |
| **Desktop Browsers** | Emulator Only | Requires WebXR API Emulator extension |

### Enabling WebXR on iOS Safari

Open **Settings** > **Safari** > **Advanced** > **Feature Flags** and toggle **WebXR Device API** to `ON`.

## Quick Start (Local Setup)

To test on a physical mobile device on your local network:

1. Clone the repository:

   ```bash
   git clone https://github.com/mario-gutierrez/web-ar.git
   cd web-ar
   ```

2. Serve files locally and expose an HTTPS tunnel:

   ```bash
   # Start local server
   npx http-server -p 8080

   # Expose port over HTTPS via ngrok
   ngrok http 8080
   ```

3. Open the generated `https://...ngrok-free.app` URL in your mobile browser.

## Deployment

Deploying to **GitHub Pages** fulfills the HTTPS requirement out of the box:

1. Push the project files to your main branch.
2. Go to **Settings** > **Pages**.
3. Select `main` as the source branch and set the folder to `/ (root)`.
4. Access your live app at `https://<your-username>.github.io/<your-repo-name>/`.

## iOS Demo (`ios-ar.html`)

The iOS demo is a camera-based fallback for iPhone and iPad browsers. It does not create a WebXR session. Instead, it combines `getUserMedia()` with AlvaAR's browser SLAM engine:

1. Requests the rear-facing camera at an ideal resolution of 1280x720.
2. Draws each video frame to an offscreen canvas and reads it as `ImageData`.
3. Initializes AlvaAR with the captured frame dimensions.
4. Passes each frame to `alva.findCameraPose()`.
5. Applies the returned 4x4 pose matrix to a Three.js camera and decomposes it into position and Euler rotation values.
6. Renders the camera feed as a `THREE.VideoTexture` with a reference cube overlaid.

Open `ios-ar.html` directly from the deployed HTTPS site. Camera access requires HTTPS, user permission, and a browser that supports `navigator.mediaDevices.getUserMedia()`. The first tracking frames may show `Searching for surface points...` while AlvaAR initializes; move the device slowly across a textured scene to provide visual features. The demo reports `Tracking Active` once a pose is available.

Unlike the WebXR demo, the iOS path does not require enabling Safari's WebXR feature flag. It relies on camera access and AlvaAR's visual tracking instead. The AlvaAR WebAssembly module is bundled inside the local `alva_ar.js` file, so no AlvaAR build is required to run the demo.

## Libraries

| Library | Usage | Repository |
| :--- | :--- | :--- |
| [Three.js](https://github.com/mrdoob/three.js) | 3D scene, camera, renderer, video texture, and pose math | [github.com/mrdoob/three.js](https://github.com/mrdoob/three.js) |
| [AlvaAR](https://github.com/alanross/AlvaAR) | WebAssembly visual SLAM and camera-pose estimation in `ios-ar.html` | [github.com/alanross/AlvaAR](https://github.com/alanross/AlvaAR) |

The WebXR demo imports Three.js `0.160.0` from [unpkg](https://unpkg.com/three@0.160.0/build/three.module.js). The iOS demo uses the same Three.js build and the locally bundled AlvaAR distribution. AlvaAR itself is based on [OV²SLAM](https://github.com/ov2slam/ov2slam) and [ORB-SLAM2](https://github.com/raulmur/ORB_SLAM2).

## License

The demo code is released under the MIT license. The bundled AlvaAR distribution is released under GPLv3; review the upstream [AlvaAR license](https://github.com/alanross/AlvaAR/blob/main/LICENSE) before redistributing the iOS demo.