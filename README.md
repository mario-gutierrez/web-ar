# WebXR 6DoF AR Pose Logger

A lightweight WebXR application built with Three.js that renders a 3D object in augmented reality while displaying live device position and orientation coordinates directly on screen.

## Features

* **Real-Time 6DoF Tracking:** Displays position (x, y, z) in meters and rotation (pitch, yaw, roll) in degrees relative to session start.
* **WebXR DOM Overlay:** Retains custom HTML/CSS UI over the live camera feed.
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
   git clone https://github.com/your-username/webxr-ar-demo.git
   cd webxr-ar-demo
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

1. Push `index.html` to your main branch.
2. Go to **Settings** > **Pages**.
3. Select `main` as the source branch and set the folder to `/ (root)`.
4. Access your live app at `https://<your-username>.github.io/<your-repo-name>/`.

## License

MIT