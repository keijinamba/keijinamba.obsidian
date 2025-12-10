---
type: programming
date:
links:
  - https://ikorihn.github.io/digitalgarden/note/Haversineの公式をGoで実装する
  - https://github.com/Turfjs/turf/blob/master/packages/turf-distance/index.ts
tags:
  - Programming
related:
  - "[[地球上のある点から○km移動した点の座標]]"
in:
  - "[[MOC/Programming]]"
---

# Harversine公式

Haversine公式は、地球上の2点間の最短距離（大円距離）を計算するための公式。

![image|500](https://upload.wikimedia.org/wikipedia/commons/thumb/c/cb/Illustration_of_great-circle_distance.svg/250px-Illustration_of_great-circle_distance.svg.png)

公式：
$$D = 2r \times \arcsin\left(\sqrt{\sin^2\frac{\phi_2 - \phi_1}{2} + \cos(\phi_1) \times \cos(\phi_2) \times \sin^2\frac{\lambda_2 - \lambda_1}{2}}\right)$$

- φ = 緯度、λ = 経度 を表す
- r は地球の赤道半径 正確には 6378137m で計算する

## TypeScript実装例

```ts
// 地球の赤道半径(m)
const EARTH_RADIUS = 6378137;

function deg2Rad(deg: number): number {
	return deg * Math.PI / 180;
}

function haversineDistance(
	point1: { lat: number, lng: number },
	point2: { lat: number, lng: number },
): number {
	const point1Rad = {
		lat: deg2Rad(point1.lat),
		lng: deg2Rad(point1.lng),
	};
	const point2Rad = {
		lat: deg2Rad(point2.lat),
		lng: deg2Rad(point2.lng),
	};
	
	const deltaLat = point2Rad.lat - point1Rad.lat;
	const deltaLon = point2Rad.lng - point1Rad.lng;
	
	const haversine = 
		Math.pow(Math.sin(deltaLat / 2), 2) +
		Math.cos(point1Rad.lat) * Math.cos(point2Rad.lat) * Math.pow(Math.sin(deltaLon / 2), 2);
	
	const distance = EARTH_RADIUS * 2 * Math.asin(Math.sqrt(haversine));
	
	return distance;
}
```

## Turf.jsでの実装

Turf.jsの`distance`関数も内部的にHaversine公式を使用しており、同等の計算が可能。
https://github.com/Turfjs/turf/blob/master/packages/turf-distance/index.ts

```ts
import { distance } from '@turf/turf';

function turfDistance(
	point1: [number, number], // [lng, lat]
	point2: [number, number], // [lng, lat]
): number {
	return distance(point1, point2, { units: 'kilometers' });
}
```

---
