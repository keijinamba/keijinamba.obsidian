---
type: programming
date:
links:
  - https://ikorihn.github.io/digitalgarden/note/Haversineの公式をGoで実装する
	- https://github.com/Turfjs/turf/blob/master/packages/turf-distance/index.ts
tags:
  - Programming
related:
in:
  - "[[MOC/Programming]]"
---

# Harversine公式の逆算をする

Haversine公式の逆算をして算出する

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

/**
 * 中心点から指定された距離（km）だけ東西南北に移動した点を計算する
 * 注意: これは近似的な計算。正確な逆算には方位角（bearing）を使った計算が必要
 */
function createBoundingBox(
	center: { lat: number, lng: number },
	distanceKm: number,
): { north: number; south: number; east: number; west: number } {
	const earthRadiusKm = EARTH_RADIUS / 1000;
	
	// 緯度方向の移動（北/南）
	const latDelta = (distanceKm / earthRadiusKm) * (180 / Math.PI);
	
	// 経度方向の移動（東/西）- 緯度によって補正
	const centerLatRad = deg2Rad(center.lat);
	const lngDelta = (distanceKm / earthRadiusKm) * (180 / Math.PI) / Math.cos(centerLatRad);
	
	const north = center.lat + latDelta;
	const south = center.lat - latDelta;
	const east = center.lng + lngDelta;
	const west = center.lng - lngDelta;
	
	return { north, south, east, west };
}
```

**逆算の確認:**
- 緯度方向: 距離dに対して角度差は `d / r` ラジアン = `(d / r) × (180 / π)` 度 ✓
- 経度方向: 緯度φでの経度方向の距離は `d / cos(φ)` で補正 ✓
- この計算は近似的に正しいが、厳密なHaversine公式の逆算ではない（方位角を使った計算が必要）

## Turf.jsでの実装

Turf.jsの`destination`関数も内部的にHaversine公式を使用しており、同等の計算が可能。
https://github.com/Turfjs/turf/blob/master/packages/turf-destination/index.ts

```ts
import { destination } from '@turf/turf';

function turfDestination(
	point1: [number, number], // [lng, lat]
	point2: [number, number], // [lng, lat]
): number {
	return destination(point1, point2, { units: 'kilometers' });
}
```

---
