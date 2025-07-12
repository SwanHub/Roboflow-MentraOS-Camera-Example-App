# Roboflow-MentraOS-Camera-Example-App

This is a simple example app which demonstrates how to use the MentraOS Camera API to take photos and display them in a webview, then to detect faces using [Roboflow](https://roboflow.com).

## Mentra instructions

### Install MentraOS on your phone

MentraOS install links: [mentra.glass/install](https://mentra.glass/install)

### (Easiest way to get started) Set up ngrok

1. `brew install ngrok`

2. Make an ngrok account

3. [Use ngrok to make a static address/URL](https://dashboard.ngrok.com/)

### Register your App with MentraOS

1. Navigate to [console.mentra.glass](https://console.mentra.glass/)

2. Click "Sign In", and log in with the same account you're using for MentraOS

3. Click "Create App"

4. Set a unique package name like `com.yourName.yourAppName`

5. For "Public URL", enter your Ngrok's static URL

6. In the edit app screen, add the microphone permission

### Get your App running!

1. [Install bun](https://bun.sh/docs/installation)

2. Clone this repo locally: `git clone https://github.com/Mentra-Community/MentraOS-Camera-Example-App`

3. cd into your repo, then type `bun install`

4. Set up your environment variables:

   - Create a `.env` file in the root directory by copying the example: `cp .env.example .env`
   - Edit the `.env` file with your app details:
     ```
     PORT=3000
     PACKAGE_NAME=com.yourName.yourAppName
     MENTRAOS_API_KEY=your_api_key_from_console
     ```
   - Make sure the `PACKAGE_NAME` matches what you registered in the MentraOS Console
   - Get your `API_KEY` from the MentraOS Developer Console
   - The `ROBOFLOW_API_KEY` is used for face detection - get your own from [Roboflow](https://roboflow.com).

5. Run your app with `bun run dev`

6. To expose your app to the internet (and thus MentraOS) with ngrok, run: `ngrok http --url=<YOUR_NGROK_URL_HERE> 3000`
   - `3000` is the port. It must match what is in the app config. For example, if you entered `port: 8080`, use `8080` for ngrok instead.

### Next Steps

Check out the full documentation at [docs.mentra.glass](https://docs.mentra.glass/camera)

## Roboflow & Hackathon instructions

If you are part of the July 12/13th Hackathon, you're eligible for a free month of [Roboflow Basic](https://roboflow.com/pricing) that comes with 30 credits (and more at-will if you run out during the event), full feature functionality (dataset management, model training, low-code workflow builder, deployment) and the chance to win an [NVIDIA Jetson Orin Nano](https://www.nvidia.com/en-us/autonomous-machines/embedded-systems/jetson-orin/nano-super-developer-kit/).

In this example app, we demonstrate basic face inference with an [open-source face detection model found on Roboflow Universe](https://universe.roboflow.com/mohamed-traore-2ekkp/face-detection-mik1i/model/27).

### How it works

1. Photo Capture: When a user takes a photo (via button press or streaming mode), the image is cached locally
2. Face Detection: The `detectFaces()` method in `index.ts` (lines 240-275) sends the image to Roboflow's API:

- Converts the image buffer to base64
- POSTs to `https://serverless.roboflow.com/face-detection-mik1i/27`
- Stores the returned face predictions.

3. Visual Display: The webview (`views/photo-viewer.ejs`) displays the photo with green bounding boxes around detected faces, showing confidence percentages
