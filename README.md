# Node.js Playout Server

A TypeScript/Node.js-based TV channel playout system that enables seamless streaming of scheduled MP4 and RTMP video files, with support for live broadcasts, frame-accurate scheduling, and HTML5 graphics overlays.

## Features

- Frame-accurate scheduling of MP4 files and RTMP streams
- Smooth transitions between content (crossfade, cut, fade-through-white)
- Live broadcast input support with instant switching
- Real-time HTML5 graphics overlays
- Multi-channel support
- RTMP and MPEG-TS output
- RESTful API for control
- WebSocket support for real-time updates
- Docker deployment ready

## Prerequisites

- Node.js 18 or later
- FFmpeg
- Docker and Docker Compose (for containerized deployment)

## Installation

### Local Development

1. Clone the repository:
   ```bash
   git clone https://github.com/stukenov/node-playout-server.git
   cd node-playout-server
   ```

2. Install dependencies:
   ```bash
   npm install
   ```

3. Build the TypeScript code:
   ```bash
   npm run build
   ```

4. Start the development server:
   ```bash
   npm run dev
   ```

### Docker Deployment

1. Build and start the containers:
   ```bash
   npm run docker:build
   npm run docker:up
   ```

2. Stop the containers:
   ```bash
   npm run docker:down
   ```

## Configuration

Create a `.env` file in the root directory:

```env
NODE_ENV=development
FFMPEG_PATH=/usr/bin/ffmpeg
```

## API Usage

### Create a Channel

```bash
curl -X POST http://localhost:3000/channels \
  -H "Content-Type: application/json" \
  -d '{
    "id": "channel1",
    "name": "Test Channel",
    "outputRtmpUrl": "rtmp://localhost/live/channel1",
    "outputMpegTsUrl": "udp://localhost:1234",
    "overlayEnabled": true
  }'
```

### Schedule Content

```bash
curl -X POST http://localhost:3000/channels/channel1/schedule \
  -H "Content-Type: application/json" \
  -d '{
    "startTime": "2024-03-01T12:00:00Z",
    "sourceType": "file",
    "source": "/media/video1.mp4",
    "duration": 300,
    "transition": "crossfade"
  }'
```

### Switch to Live

```bash
curl -X POST http://localhost:3000/channels/channel1/switchLive \
  -H "Content-Type: application/json" \
  -d '{
    "rtmpUrl": "rtmp://source.com/live/input1"
  }'
```

### Update Overlay

```bash
curl -X PUT http://localhost:3000/channels/channel1/overlay \
  -H "Content-Type: application/json" \
  -d '{
    "type": "ticker",
    "text": "Breaking News: Important Update"
  }'
```

## WebSocket Events

Connect to `ws://localhost:3000/ws` to receive real-time updates about:
- Channel creation/deletion
- Schedule updates
- Live switches
- Overlay changes
- Error events

## Directory Structure

```
src/
  ├── api/          # API server
  ├── core/         # Core playout functionality
  ├── models/       # Data models
  ├── services/     # Business logic
  ├── utils/        # Utility functions
  └── overlays/     # HTML5 overlay renderer
```

## Development

Start the development server with auto-reload:
```bash
npm run dev
```

Start the overlay renderer separately:
```bash
npm run overlay
```

Run tests:
```bash
npm test
```

## Docker Compose Services

- `playout`: Main playout service (ports 3000, 3001)
- `rtmp`: NGINX RTMP server for testing (ports 1935, 8080)

## Performance Considerations

- The system is designed to handle multiple channels efficiently
- Resource limits are configurable in docker-compose.yml
- Frame-accurate switching requires proper hardware resources
- Consider using GPU acceleration for better performance

## Contributing

1. Fork the repository
2. Create your feature branch
3. Commit your changes
4. Push to the branch
5. Create a Pull Request

## License

MIT License - see LICENSE file for details.

Copyright (c) 2025 Saken Tukenov
