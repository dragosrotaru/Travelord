# travelord

A small TypeScript library and CLI that ranks points of interest along a path of travel. Give it a start point, an end point and a list of coordinates, and it returns the points that lie roughly on the way, ordered by how soon you would reach them.

> Archived: no longer maintained. Kept for reference.

## Why

In 2023 I wanted to plan a train trip across Europe that stopped at as many slacklining and highlining communities as possible, using community locations from [SlackMap](https://slackmap.com/communities). I needed a quick way to keep only the communities on the way and order them along the route.

Most of the code and the original README were written with ChatGPT.

## How it works

1. Compute the bearing and great-circle (haversine) distance from start to end.
2. Drop points whose bearing from the start differs from the travel bearing by more than a threshold (default 45 degrees), or that are farther from the start than the destination is.
3. Score the remaining points with a weighted sum of normalised distance from the start (weight 0.8) and bearing difference (weight 0.2), then sort by score in ascending order.

The helper functions (`calculateDistance`, `calculateBearing`, `calculateBearingDiff`, `filterPoints`, `sortPoints`, and others) are exported and documented with JSDoc.

## Installation

Version 0.1.0 is on npm:

```sh
npm install travelord
```

To use the latest code (0.1.1), build from source:

```sh
pnpm install
pnpm build
node dist/cli.js --help
```

## CLI usage

```sh
travelord --input points.json --start 48.85,2.35 --end 52.52,13.40 --output ranked.json
```

| Option | Description |
| --- | --- |
| `-i, --input <file>` | JSON array of points, each with `lat` and `lng` (required). Other fields are passed through unchanged. |
| `-s, --start <lat,lng>` | Start coordinate (required) |
| `-e, --end <lat,lng>` | End coordinate (required) |
| `-dw, --distance-weight <n>` | Distance weight (default 0.8) |
| `-bw, --bearing-weight <n>` | Bearing weight (default 0.2) |
| `-t, --threshold <deg>` | Maximum bearing difference (default 45) |
| `-o, --output <file>` | Output file. Without it, results go to stdout. |

`test/data.json` contains sample input: anonymised points with only an id, name and coordinates.

## Library usage

```ts
import { rankPointsAlongBearing } from "travelord";

const start = { lat: 0, lng: 0 };
const end = { lat: 10, lng: 10 };
const points = [
    { lat: 2, lng: 2 },
    { lat: 5, lng: 5 },
    { lat: 8, lng: 8 },
];

console.log(rankPointsAlongBearing(start, end, points));
```

## Tech stack

TypeScript, Node, Commander, Jest (ts-jest).

## Development

```sh
pnpm test
```

## License

MIT. See [LICENSE](LICENSE).
